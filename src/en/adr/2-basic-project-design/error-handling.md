# Error Handling Guidelines

## Status
Accepted

## Context

As an educational machine learning library, the Hahaha project needs to handle various runtime errors. The design of error handling mechanisms directly affects:
- **User experience**: Whether error messages are clear and understandable
- **Code robustness**: Whether the system can gracefully handle exceptional situations
- **Educational value**: Whether error handling patterns reflect modern C++ best practices

In the C++23 standard, `std::expected` provides a type-safe error handling mechanism that is more suitable for functional programming style compared to traditional exceptions or error codes.

## Decision

We adopt a **layered error handling strategy**, choosing different handling methods based on error recoverability:

### Error Classification

#### 1. Recoverable Errors

Recoverable errors are those that **do not prevent the overall process from continuing**, and can usually be recovered through retry, skip, or fallback handling.

**Typical scenarios**:
- A batch training failure during training can skip that batch and continue with the next
- A corrupted sample during data loading can skip that sample and continue loading
- Network request failures can retry or use cached data
- Insufficient file permissions can try alternative paths or use default configurations

#### 2. Unrecoverable Errors

Unrecoverable errors are those that **prevent the current operation from continuing**, usually indicating program logic errors or abnormal system state.

**Typical scenarios**:
- Tensor dimension mismatch that cannot be broadcast
- Memory allocation failure
- Division by zero
- Null pointer dereference
- Unsupported tensor operation combinations

### Handling Mechanisms

#### Recoverable Errors: Using `std::expected`

For recoverable errors, use C++23's `std::expected<T, E>` type:

```cpp
#include <expected>
#include <string>

// Example: training a single batch
std::expected<void, std::string> trainBatch(const Tensor& batch) {
    // Training logic
    if (/* training failed */) {
        return std::unexpected("Batch training failed: invalid data");
    }
    return {};
}

// Usage
auto result = trainBatch(batch);
if (!result) {
    // Recoverable error, log and continue
    logger.warn("Skipping batch: {}", result.error());
    continue; // Continue to next batch
}
```

**Advantages**:
- **Type safety**: Compile-time checking of error handling
- **Performance friendly**: Zero-overhead abstraction, no exception mechanism involved
- **Functional style**: Supports chaining and composition
- **Clear semantics**: Return value clearly indicates operations that may fail

#### Unrecoverable Errors: Using Exceptions

For unrecoverable errors, use C++ standard exception mechanism:

```cpp
// Example: tensor operations
Tensor add(const Tensor& a, const Tensor& b) {
    // Check dimension compatibility
    if (!canBroadcast(a.shape(), b.shape())) {
        throw std::invalid_argument(
            "Tensor shapes incompatible: " +
            shapeToString(a.shape()) + " vs " +
            shapeToString(b.shape())
        );
    }
    
    // Check memory allocation
    if (/* insufficient memory */) {
        throw std::bad_alloc();
    }
    
    // Perform operation
    return performAdd(a, b);
}
```

**Advantages**:
- **Automatic propagation**: Errors automatically propagate upward without manual checking
- **Clear semantics**: Exceptions represent situations that "should not happen"
- **Stack information**: Combined with C++23's `std::stacktrace` to provide complete call chain

### Error Type Definitions

The project will define unified error types:

```cpp
namespace hahaha::error {

// Recoverable error types (to be defined)
enum class RecoverableError {
    BatchTrainingFailed,
    DataLoadFailed,
    NetworkRequestFailed,
    FileAccessDenied,
    // ...
};

// Wrap with expected
template<typename T>
using Result = std::expected<T, RecoverableError>;

// Unrecoverable errors: custom exception types will be defined (to be implemented)
// Currently using standard library exceptions such as std::invalid_argument, std::bad_alloc, etc.

} // namespace hahaha::error
```

## Practical Guidelines

### 1. Function Signature Design

```cpp
// ✅ Recoverable errors: use expected
std::expected<Tensor, std::string> loadDataset(const std::string& path);

// ✅ Unrecoverable errors: may throw exceptions
Tensor matmul(const Tensor& a, const Tensor& b); // may throw exceptions

// ❌ Avoid mixing: don't throw exceptions in functions returning expected
```

### 2. Error Propagation

```cpp
// Error propagation with expected
auto result = step1()
    .and_then([](auto val) { return step2(val); })
    .and_then([](auto val) { return step3(val); });

if (!result) {
    // Unified error handling
    handleError(result.error());
}

// Exception propagation: exceptions generally indicate panic, no error handling
```

### 3. Error Message Standards

- **Recoverable errors**: Provide clear error descriptions and suggested recovery actions
- **Unrecoverable errors**: Provide detailed context information, including relevant parameter values and state information

