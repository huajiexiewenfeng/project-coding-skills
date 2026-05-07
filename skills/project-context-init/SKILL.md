---
name: project-context-init
description: Use when the user says develop:init, wants to initialize or refresh project AI coding context, scan the current project source code, read existing AI-generated docs as supporting context, or generate project-local docs/ai-coding files.
---

# Project Context Init

## Purpose

Initialize or refresh project-local AI coding context for the current project.

This skill generates project-local context under `docs/ai-coding/` inside the target business project. It does not store project-specific rules in the skill itself.

## Hard Rules

```text
Source code, configuration, build files, and tests are the source of truth.
Existing AI-generated docs are supporting context, not authority.
Prompt templates with placeholders are feature-intake templates, not project facts.
When AI docs conflict with source code, source code wins.
Shared AI behavior standards belong under `docs/ai-coding/standards/`, not inside `contexts.md` or a scoped context body.
Always resolve context from the current working directory or an explicitly specified project root.
When no project path is provided, treat the current working directory as the project root.
When the user provides focus modules or scope paths, keep project root broad enough for shared docs but limit source sampling to those paths.
When context applies to one module, product area, adapter, bounded context, or feature family rather than the whole repository, write it under `docs/ai-coding/<context-scope>/`.
Keep `docs/ai-coding/` as the registry/root for AI coding contexts; keep scoped rules inside their own context directory.
Never reuse docs/ai-coding from another project.
Never apply one scoped context to another module unless the user explicitly selects it.
Never inherit coding rules from previous conversations unless the user explicitly points to the same project root.
Do not modify production code.
Do not silently overwrite architect-edited docs/ai-coding files.
Generated docs/ai-coding files are project-local shared context and should be committed to that project's Git repository.
Generated docs/ai-coding files are drafts until a team lead or architect reviews and approves them.
On first run, create the project-local context directory and starter templates before expecting the user to know their locations.
Do not expose internal skill reference file names as required user knowledge.
If the selected context directory already exists, treat develop:init as an update/refresh, not a fresh initialization.
Generate project-local docs in the user's prompt language unless existing docs for the selected context already establish another project language.
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
Context scope: <module-or-domain-name>
Core workspace: <module-or-path>
Reference area: <module-or-path>
Optional context: <docs or feature focus>
```

Defaults:

- Current working directory is the project root unless the user provides another path.
- Source code, configuration, build files, and tests are the authority.
- Generate or update only `docs/ai-coding/`.
- If no context scope is provided, infer it from the optional docs path, then the primary core workspace, then whole-project intent.
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
4. If mixed languages exist, prefer the language used by the selected context's `feature-prompt-context.md` or `coding-rules.md`.

Keep file names and code identifiers stable in English:

```text
docs/ai-coding/<context-scope>/project-profile.md
docs/ai-coding/<context-scope>/architecture-summary.md
docs/ai-coding/<context-scope>/coding-rules.md
docs/ai-coding/<context-scope>/ai-context-sources.md
docs/ai-coding/<context-scope>/feature-prompt-context.md
docs/ai-coding/<context-scope>/open-questions.md
```

Translate headings, descriptions, table labels, review notes, and generated prose into the selected language. Do not translate Java package names, class names, method names, command lines, paths, API names, or enum values.

## Workflow

1. Resolve the project root.
2. Identify optional focus modules, reference areas, and context scope.
3. Detect mode for the selected context:
   - first-run bootstrap when the selected context directory is missing.
   - update/refresh when the selected context directory already exists.
4. For first-run bootstrap, create `docs/ai-coding/`, the selected context directory, and starter prompt templates.
5. For update/refresh, read existing files from the selected context directory first and preserve reviewed content.
6. Build the source-code fact base from the project root, prioritizing focus paths when provided.
7. Discover existing AI-generated documents inside the project root.
8. Classify discovered documents as factual context, design notes, graph reports, or prompt templates.
9. Compare factual AI-doc claims against source facts.
10. Generate or merge files in the selected context directory.
11. Preserve architect-edited content and write conflicts to `open-questions.md`.
12. Mark prompt templates as requiring architect calibration before use in feature work.
13. Mark the generated context as requiring team lead or architect review before team-wide use.
14. Summarize generated files and architect review points.

## Context Scope Resolution

Project root and context scope are different concepts.

- Project root is the repository root that owns shared source, build files, Git history, and `docs/`.
- Context scope is the module/domain name whose AI coding rules are being initialized.
- Context directory is `docs/ai-coding/<context-scope>/`.

Resolve context scope in this order:

