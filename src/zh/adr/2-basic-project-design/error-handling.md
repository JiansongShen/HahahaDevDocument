# 错误处理规范

## 状态
已接受 (Accepted)

## 背景

Hahaha 项目作为教育型机器学习库，需要处理各种运行时错误。错误处理机制的设计直接影响：
- **用户体验**：错误信息是否清晰、可理解
- **代码健壮性**：系统能否优雅地处理异常情况
- **教育价值**：错误处理模式是否体现现代 C++ 最佳实践

在 C++23 标准中，`std::expected` 提供了类型安全的错误处理机制，相比传统的异常或错误码方式，更适合函数式编程风格。

## 决策

我们采用**分层错误处理策略**，根据错误的可恢复性选择不同的处理方式：

### 错误分类

#### 1. 可恢复错误（Recoverable Errors）

可恢复错误是指**不影响整体流程继续执行**的错误，通常可以通过重试、跳过或降级处理来恢复。

**典型场景**：
- 训练过程中某个 batch 训练失败，可以跳过该 batch 继续训练下一个
- 数据加载时某个样本损坏，可以跳过该样本继续加载
- 网络请求失败，可以重试或使用缓存数据
- 文件读取时权限不足，可以尝试其他路径或使用默认配置

#### 2. 不可恢复错误（Unrecoverable Errors）

不可恢复错误是指**导致当前操作无法继续执行**的错误，通常表示程序逻辑错误或系统状态异常。

**典型场景**：
- 张量维度不匹配且无法进行广播（broadcast）
- 内存分配失败
- 除零操作
- 空指针解引用
- 不支持的张量操作组合

### 处理机制

#### 可恢复错误：使用 `std::expected`

对于可恢复错误，使用 C++23 的 `std::expected<T, E>` 类型：

```cpp
#include <expected>
#include <string>

// 示例：训练单个 batch
std::expected<void, std::string> trainBatch(const Tensor& batch) {
    // 训练逻辑
    if (/* 训练失败 */) {
        return std::unexpected("Batch training failed: invalid data");
    }
    return {};
}

// 使用方式
auto result = trainBatch(batch);
if (!result) {
    // 可恢复错误，记录日志并继续
    logger.warn("Skipping batch: {}", result.error());
    continue; // 继续下一个 batch
}
```

**优势**：
- **类型安全**：编译时检查错误处理
- **性能友好**：零开销抽象，不涉及异常机制
- **函数式风格**：支持链式调用和组合
- **明确语义**：返回值明确表示可能失败的操作

#### 不可恢复错误：使用异常（Exceptions）

对于不可恢复错误，使用 C++ 标准异常机制：

```cpp
// 示例：张量运算
Tensor add(const Tensor& a, const Tensor& b) {
    // 检查维度兼容性
    if (!canBroadcast(a.shape(), b.shape())) {
        throw std::invalid_argument(
            "Tensor shapes incompatible: " +
            shapeToString(a.shape()) + " vs " +
            shapeToString(b.shape())
        );
    }
    
    // 检查内存分配
    if (/* 内存不足 */) {
        throw std::bad_alloc();
    }
    
    // 执行运算
    return performAdd(a, b);
}
```

**优势**：
- **自动传播**：错误自动向上传播，无需手动检查
- **清晰语义**：异常表示"不应该发生"的情况
- **堆栈信息**：配合 C++23 的 `std::stacktrace` 提供完整的调用链

### 错误类型定义

项目将定义统一的错误类型：

```cpp
namespace hahaha::error {

// 可恢复错误类型（待定义）
enum class RecoverableError {
    BatchTrainingFailed,
    DataLoadFailed,
    NetworkRequestFailed,
    FileAccessDenied,
    // ...
};

// 使用 expected 包装
template<typename T>
using Result = std::expected<T, RecoverableError>;

// 不可恢复错误：将定义自定义异常类型（待实现）
// 目前使用标准库异常，如 std::invalid_argument, std::bad_alloc 等

} // namespace hahaha::error
```

## 实践指南

### 1. 函数签名设计

```cpp
// ✅ 可恢复错误：使用 expected
std::expected<Tensor, std::string> loadDataset(const std::string& path);

// ✅ 不可恢复错误：可能抛出异常
Tensor matmul(const Tensor& a, const Tensor& b); // 可能抛出异常

// ❌ 避免混用：不要在 expected 返回的函数中抛出异常
```

### 2. 错误传播

```cpp
// expected 的错误传播
auto result = step1()
    .and_then([](auto val) { return step2(val); })
    .and_then([](auto val) { return step3(val); });

if (!result) {
    // 统一处理错误
    handleError(result.error());
}

// 异常的传播：异常出现时一般是panic, 不错误处理
```

### 3. 错误信息规范

- **可恢复错误**：提供清晰的错误描述和建议的恢复操作
- **不可恢复错误**：提供详细的上下文信息，包括相关参数值、状态信息

