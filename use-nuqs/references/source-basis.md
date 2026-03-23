# Source Basis

Last reviewed: 2026-03-23

## Canonical Inputs Used for This Skill

Local canonical prompt:

- /home/klaus_le/personal-workspace/skills/docs/nuqs-skill-prompt.md

Primary upstream sources:

- https://github.com/pproenca/dot-skills/blob/master/skills/.curated/nuqs/SKILL.md
- https://nuqs.dev/docs/basic-usage
- https://nuqs.dev/docs/options
- https://nuqs.dev/docs/batching
- https://nuqs.dev/docs/tips-tricks
- https://nuqs.dev/docs/debugging
- https://nuqs.dev/docs/testing
- https://nuqs.dev/docs/troubleshooting
- https://nuqs.dev/docs/server-side
- https://nuqs.dev/docs/limits
- https://nuqs.dev/docs/parsers/built-in
- https://nuqs.dev/docs/parsers/making-your-own

Supporting upstream documentation reviewed while distilling the skill:

- https://github.com/47ng/nuqs/blob/next/README.md

## Scope Decisions Encoded in This Skill

- React 19+ only
- TypeScript 5.9+ only
- Next.js 16+ App Router only when Next.js is present
- Chrome 146+ / Firefox 148+ / Safari 26+ browser baseline
- Pages Router and non-React framework guidance intentionally excluded from procedural guidance

## Notes

- The GitHub-hosted community nuqs skill and all `references/*.md` files linked from it were used as the rule inventory and prioritization guide.
- Official nuqs docs were used to constrain behavior around parser defaults, debugging, testing, App Router server-side integration, and browser limits.
- Pages Router and non-Next adapter rules from the upstream inventory were reviewed and intentionally kept out of procedural guidance for this App Router-only skill.
- Legacy browser compatibility guidance was intentionally omitted in favor of the supported evergreen browser baseline.
- The skill intentionally keeps `SKILL.md` procedural and pushes detailed parser/server/testing guidance into focused references.
