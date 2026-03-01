# AGENTS.md

## Purpose

Provide a repeatable workflow for agents to update the `use-modern-go` skill using the canonical prompt in [docs/go-skill-prompt.md](docs/go-skill-prompt.md).

## When To Apply

Use this workflow when the user asks to:
- update, regenerate, or improve the Go skill
- align the Go skill with Effective Go / Google Go style guidance
- refresh modern Go best practices in this repository

## Canonical Prompt Source

- Read and reuse the `Rewritten Prompt` section from [docs/go-skill-prompt.md](docs/go-skill-prompt.md).
- Treat that prompt as the single source of truth for update intent and scope.

## Auto-Update Workflow

1. Load current files:
- [use-modern-go/SKILL.md](use-modern-go/SKILL.md)
- [use-modern-go/references/go-style-and-design.md](use-modern-go/references/go-style-and-design.md)
- [use-modern-go/references/go-testing-and-concurrency.md](use-modern-go/references/go-testing-and-concurrency.md)
- [use-modern-go/references/modern-go-features.md](use-modern-go/references/modern-go-features.md)
- [use-modern-go/references/source-basis.md](use-modern-go/references/source-basis.md)

2. Apply the rewritten prompt from docs to update the skill content:
- keep `SKILL.md` concise and procedural
- keep detailed guidance in `references/*.md`
- preserve progressive disclosure (routing from `SKILL.md` to references)

3. Ensure `source-basis.md` remains aligned with the source links in docs.

4. Update metadata only if needed:
- [use-modern-go/agents/openai.yaml](use-modern-go/agents/openai.yaml)
- keep name/description consistent with `SKILL.md`

5. Validate quality before finishing:
- guidance is actionable and enforceable, not generic
- coverage includes design, errors, context, concurrency, testing, and version-aware modern Go
- non-negotiable rules and output expectations are clear

## Editing Rules

- Do not invent new external sources unless requested.
- Do not add extra documentation files for the skill unless requested.
- Prefer small, focused edits over broad rewrites when the user asks for incremental updates.
