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
Read shared standards from `docs/ai-coding/standards/` before scoped context rules when present.
When docs/ai-coding contains scoped contexts, select the matching context directory before reading rules.
Never apply one scoped context to another module unless the user explicitly selects it.
Lock the selected context for the current session and do not switch to another context without explicit confirmation.
If a different context is requested after one is already loaded, warn that running project-feature-dev for the new context may replace the previously loaded runtime context.
Treat context switch as replacement, not additive load.
Do not write session lock state into project docs; project docs may contain static context boundaries only.
If the selected context appears unreviewed or open-questions.md contains unresolved project-rule questions, warn before relying on it.
```

## Required References

Read these reference files when using this skill:

- `references/feature-workflow.md`
- `references/final-response-template.md`
- `references/default-feature-intake-template.md` or `references/default-feature-intake-template.zh.md` when no project-approved or user-specified feature intake template exists.

## Workflow

1. Resolve the project root.
2. Check for `docs/ai-coding/` in that project root.
3. Read shared standards under `docs/ai-coding/standards/` when present.
4. Select the context directory:
   - user-specified context path/name
   - matching `docs/ai-coding/<context-scope>/`
   - the only scoped context if exactly one exists
   - legacy root `docs/ai-coding/feature-prompt-context.md`
5. Apply the context lock:
   - if no context is loaded in the current session, lock the selected context.
   - if the same context is already loaded, continue.
   - if another context is already loaded, warn that loading the new context may replace the previous runtime context, then stop and ask for explicit switch confirmation before reading the new context.
6. Read the selected context's `feature-prompt-context.md`.
7. Read the selected context's `open-questions.md` and warn if project-rule questions appear unresolved.
8. Read `project-profile.md`, `architecture-summary.md`, and `coding-rules.md` from the selected context as needed.
9. Select the feature intake template using the template priority rules.
10. Understand the user's feature request and collect required missing inputs.
11. Read related source code in the same project root, prioritizing the selected context's core workspace.
12. Find similar implementation examples.
13. Use brainstorming if the requirement or design is unclear.
14. Implement according to project-local context.
15. Run relevant verification.
16. Summarize changes, verification, risks, and manual review points.

## Context Lock

Maintain a session-local context lock after selecting a scoped context.

When a context is first loaded, state:

```text
Context locked: <context-scope>
Context directory: docs/ai-coding/<context-scope>/
```

If the user later asks for a different context in the same session, do not load it silently. Ask for confirmation and state:

```text
Current locked context: <previous-context>
Requested context: <new-context>
Warning: loading the requested context may replace the previously loaded runtime context in this session. The previous context rules may no longer be reliably available.
Confirm switching from <previous-context> to <new-context>?
```

If the user confirms, switch the lock and explicitly treat the new context as replacing the previous runtime context. If the user does not confirm, continue using the locked context.

Treat ambiguous short aliases such as `dev`, unclear module names, or context names not listed in `contexts.md` as potential mistakes. Ask before switching.

Context lock applies only within the current conversation/session. A new session must load context again.

Do not persist the runtime lock into `feature-prompt-context.md`, `contexts.md`, or any other project file.

## Missing Context Behavior

If `docs/ai-coding/` does not exist:

1. Tell the user that project-local context has not been initialized.
2. Recommend running `project-context-init` or using the alias `develop:init`.
3. If the user wants to continue anyway, proceed with ordinary feature development and state that no project-local AI context was available.

If `docs/ai-coding/` exists but no matching scoped context can be selected:

1. Read `docs/ai-coding/contexts.md` if present.
2. List plausible context scopes briefly.
3. Ask the user which context applies before coding.
4. If no scoped contexts exist but legacy root files exist, use the legacy root context and mention that it is unscoped.

## Relationship To Brainstorming

Use brainstorming when the request needs requirement clarification, design options, workflow decisions, or behavior changes that are not already specified.

Do not duplicate brainstorming. Load project context first, then let brainstorming handle design exploration when needed.

## Final Response

Use the template in `references/final-response-template.md`.

## Project Root Resolution Note

When running from a submodule, first search upward for an existing `docs/ai-coding/`. Prefer that directory's owner as the project root. Only fall back to build markers such as `pom.xml`, `build.gradle`, `settings.gradle`, or `.git` when no project-local AI context exists.
