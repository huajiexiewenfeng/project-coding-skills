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

Read from the resolved project root only:

```text
docs/ai-coding/feature-prompt-context.md
docs/ai-coding/project-profile.md
docs/ai-coding/architecture-summary.md
docs/ai-coding/coding-rules.md
docs/ai-coding/open-questions.md
```

Required:

- `feature-prompt-context.md`

Optional when relevant:

- `project-profile.md`
- `architecture-summary.md`
- `coding-rules.md`
- `open-questions.md`

If `project-profile.md` or `architecture-summary.md` declares focus modules, use those modules as the primary source-reading scope for feature work.

## Feature Intake Templates

Select a feature intake template in this priority order:

1. User-specified template or section for the current task.
2. Project-approved template under `docs/ai-coding/prompt-templates/` or declared in `feature-prompt-context.md`.
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
2. Identify likely modules or packages, prioritizing declared focus modules.
3. Read related code.
4. Find similar existing implementation.
5. Note any relevant project-local coding rules.
6. Select and apply the feature intake template.
7. Use brainstorming if requirements or behavior are not clear.

## Implementation Rules

- Follow current project style.
- Keep changes focused on the feature.
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
