## Coding Standards

### C++ Coding Standards

We follow modern C++ best practices:

- **Language version**: C++23
- **Naming conventions**:
  - Classes and structs: `PascalCase`
  - Functions and variables: `pascalCase`
  - Constants: `PascalCase`
  - Macros: `PascalCase_`
  - Members: no special requirements, but necessary getters and setters are needed

- **Code style**:
  - Use 4 spaces for indentation
  - Line length should be limited to a maximum of 100 characters
  - Each function should have appropriate comments
  - Function interfaces should have complete Doxygen comments

### Example Code

```cpp
// Correct naming and style
/**
* @brief Describes the Tensor class
*/
class Tensor {
public:
    // Constructor
    /**
    * @brief Construct a tensor
    * @param shape The shape of the tensor
    * @param dtype The data type of the tensor
    */
    Tensor(const std::vector<size_t>& shape, DataType dtype = DataType::Float32);

    // Public methods, comments omitted here
    Tensor add(const Tensor& other) const;
    Tensor multiply(const Tensor& other) const;

private:
    std::vector<size_t> shape_; ///< The shape of the tensor
    DataType dtype_;    ///< The data type of the tensor
    std::shared_ptr<TensorData> data_; ///< The data of the tensor
};
```

More details to be added...

