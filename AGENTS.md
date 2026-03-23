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

## Workspace Baselines

Unless a user explicitly asks otherwise, regenerate and update skills against these baselines:

- React 19+
- Next.js 16+ App Router only
- TypeScript 5.9+
- Go 1.26+
- Rust 1.94+
- Node.js 24+

Supported browsers:

- Chrome 146+
- Firefox 148+
- Safari 26+

Baseline policy:

- Do not add compatibility branches, downgrade guidance, polyfill advice, or legacy framework/router coverage for older language, runtime, or browser versions unless explicitly requested.
- Treat modern platform capabilities as available by default across the supported browser set above.
- Exception: keep [use-modern-go/references/modern-go-features.md](use-modern-go/references/modern-go-features.md) as a version-complete reference covering best practices by Go version. Other Go skill files should still default to the modern baseline unless an older module target is explicitly relevant.

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
- default guidance reflects the workspace baselines above unless the user explicitly asks for a different target

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
6. Apply the workspace baselines during regeneration:
- `use-modern-go`: default to Go 1.26+ guidance in `SKILL.md` and non-version-history references; keep `references/modern-go-features.md` version-complete.
- `use-modern-javascript-typescript`: default to TypeScript 5.9+, Node.js 24+, React 19+, Next.js 16+ App Router when applicable, and supported evergreen browsers only.
- `use-nuqs`: default to React 19+, TypeScript 5.9+, Next.js 16+ App Router only, and supported evergreen browsers only.

## Editing Rules

- Do not invent new external sources unless requested.
- Do not add extra documentation files unless requested.
- Prefer small, focused edits over broad rewrites when the user asks for incremental updates.
- Prefer removing stale legacy compatibility guidance over preserving it “just in case.”
