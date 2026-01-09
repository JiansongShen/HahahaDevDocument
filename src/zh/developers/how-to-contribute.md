# 如何贡献项目

感谢您对 Hahaha 项目的兴趣！我们欢迎各种形式的贡献，无论是代码、文档、测试还是设计建议。本指南将帮助您快速上手项目开发。

## 贡献前准备

### 1. 了解项目

在开始贡献之前，请先：

- 阅读[项目介绍](../hahaha/introduce.md)了解项目目标和架构
- 查看[路线图](../hahaha/roadmap.md)了解项目规划
- 阅读[技术栈](../hahaha/tech-stack.md)了解使用的技术和工具

### 2. 选择任务

在项目的 `doc/TODO.md` 文件中，找到您感兴趣的任务：

```markdown
- [ ] 实现优化器 (需要有人解决)
```

当您决定贡献时，请在任务后添加您的信息：

```markdown
- [/] 实现优化器 - [您的名称](您的GitHub链接) (您的邮箱)
```

## 开发环境设置

### 系统要求

- **操作系统**：Linux、macOS 或 Windows (WSL)
- **编译器**：GCC 11+、Clang 14+ 或 MSVC 2022+
- **CMake**：3.20+
- **Python**：3.8+ (用于构建脚本和测试)

### 克隆和构建

1. **Fork 项目**
   ```bash
   git clone https://github.com/YOUR_USERNAME/Hahaha.git
   cd Hahaha
   ```

2. **创建开发分支**
   
   在开始开发之前，请**基于dev分支**创建一个新的分支。分支命名应该清晰描述你的工作内容：
   
   ```bash
   # 如果是开发新功能
   git checkout -b feature/[功能名称]-implement
   # 例如：feature/adam-optimizer-implement
   
   # 如果是修复bug
   git checkout -b bug/[bug描述]-fix
   # 例如：bug/tensor-broadcast-fix
   
   # 如果是文档更新
   git checkout -b docs/[文档内容]-update
   # 例如：docs/api-documentation-update
   ```
   
   **分支命名建议**：
   - 使用小写字母和连字符
   - 名称要简洁但具有描述性
   - 避免使用过长的分支名

3. **安装依赖**
   ```bash
   # Ubuntu/Debian
   sudo apt update
   sudo apt install build-essential cmake ninja-build

   # macOS (使用 Homebrew)
   brew install cmake ninja

   # Arch Linux
   sudo pacman -S cmake ninja

   # Fedora/CentOS
   sudo dnf install cmake ninja-build
   # 或者
   sudo yum install cmake ninja-build
   ```

4. **构建项目**
   ```bash
   # 使用 Meson (推荐)
   meson setup builddir
   ninja -C builddir
   
   # 构建完成后，库文件会在 builddir 目录中
   ```

### IDE 配置

推荐使用以下 IDE：
- **CLion**：原生支持 CMake 和 Meson 项目
- **VS Code**：安装 C++ 扩展和 CMake Tools 或者 meson 工具
- **Visual Studio**：支持 CMake 项目

## 代码规范

详细的代码规范请参考：[代码规范文档](code-style.md)

## 提交信息格式

提交信息应该清晰描述变更：

```
类型(范围): 简短描述

详细说明 (可选)

Fixes #123
```

类型包括：
- `feature`: 新功能
- `fix`: 修复bug
- `docs`: 文档更新或注释优化
- `style`: 代码格式调整
- `refactor`: 重构
- `test`: 测试相关
- `chore`: 构建过程或工具配置

## 开发流程

### 1. 创建分支

```bash
# 保持主分支同步（如果已配置upstream）
git checkout dev
git pull upstream dev

# 创建特性分支（使用清晰的命名）
git checkout -b feature/your-feature-name
# 或者修复分支
git checkout -b bug/issue-description-fix
# 或者文档分支
git checkout -b docs/documentation-update
```

### 2. 编写代码

