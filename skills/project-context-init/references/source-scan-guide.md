# Source Scan Guide

## Goal

Build the project fact base from source code before using AI-generated documents.

## Root-Level Files

Inspect:

- `pom.xml`
- `build.gradle`
- `settings.gradle`
- `README*`
- `.gitignore`
- `.github/workflows/*`
- `docker-compose*`
- `Dockerfile`

## Java Web Source Areas

Inspect available paths:

- `src/main/java`
- `src/main/resources`
- `src/test/java`
- `src/test/resources`
- `**/mapper/**/*.xml`
- `**/*Mapper.xml`
- `**/application*.yml`
- `**/application*.properties`

## Representative Sampling

Find up to five examples for each category when present:

- Controller: files containing `@RestController` or `@Controller`
- Service: files containing `@Service`
- Mapper: files containing `@Mapper`
- Repository: files containing `@Repository`
- Entity: files containing `@Entity`, `@Table`, or common entity package names
- DTO/VO: files or packages containing `dto`, `vo`, `request`, `response`
- Tests: files under `src/test`

Prefer files that are recently modified, central to a module, or referenced by multiple classes.

## Evidence Rules

When writing project facts:

- Cite file paths.
- Separate observed facts from inferred conventions.
- Write uncertain items to `open-questions.md`.
- Do not turn one isolated example into a global rule.

## Multi-Module Projects

If multiple modules exist:

- Identify the root build file.
- List modules in `project-profile.md`.
- Summarize module responsibilities in `architecture-summary.md`.
- Keep generated `docs/ai-coding/` at the project root unless the user explicitly asks for module-local context.

## Commands

Prefer fast file discovery:

```text
rg --files
rg "@RestController|@Controller|@Service|@Mapper|@Repository|@Entity"
```

If `rg` is unavailable, use the platform's normal recursive file search.
