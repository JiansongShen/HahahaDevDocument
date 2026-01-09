# Testing Requirements

## 2. Testing Standards (Testing Requirements)

### 2.1 Core Philosophy of Testing

In this project, testing is regarded as a **core engineering asset**, not an additional step after development completion.

Testing design follows these principles:

- Do not pursue "good-looking" coverage numbers
- Focus on **real user usage paths**
- Set **different testing intensity requirements** for different modules
- Clearly define which errors are tolerable and which are not

---

### 2.2 Definition of Core Training Pipeline

The following are defined as the **Core Training Pipeline**:

- Low-level memory allocation and management
- Numerical computation processes
- Computation task distribution and scheduling
- Automatic differentiation (Autograd)
- Optimizer logic
- Model execution-related code

These parts together form a complete training flow. Once an error that cannot be handled correctly occurs, **the entire training process becomes meaningless**.

Therefore, these modules must meet the strictest testing requirements.

---

### 2.3 Tensor Dimension Coverage Strategy (0D–3D)

Based on analysis of user usage scenarios, the project makes the following judgment:

- Users rarely perform complex operations on high-dimensional (>3D) tensors
- The most common in actual use are:
  - 0D (scalar)
  - 1D (vector)
  - 2D (matrix)
  - 3D (common batch tensors)

#### Testing Requirements

- For **every supported operation type**:
  - Even if a logical branch has already been covered
  - Additional tests for **0D–3D** must still be passed
- Testing goals:
  - Eliminate internal implementation uncertainty
  - Ensure consistent and correct behavior within the most commonly used dimension range

---

### 2.4 Data Type Coverage Requirements

In `core/include/definitions`, the project clearly lists:

- All supported operable data types

Testing requirements:

- For every supported data type
- Under all valid dimensions (0D–3D)
- Corresponding operations must be explicitly tested and verified for correctness

---

### 2.5 Testing Framework

The project uniformly uses the following testing tool:

- **Google Test (gtest)**

Mainly used for:

- Unit testing
- Parameterized testing
- Dimension × data type combination testing
- Regression testing

---

### 2.6 Testing Requirements for Non-Core Modules

For non-core modules, such as:

- Logging system
- Utility classes
- Auxiliary components

Complex dimension and mathematical verification are not required, but overall quality must still be ensured.

Current stage requirements:

- **Line coverage ≥ 80%**
- **Branch coverage ≥ 35%**
- Coverage should continue to improve as much as possible on this basis

---

### 2.7 Phased Trade-off Strategy

- Some complex issues that may only manifest in the future:
  - Not mandatory to cover at the current stage
- Avoid over-design leading to:
  - Uncontrolled development costs
  - Excessive testing burden

---

### 2.8 Testing Standards for Future Features

For subsequent new features or plugins:

- The project will **individually formulate testing requirements for each feature**
- Developers need to:
  - Write sufficient tests according to requirements


