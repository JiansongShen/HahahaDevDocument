# Project Basic Structure

## I. Overall Design Overview

The project adopts a **four-layer architecture design**, abstracting layer by layer from bottom to top, extending from hardware and computation scheduling to upper-level public interfaces and application layers. Each layer has clear responsibilities and well-defined boundaries.

---

## II. Four-Layer Division

### Layer 1: Low-Level Computation and Hardware Abstraction Layer

**Core Responsibilities**:  
Direct interaction with low-level hardware and computational resources, providing unified computation and device abstraction.

**Key Components**:
- `Compute Dispatcher`
- `Device` abstract class
- Low-level hardware interface interaction layer

**Characteristics**:
- Shields differences between different hardware implementations  
- Provides unified, extensible computation scheduling capabilities for upper layers  

---

### Layer 2: Data Abstraction and Data Operation Layer

**Core Responsibilities**:  
Abstracts computational data and provides intermediate-level data operation capabilities.

**Key Components**:
- `Tensor`
- `Tensor Operator`
- `Linear Algebra Operator`
- Various intermediate-level data operation classes

**Characteristics**:
- Unified data representation  
- Handles operations and transformations of tensors and structured data  
- Acts as a bridge between computation and algorithm layers  

---

### Layer 3: Machine Learning and Computation Graph Core Layer

**Core Responsibilities**:  
Builds high-level computation and learning capabilities, serving as the project's core logic layer.

**Key Components**:
- Machine Learning module  
- Deep Learning module  
- Dataset support system  
- Computation Graph
- Core computation structures such as DA graphs

**Characteristics**:
- Responsible for model construction, training, and inference logic  
- Unifies core mechanisms such as computation graphs and automatic differentiation  

---

### Layer 4: Public Interface and Upper Application Layer

**Core Responsibilities**:  
Provides unified, easy-to-use methods and interfaces externally, targeting public internet or upper-level applications.

**Key Components**:
- Method-level operation interfaces  
- Public internet-facing access and invocation layer

**Characteristics**:
- Provides stable APIs  
- Shields internal complex implementations  
- Supports integration with upper-level applications and external systems  

---

## III. Summary

The project achieves the following through the four-layer structure:
- **Bottom layer decouples hardware**
- **Middle layer unifies data**
- **Core layer focuses on machine learning and computation graphs**
- **Top layer provides public interfaces and application capabilities**

The overall design has good **extensibility, maintainability, and engineering clarity**.

The project is built using Meson/CMake.

