# 本地预览

## 安装 mdBook

mdBook 是一个 Rust 工具，推荐用 `cargo` 安装：

```bash
cargo install mdbook --locked
```

如果你还没有 Rust 工具链，可以先安装 Rust（推荐用 `rustup`）。

## 启动预览

在仓库根目录运行：

```bash
mdbook serve
```

默认会监听 `http://localhost:3000`，并在你修改 `src/` 下的 Markdown 后自动刷新。

## 构建静态文件

```bash
mdbook build
```

输出目录默认是 `book/`（由 `book.toml` 的 `[build].build-dir` 决定）。


