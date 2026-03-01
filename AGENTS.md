# AGENTS.md

## Purpose

Provide a repeatable workflow for agents to manage this repository as a personal skills workspace.

## When To Apply

Use this workflow when the user asks to:
- add, update, regenerate, or improve any skill in this repository
- align a skill with a style guide, reference, or source set
- refresh best practices for an existing skill

## Repository Scope

This repository is for multiple personal skills, not only Go.

- Each skill should live in its own top-level folder (example: `use-modern-go/`).
- Each skill should keep concise top-level instructions in `SKILL.md`.
- Detailed guidance should live in skill-local `references/` files when needed.
- Metadata should live in skill-local `agents/` files when needed.

## General Skill Update Workflow

1. Identify the target skill directory.
2. Load the current skill files (`SKILL.md`, then only relevant `references/*` and `agents/*` files).
3. Apply requested updates with progressive disclosure:
- keep `SKILL.md` concise and procedural
- keep detailed guidance in `references/*.md`
- route deeper detail from `SKILL.md` to references
4. Update metadata only if needed, keeping name/description consistent with `SKILL.md`.
5. Validate quality before finishing:
- guidance is actionable and enforceable, not generic
- non-negotiable rules and output expectations are clear
- scope and references are current for the skill’s domain

## New Skill Starter Template

When creating a new skill, use:

```text
<skill-name>/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── <topic>.md
```

Initialize `SKILL.md` with this minimum frontmatter:

```yaml
---
name: <skill-name>
description: <When this skill should be used and what it does>
---
```

Naming rules:

- Use lowercase letters, digits, and hyphens only.
- Keep names short and action-oriented.

## Go-Specific Canonical Prompt (use-modern-go)

When updating `use-modern-go`, treat [docs/go-skill-prompt.md](docs/go-skill-prompt.md) as the canonical source.

1. Read and reuse the `Rewritten Prompt` section from [docs/go-skill-prompt.md](docs/go-skill-prompt.md).
2. Load:
- [use-modern-go/SKILL.md](use-modern-go/SKILL.md)
- [use-modern-go/references/go-style-and-design.md](use-modern-go/references/go-style-and-design.md)
- [use-modern-go/references/go-testing-and-concurrency.md](use-modern-go/references/go-testing-and-concurrency.md)
- [use-modern-go/references/modern-go-features.md](use-modern-go/references/modern-go-features.md)
- [use-modern-go/references/source-basis.md](use-modern-go/references/source-basis.md)
3. Keep `source-basis.md` aligned with source links in `docs/go-skill-prompt.md`.
4. Update metadata only if needed:
- [use-modern-go/agents/openai.yaml](use-modern-go/agents/openai.yaml)
5. Confirm coverage includes design, errors, context, concurrency, testing, and version-aware modern Go.

## Editing Rules

- Do not invent new external sources unless requested.
- Do not add extra documentation files unless requested.
- Prefer small, focused edits over broad rewrites when the user asks for incremental updates.
