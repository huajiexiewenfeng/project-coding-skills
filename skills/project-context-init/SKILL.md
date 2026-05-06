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
When no project path is provided, treat the current working directory as the project root.
When the user provides focus modules or scope paths, keep project root broad enough for shared docs but limit source sampling to those paths.
Never reuse docs/ai-coding from another project.
Never inherit coding rules from previous conversations unless the user explicitly points to the same project root.
Do not modify production code.
Do not silently overwrite architect-edited docs/ai-coding files.
Generated docs/ai-coding files are project-local shared context and should be committed to that project's Git repository.
Generated docs/ai-coding files are drafts until a team lead or architect reviews and approves them.
On first run, create the project-local context directory and starter templates before expecting the user to know their locations.
Do not expose internal skill reference file names as required user knowledge.
If docs/ai-coding already exists, treat develop:init as an update/refresh, not a fresh initialization.
Generate project-local docs in the user's prompt language unless existing docs already establish another project language.
```

## Required References

Read these reference files before generating output:

- `references/source-scan-guide.md`
- `references/ai-doc-discovery.md`
- `references/output-templates.md`

## Short Prompt Contract

Accept short daily prompts. Do not require the user to repeat default rules.

Typical input:

```text
Use develop:init for the current project.
Core workspace: <module-or-path>
Reference area: <module-or-path>
Optional context: <docs or feature focus>
```

Defaults:

- Current working directory is the project root unless the user provides another path.
- Source code, configuration, build files, and tests are the authority.
- Generate or update only `docs/ai-coding/`.
- Use the user's current language for generated document headings, prose, tables, and review notes.
- Do not modify production code.
- Treat core workspace paths as primary source-scan scope.
- Treat reference area paths as supporting context only unless source evidence shows direct dependencies.
- Treat prompt templates as feature-intake candidates and use the conservative prompt-template decision rules.

## Output Language

Choose the output language before writing files:

1. If the user explicitly requests a language, use it.
2. Otherwise use the language of the current `develop:init` prompt.
3. In update/refresh mode, preserve the language already used by existing `docs/ai-coding/` files unless the user asks to switch.
4. If mixed languages exist, prefer the language used by `feature-prompt-context.md` or `coding-rules.md`.

Keep file names and code identifiers stable in English:

```text
docs/ai-coding/project-profile.md
docs/ai-coding/architecture-summary.md
docs/ai-coding/coding-rules.md
docs/ai-coding/ai-context-sources.md
docs/ai-coding/feature-prompt-context.md
docs/ai-coding/open-questions.md
```

Translate headings, descriptions, table labels, review notes, and generated prose into the selected language. Do not translate Java package names, class names, method names, command lines, paths, API names, or enum values.

## Workflow

1. Resolve the project root.
2. Detect mode:
   - first-run bootstrap when `docs/ai-coding/` is missing.
   - update/refresh when `docs/ai-coding/` already exists.
3. For first-run bootstrap, create the project-local context directory and starter prompt templates.
4. For update/refresh, read existing `docs/ai-coding/` first and preserve reviewed content.
5. Identify optional focus modules or scope paths.
6. Build the source-code fact base from the project root, prioritizing focus paths when provided.
7. Discover existing AI-generated documents inside the project root.
8. Classify discovered documents as factual context, design notes, graph reports, or prompt templates.
9. Compare factual AI-doc claims against source facts.
10. Generate or merge `docs/ai-coding/` files.
11. Preserve architect-edited content and write conflicts to `open-questions.md`.
12. Mark prompt templates as requiring architect calibration before use in feature work.
13. Mark the generated context as requiring team lead or architect review before team-wide use.
14. Summarize generated files and architect review points.

## First-Run Bootstrap

If `docs/ai-coding/` does not exist, create it. Also create `docs/ai-coding/prompt-templates/`.

Create starter templates when no project-approved prompt template exists:

```text
docs/ai-coding/prompt-templates/feature-intake-template.md
docs/ai-coding/prompt-templates/feature-intake-template.zh.md
```

Use the templates in `references/output-templates.md`.

After bootstrapping, guide the user in plain language:

- Show the created files.
- Explain that these are project-local files committed to the business project.
- Ask the user or architect to edit the starter template if the default fields are not enough.
- If the user already provided core workspace, reference area, and feature focus, continue initialization.
- If critical information is missing, stop after bootstrap and ask the user to fill or confirm the template before continuing.
- Use the selected output language for all generated starter files and guidance.

## Update/Refresh Mode

If `docs/ai-coding/` already exists, do not bootstrap from scratch.

Before scanning source:

- Read all existing `docs/ai-coding/*.md`.
- Detect whether `coding-rules.md`, `feature-prompt-context.md`, or prompt templates contain reviewed project guidance.
- Treat existing reviewed guidance as user/architect-authored content.

During update:

- Append or merge newly observed source facts into existing documents.
- Keep stable reviewed rules unless source evidence clearly contradicts them.
- If source evidence contradicts existing project guidance, keep the existing text and record the conflict in `open-questions.md`.
- Do not recreate starter templates if `docs/ai-coding/prompt-templates/` already contains project templates.
- If starter templates exist unchanged, they may be updated only when the built-in starter template changed and no project-specific edits are detected.
- If unsure whether a file was architect-edited, preserve it and add a review note.

Final response must say whether the run was `first-run bootstrap` or `update/refresh`.

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

Generate or update these files under the resolved project root:

```text
docs/ai-coding/project-profile.md
docs/ai-coding/architecture-summary.md
docs/ai-coding/coding-rules.md
docs/ai-coding/ai-context-sources.md
docs/ai-coding/feature-prompt-context.md
docs/ai-coding/open-questions.md
docs/ai-coding/prompt-templates/feature-intake-template.md
docs/ai-coding/prompt-templates/feature-intake-template.zh.md
```

## Merge Rules

If `docs/ai-coding/` already exists:

- Read existing files first.
- Preserve architect-edited rules.
- Add newly observed source facts in clearly marked sections.
- Put conflicts and uncertain claims in `open-questions.md`.
- Do not silently replace `coding-rules.md` or `feature-prompt-context.md`.
- Do not reset generated files back to blank templates.
- Do not overwrite project-approved prompt templates.

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
