# Implementation Documentation

This directory is for documenting implementation reasoning, design decisions, and algorithm principles. These documents are an important part of the educational value of the Hahaha project.

## 📁 Directory Structure

The structure of the `explains/` directory should correspond to the code directory structure:

```
explains/
├── index.md              # This file
├── math/                 # Corresponds to core/include/math/
│   ├── tensor.md        # Tensor implementation reasoning
│   └── tensor-wrapper.md
├── compute/              # Corresponds to core/include/compute/
│   ├── graph.md         # Computational graph implementation reasoning
│   └── autograd.md      # Automatic differentiation implementation reasoning
├── ml/                   # Corresponds to core/include/ml/
│   ├── optimizer/       # Optimizer implementation reasoning
│   │   ├── sgd.md
│   │   └── adam.md
│   ├── model/           # Model implementation reasoning
│   │   ├── linear.md
│   │   └── knn.md
│   └── loss/            # Loss function implementation reasoning
│       └── mse.md
├── backend/             # Corresponds to core/include/backend/
│   ├── device.md
│   └── vectorize.md
└── display/             # Corresponds to core/include/display/
    └── visualizer.md
```

## ✍️ Suggested Content

Each implementation documentation can include the following:

### 1. Feature Overview
- What does this feature/module do?
- What problem does it solve?

### 2. Design Reasoning
- Why was this implementation approach chosen?
- What alternatives existed? Why weren't they chosen?
- What factors were considered during design?

### 3. Algorithm Principles
- Mathematical principles of the core algorithm
- Detailed explanation of algorithm steps
- Key formulas and derivations (if applicable)

### 4. Implementation Details
- Explanation of key data structures
- Implementation logic of important functions
- How edge cases are handled

### 5. Performance Considerations
- Time complexity analysis
- Space complexity analysis
- Optimization points and trade-offs

### 6. Usage Examples
- Simple code examples
- Common usage scenarios

## 📝 Writing Guidelines

### Document Naming
- Use lowercase letters and hyphens
- File names should clearly describe the content
- Examples: `adam-optimizer.md`, `tensor-broadcast.md`

### Document Format
- Use Markdown format
- Use code blocks, formulas, and diagrams appropriately
- Keep structure clear, use heading hierarchy to organize content

### Content Requirements
- **Clear and understandable**: express ideas in concise, clear language
- **Logically complete**: include necessary background information and reasoning
- **Rich examples**: provide code examples or pseudocode
- **Personal thinking**: reflect your design reasoning and understanding

### AI Assistance
We fully support using AI to help write documentation, for example:
- Use AI to polish your verbal descriptions
- Use AI to generate mathematical formulas
- Use AI to check grammar and formatting

However, we prefer to see:
- Documentation with personal thinking and understanding
- Reflection of your design decision process
- Avoid purely AI-generated content

## 🔗 Related Links

- [How to Contribute](../developers/how-to-contribute.md) - Learn how to write implementation documentation
- [Code Style](../developers/code-style.md) - Learn about coding standards
- [Architecture Design](../design/) - View system-level design documents

## 💡 Example

Here is an example structure for an implementation documentation:

```markdown
# Adam Optimizer Implementation Reasoning

## Feature Overview

Adam (Adaptive Moment Estimation) is an adaptive learning rate optimization algorithm...

## Design Reasoning

### Why Adam?

Adam combines the advantages of Momentum and RMSprop...

### Implementation Approach

We chose to implement using template classes to support different data types...

## Algorithm Principles

### Mathematical Formulas

Adam's core formulas include:
- First moment estimate: ...
- Second moment estimate: ...
- Parameter update: ...

### Algorithm Steps

1. Initialize...
2. Compute gradients...
3. Update parameters...

## Implementation Details

### Key Data Structures

```cpp
class AdamOptimizer {
    // ...
};
```

### Important Functions

- `step()`: Execute one optimization step
- `zeroGrad()`: Zero out gradients

## Performance Considerations

- Time complexity: O(n), where n is the number of parameters
- Space complexity: O(n), need to store momentum and variance

## Usage Examples

```cpp
auto optimizer = AdamOptimizer(parameters, 0.001);
optimizer.zeroGrad();
loss.backward();
optimizer.step();
```
```

---

**Remember**: Good implementation documentation not only helps others understand the code, but also helps you better understand design decisions. Let's build a valuable knowledge base together! 📚✨
