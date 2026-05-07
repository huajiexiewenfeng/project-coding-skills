# Output Templates

Use these templates when generating project-local AI coding context in the target project.

Root registry path:

```text
docs/ai-coding/contexts.md
docs/ai-coding/standards/llm-coding-guidelines.md
```

Scoped context paths:

```text
docs/ai-coding/<context-scope>/
docs/ai-coding/<context-scope>/prompt-templates/
```

## Language Rendering Rules

These templates define structure, not fixed English copy.

- Keep file names unchanged.
- Render headings, prose, table headers, placeholders, and review notes in the selected output language.
- Use Chinese headings and labels when the user prompt is Chinese.
- Preserve existing document language during update/refresh unless the user asks to switch.
- Do not translate paths, commands, package names, class names, method names, enum values, API names, or dependency names.

Suggested Chinese title mapping:

| File | English Title | Chinese Title |
|---|---|---|
| `project-profile.md` | Project Profile | 项目画像 |
| `architecture-summary.md` | Architecture Summary | 架构摘要 |
| `coding-rules.md` | Coding Rules | 编码规则 |
| `ai-context-sources.md` | AI Context Sources | AI 上下文来源 |
| `feature-prompt-context.md` | Feature Prompt Context | 功能开发上下文 |
| `open-questions.md` | Open Questions | 待确认问题 |
| `contexts.md` | AI Coding Contexts | AI 编码上下文索引 |
| `standards/llm-coding-guidelines.md` | LLM Coding Guidelines | LLM 编码行为规范 |

## contexts.md

```markdown
# AI Coding Contexts

This file indexes AI coding contexts for this repository. Each scoped context applies only to the listed workspace unless an architect extends it.

| Context Scope | Context Directory | Core Workspace | Reference Area | Status | Last Updated |
|---|---|---|---|---|---|
| `<context-scope>` | `docs/ai-coding/<context-scope>/` | `<core paths>` | `<reference paths or none>` | `<draft | architect-reviewed | unknown>` | `<date or unknown>` |

## Rules

- Use the context that matches the module or feature being changed.
- Read shared standards under `docs/ai-coding/standards/` before scoped context rules.
- Do not apply one scoped context to another module without explicit confirmation.
- Source code, configuration, build files, and tests remain the source of truth.
```

## standards/llm-coding-guidelines.md

```markdown
# LLM Coding Guidelines

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

Tradeoff: These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

Do not assume. Do not hide confusion. Surface tradeoffs.

Before implementing:

- State assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them instead of picking silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop, name what is confusing, and ask.

## 2. Simplicity First

Minimum code that solves the problem. Nothing speculative.

- No features beyond what was asked.
- No abstractions for single-use code.
- No flexibility or configurability that was not requested.
- No error handling for impossible scenarios.
- If 200 lines could be 50, simplify.

Ask: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

Touch only what is necessary. Clean up only your own changes.

When editing existing code:

- Do not improve adjacent code, comments, or formatting.
- Do not refactor things that are not broken.
- Match existing style, even if another style seems preferable.
- If unrelated dead code is found, mention it instead of deleting it.

When changes create orphans:

- Remove imports, variables, or functions made unused by the current change.
- Do not remove pre-existing dead code unless requested.

Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

Define success criteria. Loop until verified.

Transform tasks into verifiable goals:

- "Add validation" -> "Write tests for invalid inputs, then make them pass"
- "Fix the bug" -> "Write a test that reproduces it, then make it pass"
- "Refactor X" -> "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

```text
1. [Step] -> verify: [check]
2. [Step] -> verify: [check]
3. [Step] -> verify: [check]
```

These guidelines are working if diffs contain fewer unnecessary changes, fewer rewrites are caused by overcomplication, and clarifying questions happen before implementation rather than after mistakes.
```

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
- Context scope: `<context-scope>`
- Context directory: `docs/ai-coding/<context-scope>/`
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
- Read shared standards from `docs/ai-coding/standards/llm-coding-guidelines.md` when present.
- Select the matching context under this project's `docs/ai-coding/<context-scope>/`.
- Read only the selected context directory unless the user explicitly switches context.
- Read related code before changing anything.
- Find similar existing implementation first.
- Prefer existing project patterns.
- Keep changes small and focused.
- Do not introduce new abstractions unless there is clear need.

## Context Boundary

- Context scope: `<context-scope>`
- Context directory: `docs/ai-coding/<context-scope>/`
- Applies to: `<core workspace paths>`

Rules:

- This context only applies to the listed core workspace and its confirmed dependencies.
- Do not apply this context to another module or business area without explicit confirmation.
- If another context is needed, load that context separately instead of blending rules.
- Do not blend coding rules, architecture facts, prompt templates, or assumptions from another scoped context.
- Runtime session context locks are handled by `project-feature-dev`; do not record session state in this file.

## Architecture Rules

- Follow `architecture-summary.md` and `coding-rules.md`.
- Follow shared standards for AI behavior, especially simplicity, surgical changes, and explicit assumptions.
- Source code wins over AI-generated docs.
- Do not apply rules from another project.
- Do not apply rules from another scoped context unless explicitly selected.

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
2. Project-approved template under `docs/ai-coding/<context-scope>/prompt-templates/`.
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
