# AGENTS.md

## Repository Purpose
AI-Prompt-Protocols is a plain-Markdown library of self-contained prompts, coding-agent rules, and workflow protocols. Intentionally Markdown-only: no build system, no dependencies, no website, no badge walls.

## Ground Rules
- Prompts are curated content; do not make unsolicited substantive rewrites. When explicitly updating a prompt, follow [MAINTENANCE.md](MAINTENANCE.md) for lifecycle, language, replacement, and README showcase sync.
- Keep `README.md`, navigation, showcase descriptions, and maintenance docs in English (no bilingual mirrors by default). Preserve each prompt in its original language unless translation is requested.
- Several prompts (including `agent-rules/`) are sanitized mirrors of the maintainer's private upstream collection: apply substantive changes upstream first, then mirror verbatim with private details removed.
- **Privacy hard rule**: never commit local machine paths (e.g. absolute Windows paths), personal knowledge-vault paths, or credentials. Verify all relative links before committing.

## Workflow & Authorization
- No build, test, or lint tooling exists. Edit Markdown → verify links and privacy → review diff → commit with conventional prefix (`feat:` / `docs:`).
