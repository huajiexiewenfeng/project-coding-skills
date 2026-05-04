# AI Document Discovery

## Goal

Use existing AI-generated documents to restore project context, while treating source code as the authority.

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
*.design.md
*.plan.md
*_design.md
*_plan.md
*spec*.md
*plan*.md
```

## Priority

Read in this order:

1. Source code, config, build files, tests.
2. Project-maintained human docs.
3. Superpower specs and plans.
4. Graphify reports.
5. Other AI-generated temporary docs.

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