```cpp
// ✅ Good error message
return std::unexpected(
    "Failed to load batch: file corrupted at line 42. "
    "Skipping this batch and continuing."
);

// ❌ Bad error message
return std::unexpected("Error");
```

### 4. Edge Case Handling

- **Core training pipeline**: Strict checking, unrecoverable errors thrown immediately
- **Auxiliary functions**: Can tolerate partial failures, use recoverable errors

## Consequences

### Positive Impacts

1. **Type safety**: Compile-time checking of error handling, reducing runtime errors
2. **Performance optimization**: `std::expected` has zero overhead, exceptions only for truly exceptional cases
3. **Code clarity**: Error handling semantics are clear, easy to understand and maintain
4. **Educational value**: Demonstrates modern C++ error handling best practices

### Considerations

1. **Compiler requirements**: Need support for C++23's `std::expected` (GCC 13+, Clang 16+)
2. **Learning curve**: Contributors need to understand `std::expected` usage patterns
3. **Error classification**: Need to clearly distinguish recoverable and unrecoverable errors, which requires experience and judgment

#### Error Classification Examples in Machine Learning Scenarios

To help developers better understand error classification, the following lists common error types in machine learning projects:

**Recoverable Error Examples**:

1. **Data Loading Related**
   - A data file is corrupted or has format errors → Skip the file, continue loading other files
   - A sample contains invalid values (NaN/Inf) during data preprocessing → Skip the sample, log a warning
   - Data file path does not exist → Try alternative paths or use default dataset
   - Insufficient data file permissions → Try other paths or use cached data

2. **Training Process Related**
   - Gradient explosion during a batch training → Skip the batch, continue with the next
   - Loss value is abnormal (NaN) for a batch → Skip the batch, log it
   - Learning rate too small during optimizer update causing no update → Adjust learning rate and retry
   - A metric calculation fails during validation set evaluation → Skip the metric, continue with other metrics

3. **Model Inference Related**
   - Input data format mismatch during inference → Try automatic conversion or use default values
   - Model output contains abnormal values → Post-process correction or use default output
   - Inference service temporarily unavailable → Use cached results or degraded service

4. **Resource Management Related**
   - GPU memory insufficient (for a single batch) → Reduce batch size and retry
   - Temporary file creation failure → Use memory cache or try other locations
   - Network request timeout → Retry or use local cache

**Unrecoverable Error Examples**:

1. **Tensor Operations Related**
   - Dimension mismatch during matrix multiplication that cannot be broadcast → Throw exception
   - Element count mismatch during tensor reshape → Throw exception
   - Tensor index out of bounds → Throw exception (can use `std::out_of_range`)
   - Unsupported data type combination operations → Throw exception

2. **Model Structure Related**
   - Model layer configuration error preventing forward propagation → Throw exception
   - Forward propagation attempted with uninitialized model parameters → Throw exception
   - Version incompatibility during model save/load → Throw exception

3. **Computation Graph Related**
   - Circular dependency during computation graph construction → Throw exception
   - Unsupported operation encountered during automatic differentiation → Throw exception
   - Gradient computation failure during backpropagation (core logic error) → Throw exception

4. **System Resources Related**
   - System memory completely exhausted, unable to allocate tensors → Throw exception (can use `std::bad_alloc`)
   - GPU device unavailable and cannot fallback to CPU → Throw exception
   - Critical file corrupted with no backup → Throw exception

5. **Numerical Computation Related**
   - Division by zero (e.g., standard deviation is 0 during normalization) → Throw exception
   - Matrix is singular (non-invertible) during matrix inversion → Throw exception
   - Numerical overflow causing invalid results → Throw exception

**Judgment Principles**:

- **Recoverable**: Errors occur at the **data level** or **single operation** level, do not affect the overall process, can continue through skip, retry, fallback, etc.
- **Unrecoverable**: Errors occur at the **core computation logic**, **model structure**, or **system resource** level, indicating program logic errors that must be stopped immediately and fixed

**Example Code Comparison**:

```cpp
// ✅ Recoverable error: use expected
std::expected<Tensor, std::string> loadBatch(const std::string& path) {
    if (/* file corrupted */) {
        return std::unexpected("Batch file corrupted, skipping");
    }
    return loadFromFile(path);
}

// ✅ Unrecoverable error: throw exception
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

## Related Decisions

- [ADR-0001: Adopt Modern C++23 Features](../1-cpp23/0001-cpp23.md) - `std::expected` is one of the reasons for choosing C++23
- [Testing Requirements](test-requirement.md) - Error handling requires corresponding test coverage