- 遵循必要的[代码规范](code-style.md)
- 添加必要的注释
- 编写单元测试
- 更新相关文档

### 3. 测试

运行测试套件：

```bash
# 使用 Meson 构建并运行测试
meson setup builddir
ninja -C builddir
ninja -C builddir test

# 或者使用 CMake
mkdir cmake-build && cd cmake-build
cmake .. -GNinja
ninja
ctest

# 运行特定测试模块
# 张量相关测试
ninja -C builddir test-tensor
# 自动微分测试
ninja -C builddir test-autograd
# 机器学习测试
ninja -C builddir test-ml
```

### 4. 代码格式化

使用项目的格式化脚本：

```bash
# 格式化代码
./format.sh

# 检查格式
./format.sh --check
```

## 提交 Pull Request

### 1. 推送分支

```bash
# 提交本地更改
git add .
git commit -m "feature: 实现张量加法操作

- 添加 Tensor::add 方法
- 支持广播机制
- 添加相应的单元测试"

# 推送分支
git push origin feature/tensor-add
```

### 2. 创建 PR

在 GitHub 上：

1. 访问您的 fork
2. 点击 "Compare & pull request"
3. 填写 PR 描述：
   - 简要说明变更内容
   - 关联相关 issue（如果有）
   - 描述测试方式
   - @mention 合适的 reviewer

### 3. 代码审查

PR 创建后：

- 等待 CI/CD 检查通过
- 回应 reviewer 的评论
- 根据反馈修改代码
- 获得 approval 后等待合并

## 其他

### 处理已有任务

如果您想完成的任务已经有负责人：

1. 联系任务负责人（通过 issue 或 PR）
2. 讨论任务分工
3. 在负责人分支基础上创建子分支：
   ```bash
   git checkout feature/optimizer-implementation
   git checkout -b feature/optimizer-implementation-adam
   ```

### 贡献类型

除了代码贡献，我们也欢迎：

- **文档改进**：修复文档错误、添加示例
- **测试增强**：提高测试覆盖率、添加边界测试
- **性能优化**：优化算法或内存使用
- **工具改进**：构建脚本、CI/CD 流程
- **设计讨论**：在 ADR 中记录架构决策

### 贡献者认可

您的贡献将被认可：

- 在贡献者列表中添加您的信息
- 在文档中记录您的实现思路
- 参与项目决策讨论

## 📝 记录实现思路的鼓励与建议

### 为什么我们鼓励在文档中记录实现思路？

Hahaha 项目特别强调**教育价值**，我们相信优秀的代码实现如果能伴随着清晰的文档说明，将会产生更大的影响。这不仅有助于其他开发者理解你的工作，更重要的是：

1. **知识传承**：让后来者能够快速理解设计理念和实现细节
2. **社区协作**：帮助团队成员更好地协作和代码审查
3. **学习价值**：为学习者提供算法实现的思考过程和设计决策
4. **维护效率**：当代码需要修改时，文档可以帮助快速理解原有逻辑

记录实现思路是一种值得鼓励的良好实践，它能让你的贡献产生更大的影响力。

### 我们鼓励记录的内容

当你实现新功能时，我们鼓励在相关文档中添加实现思路的记录。

**文档位置说明**：

文档应该放在 `src/<lang>/explains/` 目录(我们提倡您使用英语)下，目录结构应该与代码目录结构相对应。例如：

- 如果您实现或者优化或者修复bug了 `core/include/ml/optimizer/AdamOptimizer.h`，可以在 `src/zh/explains/ml/optimizer.md` 中添加相关内容
- 如果您实现或者优化或者修复bug了 `core/include/math/TensorWrapper.h`，可以在 `src/zh/explains/math/tensor-wrapper.md` 中添加相关内容
- 如果您实现或者优化或者修复bug了 `core/include/compute/graph/ComputeNode.h`，可以在 `src/zh/explains/compute/graph.md` 中添加相关内容

