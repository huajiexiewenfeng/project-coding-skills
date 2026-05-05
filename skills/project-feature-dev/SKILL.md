---
name: project-feature-dev
description: Use when the user says develop:feature, wants to implement a feature or new requirement in the current project, or wants project-local docs/ai-coding context and team coding rules loaded before development.
---

# Project Feature Dev

## Purpose

Load project-local AI coding context before feature development.

This skill is a context loader and project-rule adapter. It does not replace brainstorming or other feature-design workflows.

## Hard Rules

```text
Always resolve the current project root before reading project rules.
Only read docs/ai-coding from the current project root.
Never reuse docs/ai-coding from another project.
Never apply rules from previous conversations to a different project.
Source code wins over docs/ai-coding when they conflict.
If docs/ai-coding does not exist, recommend project-context-init.
If docs/ai-coding appears unreviewed or open-questions.md contains unresolved project-rule questions, warn before relying on it.
```

## Required References

Read these reference files when using this skill:

- `references/feature-workflow.md`
- `references/final-response-template.md`

## Workflow

1. Resolve the project root.
2. Check for `docs/ai-coding/` in that project root.
3. Read `docs/ai-coding/feature-prompt-context.md`.
4. Read `open-questions.md` and warn if project-rule questions appear unresolved.
5. Read `project-profile.md`, `architecture-summary.md`, and `coding-rules.md` as needed.
6. Understand the user's feature request.
7. Read related source code in the same project root.
8. Find similar implementation examples.
9. Use brainstorming if the requirement or design is unclear.
10. Implement according to project-local context.
11. Run relevant verification.
12. Summarize changes, verification, risks, and manual review points.

## Missing Context Behavior

If `docs/ai-coding/` does not exist:

1. Tell the user that project-local context has not been initialized.
2. Recommend running `project-context-init` or using the alias `develop:init`.
3. If the user wants to continue anyway, proceed with ordinary feature development and state that no project-local AI context was available.

## Relationship To Brainstorming

Use brainstorming when the request needs requirement clarification, design options, workflow decisions, or behavior changes that are not already specified.

Do not duplicate brainstorming. Load project context first, then let brainstorming handle design exploration when needed.

## Final Response

Use the template in `references/final-response-template.md`.

## Project Root Resolution Note

When running from a submodule, first search upward for an existing `docs/ai-coding/`. Prefer that directory's owner as the project root. Only fall back to build markers such as `pom.xml`, `build.gradle`, `settings.gradle`, or `.git` when no project-local AI context exists.
