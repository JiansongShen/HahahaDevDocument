# Project CI/CD Requirements

*Currently there is no version release CD, only basic documentation CD*. Given that feature implementation is not complete, version releases are not considered for now.

This document only describes CI requirements.

CI uses GitHub Actions for operations.

When PR/push to main/dev branches occurs, full CI is triggered, covering multi-platform, multi-compiler testing, and using gcovr to report test coverage.

Coverage includes:
- meson-ubuntu-x64-gcc
- meson-ubuntu-x64-clang
- meson-ubuntu-arm64-gcc
- meson-ubuntu-arm64-clang
- meson-macos-x64-llvm
- meson-macos-arm64-llvm
- meson-windows-x64-msvc
- cmake-ubuntu-x64-gcc
- cmake-ubuntu-x64-clang
- cmake-macos-arm64-llvm
- cmake-windows-x64-msvc

Coverage requirements:
- line > 80%
- branch > 35%

CI fails when any job fails.

For the future, we will gradually increase test coverage, aiming for line > 90%, branch > 50%.

For CD, there is currently basic CD that only handles Doxygen documentation deployment.


