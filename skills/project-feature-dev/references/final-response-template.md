# Final Response Template

Use this shape after completing feature work.

```text
变更摘要：
- <what changed>

涉及文件：
- <path>: <why it changed>

验证：
- <command>: <result>

风险：
- <remaining risk or "未发现明显剩余风险">

建议人工重点 review：
- <specific file, behavior, or rule to inspect>
```

If no tests or verification were run, say:

```text
验证：
- 未运行：<specific reason>
```

If `docs/ai-coding/` was missing and the user chose to continue, say:

```text
项目上下文：
- 未发现 docs/ai-coding/，本次未加载项目本地 AI 编码上下文。
```
