# ADR：架构决策记录（Architecture Decision Records）

ADR 用来记录"**最终决定了什么**"以及"**为什么这么决定**"。它的价值在于：未来回看时，能迅速理解当时的约束与取舍，而不是重复走一遍讨论。

## 这个目录放什么

- 已达成共识的关键决策（例如：采用某协议/存储/架构拆分方式）
- 决策的背景、约束、备选方案与理由
- 决策的影响范围与后续动作（迁移、兼容、风险）

## 这个目录不放什么

- 还在摇摆的方案草稿（放到 `design/`）
- 过程性讨论、评审意见原始记录（放到 `discussions/`）

## 编号与命名建议

- 文件命名：`NNNN-title.md`（例如 `1-auth-strategy.md`）
- 标题格式：`ADR-NNNN：xxx`
- 目录组织：相关 ADR 可以组织在子目录中（例如 `1-cpp23/`、`2-basic-project-design/`）

## ADR 列表

### ADR-0001：使用 C++23 现代特性
- 位置：[1-cpp23/0001-cpp23.md](1-cpp23/0001-cpp23.md)
- 状态：已接受
- 内容：决定全面拥抱 C++23 标准，使用 Concepts 和 stacktrace 等现代特性

### ADR-0002：基础项目设计
- 位置：[2-basic-project-design/0002-basic-project-design.md](2-basic-project-design/0002-basic-project-design.md)
- 状态：已接受
- 内容：包含项目基本结构、CI/CD 要求、依赖管理、测试要求、功能添加要求、文档注释要求和贡献规范等基础设计决策