```cpp
// ✅ 好的错误信息
return std::unexpected(
    "Failed to load batch: file corrupted at line 42. "
    "Skipping this batch and continuing."
);

// ❌ 不好的错误信息
return std::unexpected("Error");
```

### 4. 边界情况处理

- **核心训练链路**：严格检查，不可恢复错误立即抛出
- **辅助功能**：可以容忍部分失败，使用可恢复错误

## 影响

### 正面影响

1. **类型安全**：编译时检查错误处理，减少运行时错误
2. **性能优化**：`std::expected` 零开销，异常仅用于真正异常情况
3. **代码清晰**：错误处理语义明确，易于理解和维护
4. **教育价值**：展示现代 C++ 错误处理最佳实践

### 注意事项

1. **编译器要求**：需要支持 C++23 的 `std::expected`（GCC 13+, Clang 16+）
2. **学习曲线**：贡献者需要理解 `std::expected` 的使用模式
3. **错误分类**：需要明确区分可恢复和不可恢复错误，这需要经验和判断

#### 机器学习场景中的错误分类示例

为了帮助开发者更好地理解错误分类，以下列举了机器学习项目中常见的错误类型：

**可恢复错误示例**：

1. **数据加载相关**
   - 某个数据文件损坏或格式错误 → 跳过该文件，继续加载其他文件
   - 数据预处理时某个样本包含无效值（NaN/Inf）→ 跳过该样本，记录警告
   - 数据文件路径不存在 → 尝试备用路径或使用默认数据集
   - 数据文件权限不足 → 尝试其他路径或使用缓存数据

2. **训练过程相关**
   - 某个 batch 训练时梯度爆炸 → 跳过该 batch，继续下一个
   - 某个 batch 的损失值异常（NaN）→ 跳过该 batch，记录日志
   - 优化器更新时学习率过小导致无更新 → 调整学习率后重试
   - 验证集评估时某个指标计算失败 → 跳过该指标，继续其他指标

3. **模型推理相关**
   - 推理时输入数据格式不匹配 → 尝试自动转换或使用默认值
   - 模型输出包含异常值 → 进行后处理修正或使用默认输出
   - 推理服务暂时不可用 → 使用缓存结果或降级服务

4. **资源管理相关**
   - GPU 内存不足（单个 batch）→ 减小 batch size 后重试
   - 临时文件创建失败 → 使用内存缓存或尝试其他位置
   - 网络请求超时 → 重试或使用本地缓存

**不可恢复错误示例**：

1. **张量运算相关**
   - 矩阵乘法时维度不匹配且无法广播 → 抛出异常
   - 张量 reshape 时元素总数不匹配 → 抛出异常
   - 张量索引越界 → 抛出异常（可使用 `std::out_of_range`）
   - 不支持的数据类型组合运算 → 抛出异常

2. **模型结构相关**
   - 模型层数配置错误导致前向传播无法执行 → 抛出异常
   - 模型参数未初始化就进行前向传播 → 抛出异常
   - 模型保存/加载时版本不兼容 → 抛出异常

3. **计算图相关**
   - 计算图构建时出现循环依赖 → 抛出异常
   - 自动微分时遇到不支持的操作 → 抛出异常
   - 反向传播时梯度计算失败（核心逻辑错误）→ 抛出异常

4. **系统资源相关**
   - 系统内存完全耗尽，无法分配张量 → 抛出异常（可使用 `std::bad_alloc`）
   - GPU 设备不可用且无法回退到 CPU → 抛出异常
   - 关键文件损坏且无备份 → 抛出异常

5. **数值计算相关**
   - 除零操作（如归一化时标准差为 0）→ 抛出异常
   - 矩阵求逆时矩阵奇异（不可逆）→ 抛出异常
   - 数值溢出导致结果无效 → 抛出异常

**判断原则**：

- **可恢复**：错误发生在**数据层面**或**单个操作**，不影响整体流程，可以通过跳过、重试、降级等方式继续
- **不可恢复**：错误发生在**核心计算逻辑**、**模型结构**或**系统资源**层面，表示程序逻辑错误，必须立即停止并修复

**示例代码对比**：

```cpp
// ✅ 可恢复错误：使用 expected
std::expected<Tensor, std::string> loadBatch(const std::string& path) {
    if (/* 文件损坏 */) {
        return std::unexpected("Batch file corrupted, skipping");
    }
    return loadFromFile(path);
}

// ✅ 不可恢复错误：抛出异常
Tensor matmul(const Tensor& a, const Tensor& b) {
    if (!canMatmul(a.shape(), b.shape())) {
        throw std::invalid_argument(
            "Cannot multiply: " + shapeToString(a.shape()) + 
            " × " + shapeToString(b.shape())
        );
    }
    return performMatmul(a, b);
}
```

## 相关决策

- [ADR-0001：使用 C++23 现代特性](../1-cpp23/0001-cpp23.md) - `std::expected` 是选择 C++23 的原因之一
- [测试要求](test-requirement.md) - 错误处理需要相应的测试覆盖