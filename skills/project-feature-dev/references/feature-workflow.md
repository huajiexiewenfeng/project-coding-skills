# Feature Workflow

## Project Root

Resolve the project root before reading rules or source files.

Use this order:

1. User-provided path.
2. Current working directory.
3. Nearest ancestor containing `docs/ai-coding`.
4. Nearest ancestor containing `pom.xml`, `build.gradle`, `settings.gradle`, or `.git`.

If multiple roots are plausible, ask the user to confirm.

For multi-module projects, prefer the root that owns `docs/ai-coding/`, even when the current submodule also has a build file.

## Context Loading

Read from the resolved project root only. Then select one context directory.

First read shared standards when present:

```text
docs/ai-coding/standards/llm-coding-guidelines.md
docs/ai-coding/standards/*.md
```

Shared standards define AI behavior such as thinking before coding, simplicity, surgical changes, and verification discipline. They do not override source-code facts or the selected context's project-specific architecture rules.

Selection priority:

1. User-specified context path or context scope.
2. `docs/ai-coding/<context-scope>/` matching the current module, feature area, or declared core workspace.
3. The only scoped context under `docs/ai-coding/` if exactly one exists.
4. Legacy root files directly under `docs/ai-coding/`.

If multiple scoped contexts are plausible, ask the user which one applies before coding.

For scoped context, read:

```text
docs/ai-coding/<context-scope>/feature-prompt-context.md
docs/ai-coding/<context-scope>/project-profile.md
docs/ai-coding/<context-scope>/architecture-summary.md
docs/ai-coding/<context-scope>/coding-rules.md
docs/ai-coding/<context-scope>/open-questions.md
```

Required:

- `feature-prompt-context.md`

Optional when relevant:

- `project-profile.md`
- `architecture-summary.md`
- `coding-rules.md`
- `open-questions.md`

If `project-profile.md` or `architecture-summary.md` declares focus modules, use those modules as the primary source-reading scope for feature work.

Do not load rules from a different scoped context unless the user explicitly switches context.

Rule priority:

1. Source code, configuration, build files, and tests.
2. Shared standards under `docs/ai-coding/standards/`.
3. Selected scoped context files.
4. User's current feature request.

## Feature Intake Templates

Select a feature intake template in this priority order:

1. User-specified template or section for the current task.
2. Project-approved template under `docs/ai-coding/<context-scope>/prompt-templates/` or declared in the selected `feature-prompt-context.md`.
3. Project candidate template under `docs/prompt-template/` or other user-provided project docs, after asking whether to use it.
4. Skill default template: `references/default-feature-intake-template.md` or `references/default-feature-intake-template.zh.md`.

Choose the default template language from the user's current language. Use the Chinese default template for Chinese conversations.

When a project candidate template is discovered but not approved, ask whether to:

1. Use the project template.
2. Use the default template.
3. Merge project-specific fields into the default template.
4. Skip template collection for this feature.

If `feature-prompt-context.md` declares an approved feature intake template, collect the required per-feature values before implementation. Examples include API document URL, target branch, feature type, target capability, and extra constraints. Do not treat unfilled placeholders as requirements.

If a required field is missing, ask whether to:

1. Fill the missing values now.
2. Continue using only stable project context.
3. Skip the template for this feature.

## Context Approval Check

Before relying on project-local context:

- Read `open-questions.md` if it exists.
- If it contains unresolved architecture, coding-rule, or project-boundary questions, warn the user.
- If the generated context appears to be an unreviewed draft, recommend team lead or architect review before broad team use.
- For the current task, continue only if the user accepts the risk or the unresolved questions are unrelated to the feature.

## Before Coding

1. Restate the feature request briefly.
2. State key assumptions or ask if unclear.
3. Identify likely modules or packages, prioritizing declared focus modules.
4. Read related code.
5. Find similar existing implementation.
6. Note relevant shared standards and project-local coding rules.
7. Select and apply the feature intake template.
8. Use brainstorming if requirements or behavior are not clear.

## Implementation Rules

- Follow current project style.
- Prefer the smallest implementation that satisfies the request.
- Keep changes focused on the feature.
- Avoid speculative abstractions, adjacent cleanup, and unrelated formatting changes.
- Do not introduce new framework choices without explicit need.
- Do not change public compatibility unless requested.
- Add or update tests when business logic changes.
- If project docs and source code conflict, follow source code and mention the conflict.

## Verification

Pick the smallest relevant verification command from project context or build files.

Examples:

```text
mvn test
mvn -q test
mvn -pl <module> test
./gradlew test
./gradlew :<module>:test
```

If verification cannot be run, state why.
