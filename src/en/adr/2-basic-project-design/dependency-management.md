# Project Dependency Management

Dependencies are divided into two types:

- **Strong system-related dependencies**: such as OpenGL, GLFW, Vulkan, CUDA, etc., which need to interact with the underlying system, fall into this category
- **Library dependencies**: such as ImGui, GTest, etc., which do not need to interact with the underlying system, fall into this category

For strong system-related dependencies, installation is done through system package management.

For library dependencies:

- Meson installs via wrap
- CMake installs via installation scripts