如果对应的文档文件不存在，您可以创建新文件。文档的目录结构应该清晰反映代码的设计思路。

我们完全支持您使用 AI 帮助撰写这些文档，比如您可以通过口头描述您的思路，然后使用 AI 简单润色添加内容（比如不易写的公式等），亦或者不进行润色，保留您的个人风格，我们完全尊重您的选择。

当然，我们更希望看到的是有个人思考和理解的文档，避免仅仅是 AI 的生成内容或难以理解的文档。


#### 1. 新功能实现

我们可以记录：

- **设计思路**：为什么选择这种实现方式？有哪些替代方案？
- **算法原理**：核心算法的数学原理和步骤
- **架构决策**：如何与其他模块交互？接口设计原则是什么？
- **性能考虑**：时间复杂度、空间复杂度分析（如果有的话）
- **边界情况**：特殊输入的处理方式（如果需要的话）

**示例文档结构：**
```markdown
## 功能名称

### 实现思路
[详细描述你的设计思路和决策过程]

### 算法原理
[数学原理和算法步骤]

### 架构设计
[与现有系统的集成方式]

### 性能分析
[复杂度分析和优化点]
```

#### 2. 优化现有功能
当你优化或重构现有代码时，我们鼓励更新相关文档，记录：

- **优化动机**：为什么要进行这次优化？
- **性能提升**：具体的性能改善数据（速度、内存使用等）
- **兼容性影响**：是否影响现有API？如何保持向后兼容？
- **权衡考虑**：优化带来的好处和可能的代价

**示例文档更新：**
```markdown
## 优化记录

### [实际优化出来的功能](您的修改的链接)
- **优化内容**：[具体说明]
- **性能提升**：计算时间减少 30%，内存使用降低 20%
- **实现方式**：[技术细节]
- **兼容性**：保持完全向后兼容
```

#### 3. Bug修复
修复 bug 时，我们鼓励在相关位置记录：

- **问题描述**：bug 的具体表现和影响范围
- **根本原因**：问题产生的根本原因分析
- **修复方案**：为什么选择这种修复方式？
- **测试验证**：如何验证修复的正确性？

### 文档位置建议

建议将文档放在与代码实现相对应的位置，比如：
- 核心算法的相关说明可以放在 `src/<lang>/hahaha/`（例如 `src/zh/hahaha/`）目录
- 架构设计的文档可以放在 `src/<lang>/design/`（例如 `src/zh/design/`）目录
- API 接口的说明可以在头文件注释中，并同步到相关文档

### 一些实践建议

1. **尝试写文档**：实现前先写设计文档，有助于理清思路
2. **结合注释**：重要函数要有详细注释，文档提供高层概览
3. **定期检查**：代码审查时同时检查相关文档的准确性
4. **从小做起**：从为现有功能补充文档开始，逐渐熟悉文档编写

### 记录思路的好处

积极记录实现思路的贡献者往往会获得：

- **更快的代码审查**：清晰的文档有助于 reviewer 快速理解您的实现思路
- **文档与代码同步PR**：您可以将代码实现和文档实现分别提交PR，在代码PR中引用文档PR，审核通过后我们会同时合并它们
- **社区认可**：你的思考过程将成为项目宝贵的知识资产
- **学习机会**：通过文档化加深对实现的理解
- **影响力提升**：你的设计思路可能影响项目的未来发展

我们认为：**好的代码 + 好的文档 = 完美的贡献**！在 Hahaha 项目中，虽然我们不强制要求记录实现思路，但我们真诚地鼓励大家这样做，它能让你的贡献产生更大的价值，也能让整个项目变得更加优秀。📚✨

## 获取帮助

如果您在贡献过程中遇到问题：

- 查看[常见问题解答](../appendix/faq.md)
- 在 GitHub Issues 中提问
- 加入我们的社区讨论

我们致力于让贡献过程愉快而高效！🎉

