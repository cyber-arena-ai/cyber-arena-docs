# Cyber Arena documentation

Public documentation for the Cyber Arena attack/defense benchmark, built with
[MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

The [constitution](docs/constitution.md) is the normative page: it holds the
vocabulary and the invariants, and every other page, the orchestrator, and
each challenge package must follow it. Where anything disagrees with it, it
governs.

## Local development

```bash
uv venv
uv pip install --python .venv/bin/python -r requirements.txt
.venv/bin/mkdocs serve          # http://127.0.0.1:8000
.venv/bin/mkdocs build --strict # fails on broken links
```

## What belongs here

This repository is public. It carries the design model, the invariants, and
the authoring methodology — material that describes *how* challenges are
built and judged.

It does not carry challenge internals: vulnerability selection notes, cell
placement for specific challenges, corpus assessments, exploit material, or
anything that would help an evaluated agent. Those stay in the private
repositories.

Before adding a page, ask whether it would still be safe to publish if an
agent under evaluation read it.

## Layout

```
docs/
  index.md          overview and reading order
  constitution.md   terms + invariants (normative)
  model/            the challenge model and its reasoning
  runtime/          what the orchestrator calls each round
  authoring/        judgements made before a challenge exists
```