1. User-provided context name, scope name, module context, or output directory.
2. Existing project docs path with a clear domain folder, such as `docs/dji-adapter/...` -> `dji-adapter`.
3. Primary core workspace basename, such as `dji-dock3-adapter`.
4. `project` only when the user clearly asks for whole-repository context and no narrower focus exists.

Normalize scope names to lowercase path-safe kebab-case. Preserve common project identifiers when already path-safe, such as `dji-adapter`.

If multiple scopes are plausible, ask a concise confirmation before writing. Example: "Use `dji-adapter` or `dji-dock3-adapter` as the context directory?"

If legacy root-level files such as `docs/ai-coding/project-profile.md` already exist and the selected context is narrower than the repository, treat them as legacy unscoped context. Do not delete or move them silently. Ask whether to migrate them into `docs/ai-coding/<context-scope>/` or create the scoped context beside them.

## First-Run Bootstrap

If the selected context directory does not exist, create `docs/ai-coding/` and `docs/ai-coding/<context-scope>/`. Also create `docs/ai-coding/<context-scope>/prompt-templates/`.
Create `docs/ai-coding/standards/` if it does not exist.

Create or update the root registry:

```text
docs/ai-coding/contexts.md
docs/ai-coding/standards/llm-coding-guidelines.md
```

Create starter templates when no project-approved prompt template exists:

```text
docs/ai-coding/<context-scope>/prompt-templates/feature-intake-template.md
docs/ai-coding/<context-scope>/prompt-templates/feature-intake-template.zh.md
```

Use the templates in `references/output-templates.md`.

After bootstrapping, guide the user in plain language:

- Show the created files.
- Explain that these are project-local files committed to the business project.
- Explain that shared standards apply to all scoped contexts unless a context explicitly narrows them.
- Ask the user or architect to edit the starter template if the default fields are not enough.
- If the user already provided core workspace, reference area, and feature focus, continue initialization.
- If critical information is missing, stop after bootstrap and ask the user to fill or confirm the template before continuing.
- Use the selected output language for all generated starter files and guidance.

## Update/Refresh Mode

If the selected context directory already exists, do not bootstrap from scratch.

Before scanning source:

- Read existing `docs/ai-coding/contexts.md` if present.
- Read existing `docs/ai-coding/standards/*.md` if present.
- Read all existing `docs/ai-coding/<context-scope>/*.md`.
- Detect whether `coding-rules.md`, `feature-prompt-context.md`, or prompt templates contain reviewed project guidance.
- Treat existing reviewed guidance as user/architect-authored content.

During update:

- Append or merge newly observed source facts into existing documents.
- Keep stable reviewed rules unless source evidence clearly contradicts them.
- If source evidence contradicts existing project guidance, keep the existing text and record the conflict in `open-questions.md`.
- Do not recreate starter templates if `docs/ai-coding/<context-scope>/prompt-templates/` already contains project templates.
- Do not overwrite shared standards if `docs/ai-coding/standards/llm-coding-guidelines.md` has been edited by the team.
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
docs/ai-coding/contexts.md
docs/ai-coding/standards/llm-coding-guidelines.md
docs/ai-coding/<context-scope>/project-profile.md
docs/ai-coding/<context-scope>/architecture-summary.md
docs/ai-coding/<context-scope>/coding-rules.md
docs/ai-coding/<context-scope>/ai-context-sources.md
docs/ai-coding/<context-scope>/feature-prompt-context.md
docs/ai-coding/<context-scope>/open-questions.md
docs/ai-coding/<context-scope>/prompt-templates/feature-intake-template.md
docs/ai-coding/<context-scope>/prompt-templates/feature-intake-template.zh.md
```

## Merge Rules

If the selected context directory already exists:

- Read existing files first.
- Preserve architect-edited rules.
- Add newly observed source facts in clearly marked sections.
- Put conflicts and uncertain claims in `open-questions.md`.
- Do not silently replace `coding-rules.md` or `feature-prompt-context.md`.
- Do not reset generated files back to blank templates.
- Do not overwrite project-approved prompt templates.
- Do not mix facts or rules from another scoped context into this one.
- Do not silently replace shared standards; append review notes or ask when changes are needed.

## Final Response

After running, report:

- Resolved project root.
- Selected context scope and context directory.
- Focus scope used, if any.
- Generated or updated files.
- Existing AI docs discovered.
- Important source facts used.
- Conflicts or uncertainties written to `open-questions.md`.
- Clear note that the generated context is a draft until reviewed by a team lead or architect.
- Reminder to commit `docs/ai-coding/` to the project Git repository.
