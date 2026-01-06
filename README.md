# HahahaDevDocument

这是一个 **mdBook** 文档仓库模板（已内置示例章节 + 可选 GitHub Pages 自动部署）。

## 本地预览

安装 mdBook：

```bash
cargo install mdbook --locked
```

启动开发服务器：

```bash
mdbook serve
```

浏览器打开：`http://localhost:3000`

## 文档目录

- `book.toml`: mdBook 配置
- `src/SUMMARY.md`: 左侧导航（章节顺序）
- `src/`: 你的文档内容（Markdown）

## 发布到 GitHub Pages（可选）

仓库已包含工作流：`.github/workflows/deploy.yml`，会在你 push 到 `main` 后自动构建并发布。

你需要在 GitHub 仓库里做一次设置：

- **Settings → Pages → Build and deployment**：选择 **GitHub Actions**

部署完成后，Actions 里会显示站点 URL。

## 需要你替换的占位符

在 `book.toml` 里把下面的占位符改成你的仓库地址：

- `git-repository-url = "https://github.com/<OWNER>/<REPO>"`
- `edit-url-template = "https://github.com/<OWNER>/<REPO>/edit/main/{path}"`
