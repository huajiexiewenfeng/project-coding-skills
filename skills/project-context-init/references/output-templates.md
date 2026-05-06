# Output Templates

Use these templates when generating `docs/ai-coding/` in the target project.

## prompt-templates/feature-intake-template.md

```markdown
# Feature Intake Template

Use this project-local template to collect feature inputs before implementation. Edit it for this project after architect review.

## Required

- Feature goal: `<what the user wants to build or change>`
- Core workspace: `<primary module or path>`
- Acceptance behavior: `<observable behavior or expected result>`

## Optional

- Reference area: `<supporting module, implementation, API, or document>`
- Related docs: `<paths or URLs>`
- Constraints: `<compatibility, dependency, API, encoding, performance, or rollout constraints>`
- Verification: `<preferred compile, test, or manual check>`
- Open questions: `<unknowns that should be clarified before coding>`

## Rules

- Empty fields are not requirements.
- Source code and `docs/ai-coding/` remain the authority.
- Project-specific fields may be added by the team lead or architect.
```

## prompt-templates/feature-intake-template.zh.md

```markdown
# 功能输入模板

用于在实现前收集本项目的功能开发输入。架构师 review 后可以按项目情况调整。

## 必填

- 功能目标：`<用户希望新增或修改什么>`
- 核心工作区：`<主要模块或路径>`
- 验收行为：`<可观察的行为或预期结果>`

## 可选

- 参考区：`<辅助模块、实现、API 或文档>`
- 相关文档：`<路径或 URL>`
- 约束：`<兼容性、依赖、API、编码、性能或发布约束>`
- 验证方式：`<建议的编译、测试或人工检查>`
- 未确认问题：`<编码前需要澄清的问题>`

## 规则

- 空字段不是需求。
- 源码和 `docs/ai-coding/` 仍然是权威来源。
- 团队 leader 或架构师可以按项目情况增加字段。
```

## project-profile.md

```markdown
# Project Profile

## Context Status

- Mode: `<first-run bootstrap | update/refresh>`
- Last updated: `<date or unknown>`
- Review status: `<draft | architect-reviewed | unknown>`

## Project Root

- Path: `<resolved project root>`

## Scope

- Core workspace: `<primary source-scan paths or current project>`
- Reference area: `<supporting reference paths or none>`

## Build

- Build tool: `<Maven | Gradle | unknown>`
- Java version: `<observed version or unknown>`
- Spring Boot version: `<observed version or unknown>`

## Main Dependencies

- Web: `<observed dependency or unknown>`
- Persistence: `<observed dependency or unknown>`
- Cache: `<observed dependency or unknown>`
- Messaging: `<observed dependency or unknown>`
- Testing: `<observed dependency or unknown>`

## Common Commands

- Compile: `<observed command or unknown>`
- Test: `<observed command or unknown>`
- Run locally: `<observed command or unknown>`

## Evidence

- `<file path>`: `<fact observed>`
```

## architecture-summary.md

```markdown
# Architecture Summary

## Source Of Truth

This document summarizes observed source-code facts. If this document conflicts with source code, source code wins.

## Modules

- `<module>`: `<observed responsibility or unknown>`

## Package Structure

- Controller: `<path or unknown>`
- Service: `<path or unknown>`
- Mapper/Repository: `<path or unknown>`
- Entity: `<path or unknown>`
- DTO/VO: `<path or unknown>`
- Config: `<path or unknown>`
- Test: `<path or unknown>`

## Typical Call Direction

```text
<observed call direction, for example Controller -> Service -> Mapper>
```

## Representative Files

- `<file path>`: `<why this is a good example>`

## Architecture Exceptions

- `<observed exception or "No clear exception observed">`
```

## coding-rules.md

```markdown
# Coding Rules

## Source Priority

Source code, configuration, build files, and tests are the source of truth.

## Observed Rules

- Prefer existing project style.
- Read related code and similar implementations before changing code.
- Keep changes small and focused.
- Do not introduce new frameworks or abstractions without clear need.
- Do not change public API compatibility unless explicitly requested.
- Add or update tests when business logic changes.
- Report verification results after implementation.

## Project-Specific Rules

- `<rule inferred from source evidence>`

## Evidence

- `<file path>`: `<evidence for rule>`
```

## ai-context-sources.md

```markdown
# AI Context Sources

Existing AI-generated documents are supporting context, not authority.

## Sources Read

| Path | Source Type | Summary | Trust Level | Notes |
|---|---|---|---|---|
| `<path>` | `<superpower | graphify | prompt-template | other-ai | manual-doc>` | `<summary>` | `<source-confirmed | template-not-fact | partial | uncertain>` | `<notes>` |

## Prompt Templates Found

Prompt templates are feature-intake aids, not source-of-truth project facts.

| Path | Intended Use | Required Placeholders | Handling Decision |
|---|---|---|---|
| `<path>` | `<when this template is useful>` | `<fields users must fill per feature>` | `<adopt | wait-for-filled-values | merge-with-default | stable-parts-only | skip | unknown>` |

## Conflicts With Source Code

- `<AI-doc claim>` conflicts with `<source evidence>`.

## Useful Context

- `<context that was confirmed by source code or explicitly marked as supporting context>`
```

## feature-prompt-context.md

```markdown
# Feature Prompt Context

## Before Coding

- Resolve the current project root.
- Read this project's `docs/ai-coding/` only.
- Read related code before changing anything.
- Find similar existing implementation first.
- Prefer existing project patterns.
- Keep changes small and focused.
- Do not introduce new abstractions unless there is clear need.

## Architecture Rules

- Follow `architecture-summary.md` and `coding-rules.md`.
- Source code wins over AI-generated docs.
- Do not apply rules from another project.

## Feature Workflow

1. Understand the requirement.
2. Read related code.
3. Find similar implementation.
4. Use brainstorming when design clarification is needed.
5. Propose a short implementation plan when useful.
6. Implement the smallest safe change.
7. Add or update tests when business logic changes.
8. Run relevant verification.

## Feature Intake

Template priority:

1. User-specified template or section for the current task.
2. Project-approved template under `docs/ai-coding/prompt-templates/`.
3. Project candidate template under `docs/prompt-template/`, after user confirmation.
4. Skill default template.

If this project has an approved prompt template, collect these values before implementation:

- `<field name>`: `<why it is needed>`

Do not treat unfilled placeholders as requirements.
Do not apply a prompt template from another project.

## Final Response

Always include:

- changed files
- verification commands
- remaining risks
- manual review points
```

## open-questions.md

```markdown
# Open Questions

## Needs Architect Review

- `<question>`
- `<prompt template path>`: choose whether to adopt, wait for filled placeholders, merge with the skill default template, use stable verified parts only, or skip this template.

## Source vs AI Doc Conflicts

- `<conflict>`

## Unclear Project Conventions

- `<unclear convention>`

## Missing Evidence

- `<claim or rule that could not be confirmed>`
```
