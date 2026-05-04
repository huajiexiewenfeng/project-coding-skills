# Feature Workflow

## Project Root

Resolve the project root before reading rules or source files.

Use this order:

1. User-provided path.
2. Current working directory.
3. Nearest ancestor containing `docs/ai-coding`, `pom.xml`, `build.gradle`, `settings.gradle`, or `.git`.

If multiple roots are plausible, ask the user to confirm.

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

## Before Coding

1. Restate the feature request briefly.
2. Identify likely modules or packages.
3. Read related code.
4. Find similar existing implementation.
5. Note any relevant project-local coding rules.
6. Use brainstorming if requirements or behavior are not clear.

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
