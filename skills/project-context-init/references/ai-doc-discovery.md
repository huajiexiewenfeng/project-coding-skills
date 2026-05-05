# AI Document Discovery

## Goal

Use existing AI-generated documents to restore project context, while treating source code as the authority. Treat prompt templates with placeholders as feature-intake templates, not project facts.

## Discovery Paths

Search inside the resolved project root only:

```text
docs/superpowers/specs/
docs/superpowers/plans/
docs/superpowers/
graphify-out/GRAPH_REPORT.md
graphify-out/graph.json
graphify-out/
docs/ai-coding/
docs/**/*.md
*.design.md
*.plan.md
*-design.md
*-plan.md
*_design.md
*_plan.md
*prompt*.md
**/*prompt*.md
**/*design*.md
**/*plan*.md
*spec*.md
*plan*.md
```

## Priority

Read in this order:

1. Source code, config, build files, tests.
2. Project-maintained human docs.
3. Project-maintained design, enum, API, and adapter docs under `docs/`.
4. Superpower specs and plans.
5. Graphify reports.
6. Other AI-generated temporary docs.

## Document Classification

Classify every discovered document before using it:

| Class | Examples | How to use |
|---|---|---|
| `factual-context` | architecture notes, API notes, enum design, module guides | Verify claims against source code before promoting to rules. |
| `design-note` | specs, plans, ADR-like docs | Treat as intent or rationale; verify current implementation. |
| `graph-report` | `graphify-out/GRAPH_REPORT.md` | Use as a navigation aid and confidence-marked supporting context. |
| `prompt-template` | `*prompt*.md` with placeholders such as `<feature>`, `<branch>`, `<API URL>` | Register as an intake template. Do not treat placeholder text or per-feature examples as project facts. |

## Prompt Template Rules

When a document is a prompt template:

- Record it in `ai-context-sources.md` with source type `prompt-template`.
- Summarize its intended use and required placeholders.
- Do not extract project coding rules from placeholder sections.
- Do not put per-feature variables into `coding-rules.md`.
- Do not copy the full template into `feature-prompt-context.md`.
- Add a manual review item asking the team lead or architect to decide which fields become the approved feature intake.
- If the template contains stable project constraints, verify those constraints against source code or human-maintained docs before promoting them.

## Graphify Rules

First read:

```text
graphify-out/GRAPH_REPORT.md
```

Do not read all of `graphify-out/graph.json` unless needed. If reading graph data, prefer targeted lookup and summarize only relevant nodes and edges.

Respect graphify confidence:

- `EXTRACTED`: may be treated as observed from source corpus, but still verify against source code when used as a rule.
- `INFERRED`: mark as inference.
- `AMBIGUOUS`: write to `open-questions.md`.

Do not run graphify in MVP. Only read existing graphify outputs.

## Conflict Handling

When AI docs conflict with source code:

- Use source code as truth.
- Record the conflict in `open-questions.md`.
- Do not put the AI-doc claim into `coding-rules.md`.
- Do not put the AI-doc claim into `feature-prompt-context.md`.

## ai-context-sources.md Entries

For every useful AI document, record:

- Path.
- Source type.
- Summary.
- Trust level.
- Confirmed facts.
- Unconfirmed claims.
- Required placeholders, if the source is a prompt template.
