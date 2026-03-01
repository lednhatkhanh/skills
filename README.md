# Personal Skills Workspace

This repo is a workspace for your personal AI skills.

## What's Here

- Skill folders at the repository root (current example: `use-modern-go/`)
- Per-skill `SKILL.md` files for concise operational instructions
- Optional per-skill `references/` and `agents/` folders for deeper guidance and metadata
- `docs/` for shared or skill-specific prompt assets (current example: `docs/go-skill-prompt.md`)

## Current Skills

- `use-modern-go`: modern Go design, style, testing, concurrency, and version-aware guidance

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

## Go Skill Canonical Prompt

For `use-modern-go` updates, use the rewritten prompt in `docs/go-skill-prompt.md`.
