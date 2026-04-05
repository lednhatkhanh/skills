# Personal Skills Workspace

This repo is a workspace for your personal AI skills.

## What's Here

- Skill folders at the repository root (current examples: `use-modern-go/`, `use-modern-javascript-typescript/`, `use-nuqs/`)
- Per-skill `SKILL.md` files for concise operational instructions
- Optional per-skill `references/` and `agents/` folders for deeper guidance and metadata
- `docs/` for shared or skill-specific prompt assets (current examples: `docs/go-skill-prompt.md`, `docs/javascript-typescript-skill-prompt.md`, `docs/nuqs-skill-prompt.md`)

## Current Skills

| Skill | Focus | Canonical Prompt |
| --- | --- | --- |
| `use-modern-go` | modern Go design, style, testing, concurrency, and version-aware guidance | `docs/go-skill-prompt.md` |
| `use-modern-javascript-typescript` | strict, modern JS/TS guidance with function-first design, erasable TS syntax, immutable defaults, and pitfall prevention | `docs/javascript-typescript-skill-prompt.md` |
| `use-nuqs` | type-safe URL query state for React + TypeScript, with Next.js 16+ App Router integration, parser discipline, navigation semantics, and testing/debugging guidance | `docs/nuqs-skill-prompt.md` |

## Quick Update Flow (Any Skill)

1. Pick the target skill directory.
2. Update `SKILL.md` first, then adjust relevant `references/` and `agents/` files if needed.
3. Keep top-level instructions concise and route details to references.
4. Review changes and run your normal checks before commit.

## Skill Starter Template

Use this structure for any new skill:

```text
<skill-name>/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── <topic>.md
```

Minimal `SKILL.md` starter:

```md
---
name: <skill-name>
description: <When this skill should be used and what it does>
---

# <Skill Title>

## Purpose

<Short description of the outcome this skill produces.>

## Workflow

1. <Step 1>
2. <Step 2>
3. <Step 3>

## References

- Read `references/<topic>.md` when <condition>.
```

Naming convention:

- Use lowercase letters, digits, and hyphens only (example: `api-design-review`).

## Canonical Prompt Index

| Skill | Canonical prompt to use when updating |
| --- | --- |
| `use-modern-go` | `docs/go-skill-prompt.md` |
| `use-modern-javascript-typescript` | `docs/javascript-typescript-skill-prompt.md` |
| `use-nuqs` | `docs/nuqs-skill-prompt.md` |

## Full Skill Audit Prompt

Copy-paste the prompt below to an agent to run a complete audit and update cycle on a skill. Replace `<SKILL_NAME>` with the target (e.g., `use-modern-go`, `use-modern-javascript-typescript`, `use-nuqs`).

````text
Audit, verify, and optimize the `<SKILL_NAME>` skill in this repository. This is a multi-step task — do all of it.

## Context

- This repo contains AI coding skills at the root level (e.g., `use-modern-go/`, `use-modern-javascript-typescript/`, `use-nuqs/`).
- Each skill has a `SKILL.md`, `references/` directory, and `agents/` directory.
- Each skill has a canonical prompt file in `docs/` (see `README.md` canonical prompt index).
- Skills target AI coding agents, not humans. Optimize for: concise directive rules, minimal context window usage, no explanatory prose.
- Read `AGENTS.md` for workspace baselines, editing rules, and the canonical prompt workflow.

## Step 1: Read everything

1. Read `AGENTS.md` for workspace baselines and conventions.
2. Read the skill's canonical prompt file from `docs/` (find it in the canonical prompt index in `README.md`).
3. Read all skill files: `SKILL.md`, every file in `references/`, and `agents/openai.yaml`.
4. Read `references/source-basis.md` to get the list of upstream source URLs.

## Step 2: Fetch and verify all upstream references

1. Fetch every URL listed in the canonical prompt's "Source Basis" section and in `references/source-basis.md`.
   - For GitHub URLs, use raw.githubusercontent.com.
   - For documentation sites, fetch the page content.
2. Extract key facts: API signatures, feature lists, version numbers, patterns, configuration options.
3. Compare what the upstream sources say against what the skill files currently contain.
4. Note: missing features, incorrect information, outdated APIs, renamed options, new APIs not yet covered.

## Step 3: Fix and update

Apply all fixes and updates to the skill files. Follow these rules strictly:

**Content rules:**
- Fix any incorrect information found in Step 2.
- Add any missing features, APIs, or patterns discovered from upstream sources.
- Remove any content that is no longer accurate or has been superseded.
- Never add fallback code, polyfills, compatibility branches, or legacy workarounds for older runtimes/browsers. Always use modern syntax and APIs.
- Update version numbers, API names, and option names to match current upstream docs.

**Context optimization rules:**
- SKILL.md is the single source of truth for rules, baselines, and validation gates. It is always loaded by the agent.
- Reference files add depth (examples, config, patterns, lookup tables) — they must NOT restate rules already in SKILL.md.
- Each reference file should start with a note like: "Rules from SKILL.md are not repeated here."
- No TOC sections in references (waste of context).
- No review/reliability/consistency checklists in references (they duplicate SKILL.md rules).
- No scope-baseline sections in references (baseline is in SKILL.md).
- Use tables instead of prose for anti-pattern → fix mappings, feature matrices, and option lists.
- Keep code examples minimal — one "Prefer" example per pattern, no "Avoid" examples unless the wrong pattern is genuinely non-obvious.
- Merge small sections (under 5 bullets) into related sections.

**Structural rules:**
- Update `references/source-basis.md` with any new URLs discovered and set "Last reviewed" to today's date.
- Update the canonical prompt file in `docs/` if the requirements or source list changed.
- Update `agents/openai.yaml` only if the skill description changed.
- Keep `SKILL.md` Never/Always rule format.

## Step 4: Validate

1. Re-read all updated files and verify internal consistency:
   - No rule in a reference file that is already stated in SKILL.md.
   - No baseline/scope restated outside SKILL.md.
   - No stale API names, removed options, or incorrect version numbers.
   - All upstream source URLs in `source-basis.md` match the canonical prompt's source list.
2. Report a summary: what was added, what was fixed, what was removed, and the before/after character count for each file.
````
