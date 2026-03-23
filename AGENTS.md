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

- Each skill should live in its own top-level folder (examples: `use-modern-go/`, `use-modern-javascript-typescript/`, `use-nuqs/`).
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

## Canonical Prompt Index

| Skill | Canonical prompt | Skill files to load |
| --- | --- | --- |
| `use-modern-go` | [docs/go-skill-prompt.md](docs/go-skill-prompt.md) | [use-modern-go/SKILL.md](use-modern-go/SKILL.md), [use-modern-go/references/go-style-and-design.md](use-modern-go/references/go-style-and-design.md), [use-modern-go/references/go-testing-and-concurrency.md](use-modern-go/references/go-testing-and-concurrency.md), [use-modern-go/references/modern-go-features.md](use-modern-go/references/modern-go-features.md), [use-modern-go/references/source-basis.md](use-modern-go/references/source-basis.md), [use-modern-go/agents/openai.yaml](use-modern-go/agents/openai.yaml) |
| `use-modern-javascript-typescript` | [docs/javascript-typescript-skill-prompt.md](docs/javascript-typescript-skill-prompt.md) | [use-modern-javascript-typescript/SKILL.md](use-modern-javascript-typescript/SKILL.md), [use-modern-javascript-typescript/references/style-safety-and-correctness.md](use-modern-javascript-typescript/references/style-safety-and-correctness.md), [use-modern-javascript-typescript/references/performance-and-lazy-evaluation.md](use-modern-javascript-typescript/references/performance-and-lazy-evaluation.md), [use-modern-javascript-typescript/references/pitfalls-and-anti-patterns.md](use-modern-javascript-typescript/references/pitfalls-and-anti-patterns.md), [use-modern-javascript-typescript/references/source-basis.md](use-modern-javascript-typescript/references/source-basis.md), [use-modern-javascript-typescript/agents/openai.yaml](use-modern-javascript-typescript/agents/openai.yaml) |
| `use-nuqs` | [docs/nuqs-skill-prompt.md](docs/nuqs-skill-prompt.md) | [use-nuqs/SKILL.md](use-nuqs/SKILL.md), [use-nuqs/references/parser-and-setup.md](use-nuqs/references/parser-and-setup.md), [use-nuqs/references/state-and-server-integration.md](use-nuqs/references/state-and-server-integration.md), [use-nuqs/references/performance-navigation-and-limits.md](use-nuqs/references/performance-navigation-and-limits.md), [use-nuqs/references/debugging-testing-and-troubleshooting.md](use-nuqs/references/debugging-testing-and-troubleshooting.md), [use-nuqs/references/source-basis.md](use-nuqs/references/source-basis.md), [use-nuqs/agents/openai.yaml](use-nuqs/agents/openai.yaml) |

## Canonical Prompt Workflow

When updating a skill listed in the canonical prompt index:

1. Read and reuse the `Rewritten Prompt` section from that skill's canonical prompt file.
2. Load the skill files listed in the table row.
3. Keep that skill's `references/source-basis.md` aligned with links in its canonical prompt file.
4. Update `agents/openai.yaml` only if needed and keep metadata consistent with `SKILL.md`.
5. Confirm domain coverage stays current for the skill:
- `use-modern-go`: design, errors, context, concurrency, testing, version-aware modern Go.
- `use-modern-javascript-typescript`: strict mode, erasable TypeScript syntax, immutable operations, function-first design, optional chaining/nullish coalescing, common safety/performance pitfalls.
- `use-nuqs`: typed parsers, React client-component boundaries, Next.js App Router server integration, coordinated query-state updates, navigation/history semantics, URL limits, and testing/debugging of nuqs-backed components.

## Editing Rules

- Do not invent new external sources unless requested.
- Do not add extra documentation files unless requested.
- Prefer small, focused edits over broad rewrites when the user asks for incremental updates.
