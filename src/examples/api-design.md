# 示例章节：API 设计

这是一篇“示例文档”，展示 mdBook 中常见写法：标题、列表、代码块、提示块、表格等。

## 目标

设计一个面向内部服务的 HTTP API：创建/查询一个资源 `Widget`。

## 一句话结论（推荐放最前）

- **命名清晰**：路径用名词，动作用 HTTP 方法表达
- **错误一致**：错误码 + 机器可读的错误类型 + 可读的 message
- **可演进**：字段可选、向后兼容、避免破坏性改动

## 请求与响应

### 创建 Widget

```http
POST /api/widgets
Content-Type: application/json

{
  "name": "demo",
  "enabled": true
}
```

成功响应：

```http
201 Created
Content-Type: application/json

{
  "id": "w_123",
  "name": "demo",
  "enabled": true,
  "created_at": "2026-01-06T00:00:00Z"
}
```

### 查询 Widget

```http
GET /api/widgets/w_123
```

## 错误返回（统一格式）

```json
{
  "error": {
    "type": "VALIDATION_ERROR",
    "message": "name is required",
    "fields": {
      "name": "missing"
    }
  }
}
```

| 字段 | 含义 |
| --- | --- |
| `type` | 机器可读错误类型（稳定） |
| `message` | 人类可读信息（可变化） |
| `fields` | 细粒度字段错误（可选） |

## 提示块（用引用语法模拟）

> 提示：mdBook 原生 Markdown 没有“提示框”语法，但引用块（`>`）在多数主题下也能达到良好的强调效果。


