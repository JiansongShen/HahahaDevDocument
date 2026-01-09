
## 代码规范

### C++ 编码标准

我们遵循现代 C++ 最佳实践：

- **语言版本**：C++23
- **命名约定**：
  - 类和结构体：`PascalCase`
  - 函数和变量：`pascalCase`
  - 常量：`PascalCase`
  - macro：`PascalCase_`
  - 成员：无特殊要求，但需必要的 getter 和 setter

- **代码风格**：
  - 使用 4 个空格缩进
  - 行长度限制在最长 100 字符以内
  - 每个函数都有合适的注释
  - 函数接口等应有完善 doxygen 注释

### 示例代码

```cpp
// 正确的命名和风格
/**
* @brief 描述 Tensor 类
*/
class Tensor {
public:
    // 构造函数
    /**
    * @brief 构造一个张量
    * @param shape 张量的形状
    * @param dtype 张量的数据类型
    */
    Tensor(const std::vector<size_t>& shape, DataType dtype = DataType::Float32);

    // 公共方法, 此处省略注释
    Tensor add(const Tensor& other) const;
    Tensor multiply(const Tensor& other) const;

private:
    std::vector<size_t> shape_; ///< 张量的形状
    DataType dtype_;    ///< 张量的数据类型
    std::shared_ptr<TensorData> data_; ///< 张量的数据
};
```

more...
