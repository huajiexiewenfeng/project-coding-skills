# Project Coding Skills

Reusable AI coding skills for project-local context initialization and feature development.

English | [简体中文](./README.zh.md)

![Project Coding Skills workflow](./docs/assets/project-coding-skills-flow-en.svg)

This project helps AI coding agents work with the rules of the current repository instead of carrying assumptions from another project. The core idea is simple:

> Skills are global. Project rules are local.

## Why

AI coding tools are powerful, but a team usually has project-specific conventions:

- architecture style
- package layout
- controller/service/repository boundaries
- testing habits
- API conventions
- existing AI-generated specs and plans
- code review expectations

Those rules should not live in every developer's personal prompt. They should live in the project repository and be shared by the whole team.

## MVP Skills

This repository currently contains two MVP skills:

```text
skills/
  project-context-init/
  project-feature-dev/
```

### `project-context-init`

Alias: `develop:init`

Use this skill to initialize or refresh project-local AI coding context.

It scans the current project, treats source code as the source of truth, reads existing AI-generated documents as supporting context, and generates:

```text
docs/ai-coding/
  project-profile.md
  architecture-summary.md
  coding-rules.md
  ai-context-sources.md
  feature-prompt-context.md
  open-questions.md
```

These files belong to the target business project, not this skill repository.

### `project-feature-dev`

Alias: `develop:feature`

Use this skill before implementing a feature.

It resolves the current project root, loads only that project's `docs/ai-coding/`, reads related source code, finds similar implementations, and then proceeds with feature work using the current project's rules.

It does not replace brainstorming or design workflows. It loads project context before those workflows run.

## Core Principles

```text
Source code is the source of truth.
AI-generated docs are context, not authority.
Each project owns its own docs/ai-coding/.
Never reuse context from another project.
Commit docs/ai-coding/ to the target project's Git repository.
Use project-feature-dev after the team lead or architect has reviewed the generated context.
```

## Installation

### Install with Skills CLI

```bash
npx skills add huajiexiewenfeng/project-coding-skills
```

For local development from this repository root:

```bash
npx skills add .
```

After installation, restart Codex or your agent runtime so the skills can be rediscovered.

### Manual Install

If your tool does not support installing skills from GitHub, copy the two skill folders into your local skills directory.

For Codex-style local skills, the structure should remain:

```text
skills/
  project-context-init/
    SKILL.md
    references/

  project-feature-dev/
    SKILL.md
    references/
```

## Usage

### 1. Initialize a project

In a business project repository:

```text
Use develop:init to initialize this project's AI coding context.
```

The skill generates `docs/ai-coding/` in that project.

### 2. Calibrate and approve the context

`project-context-init` generates a draft. Before the team uses it for feature development, a team lead or architect should review and adjust the generated files.

Review especially:

```text
docs/ai-coding/open-questions.md
docs/ai-coding/coding-rules.md
docs/ai-coding/feature-prompt-context.md
```

The reviewer should:

- resolve or explicitly keep items in `open-questions.md`
- correct project-specific rules in `coding-rules.md`
- refine the reusable team prompt in `feature-prompt-context.md`
- commit the approved `docs/ai-coding/` to the business project's Git repository

After that commit, every team member and AI agent uses the same context.

### 3. Develop a feature

In the same business project:

```text
Use develop:feature to implement this requirement: ...
```

The skill loads only the current project's `docs/ai-coding/` and follows that project's coding style.

## Existing AI Documents

`project-context-init` can read existing AI-generated context, including:

```text
docs/superpowers/specs/
docs/superpowers/plans/
graphify-out/GRAPH_REPORT.md
graphify-out/graph.json
docs/ai-coding/
docs/**/*.md
*.design.md
*.plan.md
*-design.md
*-plan.md
*prompt*.md
```

Important: these documents are supporting context only. If they conflict with source code, source code wins.

## Repository Layout

```text
skills/
  project-context-init/
    SKILL.md
    references/
      output-templates.md
      source-scan-guide.md
      ai-doc-discovery.md

  project-feature-dev/
    SKILL.md
    references/
      feature-workflow.md
      final-response-template.md
```

## Status

This project is in MVP stage.

Current focus:

- project context initialization
- project-local feature development context loading
- source-first rules
- project isolation
- Git-shared team context

Future extensions may include bugfix, review, database change, risk check, and context refresh skills.
