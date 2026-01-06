# 附录：FAQ

## Q：为什么有 `SUMMARY.md`？

mdBook 用 `src/SUMMARY.md` 来生成左侧导航和章节顺序。你新增/调整章节，通常只需要同步修改这里。

## Q：图片放哪里？

常见做法是：

- 放在 `src/assets/`（你可以自行创建）
- 引用时用相对路径，例如：`![logo](assets/logo.png)`

## Q：能否多语言？

可以，但需要你自行组织多语言目录与构建策略。这个模板默认 `multilingual = false`，先把单语言内容写扎实更划算。

## Q：如何把 GitHub 仓库链接显示在页面上？

在 `book.toml` 的 `[output.html]` 里设置：

- `git-repository-url`
- `edit-url-template`

把里面的 `<OWNER>/<REPO>` 换成你的真实仓库即可。


