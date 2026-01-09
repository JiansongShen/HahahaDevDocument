# 项目的cicd要求

*目前没有发版本的cd,仅有普通的文档cd*, 鉴于功能实现并不完全,暂时不考虑发版本.

本文仅描述ci需求

ci使用github actions进行操作

在pr/push到main/dev分支时,会触发全量ci,覆盖多平台多编译器测试,并使用gcovr报告测试覆盖率,

覆盖面有:
meson-ubuntu-x64-gcc
meson-ubuntu-x64-clang
meson-ubuntu-arm64-gcc
meson-ubuntu-arm64-clang
meson-macos-x64-llvm
meson-macos-arm64-llvm
meson-windows-x64-msvc
cmake-ubuntu-x64-gcc
cmake-ubuntu-x64-clang
cmake-macos-arm64-llvm
cmake-windows-x64-msvc

覆盖率要求:
line > 80%
branch > 35%

在其中任意job失败时,ci失败

对于日后我们会逐渐增加测试量,尽量追求line > 90%, branch > 50%

对于cd,现在有基础的cd仅做doxygen文档部署

