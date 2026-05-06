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

Inspect available paths. If focus modules or scope paths were provided, apply these patterns inside those paths first:

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

## Focus Scope

For large or multi-module repositories, project root and source scan scope can be different.

Project root is used for:

- root build files
- shared `docs/`
- existing AI-generated documents
- generated `docs/ai-coding/` registry

Focus scope is used for detailed source sampling.

When focus paths are provided:

- Sample source code primarily inside those paths.
- Read other modules only when referenced by focused modules.
- Record focus paths in `project-profile.md`.
- Summarize non-focused modules as out of scope unless they define shared APIs, DTOs, enums, clients, configuration, or tests used by the focused modules.
- Do not infer global project rules from non-focused modules.

## Context Scope

When the repository has multiple modules or product areas, write context for the selected scope instead of the whole repository.

- Record the selected context scope in `project-profile.md`.
- Generate scoped context under `docs/ai-coding/<context-scope>/`.
- Keep root `docs/ai-coding/contexts.md` as an index of available contexts.
- Do not infer repository-wide coding rules from a scoped scan.
- If the user provides an existing docs path such as `docs/dji-adapter/...`, consider that path's domain folder as a candidate context scope.

## Reference Areas

When the user provides reference areas:

- Use them to understand behavior, DTOs, enums, API semantics, errors, tests, or existing implementation patterns.
- Do not treat them as the main implementation target.
- Do not infer that the core workspace may directly depend on the reference area unless source code already shows that dependency.
- Record reference areas separately from core workspace paths in `project-profile.md` or `architecture-summary.md`.

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
- Keep generated context at the project root under `docs/ai-coding/<context-scope>/` when the user provides a focus module, domain, adapter, bounded context, or module-local purpose.
- If focus modules were provided, mark them as the primary coding context.

## Commands

Prefer fast file discovery:

```text
rg --files
rg "@RestController|@Controller|@Service|@Mapper|@Repository|@Entity"
```

If `rg` is unavailable, use the platform's normal recursive file search.
