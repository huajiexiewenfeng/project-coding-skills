---
name: project-context-init
description: Use when the user says develop:init, wants to initialize or refresh project AI coding context, scan the current project source code, read existing AI-generated docs as supporting context, or generate project-local docs/ai-coding files.
---

# Project Context Init

## Purpose

Initialize or refresh project-local AI coding context for the current project.

This skill generates `docs/ai-coding/` inside the target business project. It does not store project-specific rules in the skill itself.

## Hard Rules

```text
Source code, configuration, build files, and tests are the source of truth.
Existing AI-generated docs are supporting context, not authority.
Prompt templates with placeholders are feature-intake templates, not project facts.
When AI docs conflict with source code, source code wins.
Always resolve context from the current working directory or an explicitly specified project root.
When the user provides focus modules or scope paths, keep project root broad enough for shared docs but limit source sampling to those paths.
Never reuse docs/ai-coding from another project.
Never inherit coding rules from previous conversations unless the user explicitly points to the same project root.
Do not modify production code.
Do not silently overwrite architect-edited docs/ai-coding files.
Generated docs/ai-coding files are project-local shared context and should be committed to that project's Git repository.
Generated docs/ai-coding files are drafts until a team lead or architect reviews and approves them.
```

## Required References

Read these reference files before generating output:

- `references/source-scan-guide.md`
- `references/ai-doc-discovery.md`
- `references/output-templates.md`

## Workflow

1. Resolve the project root.
2. Identify optional focus modules or scope paths.
3. Build the source-code fact base from the project root, prioritizing focus paths when provided.
4. Discover existing AI-generated documents inside the project root.
5. Classify discovered documents as factual context, design notes, graph reports, or prompt templates.
6. Compare factual AI-doc claims against source facts.
7. Generate or merge `docs/ai-coding/` files.
8. Preserve architect-edited content and write conflicts to `open-questions.md`.
9. Mark prompt templates as requiring architect calibration before use in feature work.
10. Mark the generated context as requiring team lead or architect review before team-wide use.
11. Summarize generated files and architect review points.

## Project Root Resolution

Use this order:

1. User-provided project path.
2. Current working directory.
3. Nearest ancestor containing one of:
   - `pom.xml`
   - `build.gradle`
   - `settings.gradle`
   - `.git`
   - `docs/ai-coding`

If multiple plausible roots exist, ask the user to confirm before writing files.

## Focus Scope

For large or multi-module repositories, the project root and source scan scope may be different.

Use the project root for:

- shared `docs/`
- existing AI documents
- root build files
- generated `docs/ai-coding/`
- repository-level Git context

Use focus scope for source sampling when the user provides paths such as:

```text
dji-dock3-adapter
dock-api
```

Do not scan every module equally when focus scope is provided. Summarize out-of-scope modules only when they are needed to explain dependencies or boundaries.

## Output Contract

Generate or update exactly these files under the resolved project root:

```text
docs/ai-coding/project-profile.md
docs/ai-coding/architecture-summary.md
docs/ai-coding/coding-rules.md
docs/ai-coding/ai-context-sources.md
docs/ai-coding/feature-prompt-context.md
docs/ai-coding/open-questions.md
```

## Merge Rules

If `docs/ai-coding/` already exists:

- Read existing files first.
- Preserve architect-edited rules.
- Add newly observed source facts in clearly marked sections.
- Put conflicts and uncertain claims in `open-questions.md`.
- Do not silently replace `coding-rules.md` or `feature-prompt-context.md`.

## Final Response

After running, report:

- Resolved project root.
- Focus scope used, if any.
- Generated or updated files.
- Existing AI docs discovered.
- Important source facts used.
- Conflicts or uncertainties written to `open-questions.md`.
- Clear note that the generated context is a draft until reviewed by a team lead or architect.
- Reminder to commit `docs/ai-coding/` to the project Git repository.
