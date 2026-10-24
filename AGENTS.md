# AGENTS.md

## Repository purpose

AI-Prompt-Protocols is a small, high-signal, plain-text library of prompts, coding-agent rules, and workflow protocols for AI-assisted development. Every document is a complete, self-contained prompt — copy it into an agent's context and use it. The repository is intentionally Markdown-only: no build system, no dependencies, no website.

## Layout

- `MAINTENANCE.md` — prompt lifecycle and maintenance rules.
- `agent-rules/` — global behavioral rule sets (English): `codex-global-rules.md` and the tool-agnostic `universal-coding-agent-global-rules.md`.
- `ui-workflows/` — UI screenshot → implementation spec, and UI visual fidelity refinement (English).
- `project-workflows/` — Git closeout / handoff, repository audit–repair–delivery, and user-selected local recycle-bin cleanup (Chinese).
- `review-workflows/` — high-signal code review (English) and independent whole-repository audit (Chinese).
- `init-workflows/universal-agent-init.md` — AGENTS.md / CLAUDE.md bootstrap protocol (English).
- `README.md` — English home page and protocol showcase.

## Ground rules

- The prompt documents are curated content. Do not make unsolicited substantive rewrites to curated prompts. When a task explicitly requests a prompt update, modify the canonical prompt deliberately and follow `MAINTENANCE.md` for synchronization and verification.
- See [MAINTENANCE.md](MAINTENANCE.md) for prompt lifecycle, language policy, replacement, and showcase maintenance rules.
- Use English for README, navigation, showcase descriptions, and repository maintenance documentation. Do not create or maintain bilingual mirrors by default.
- Preserve each prompt in its original language; do not translate or rewrite prompts unless explicitly requested. Existing prompt translations are preserved, but automatic translation synchronization is not required.
- Link directly to the original prompt and verify relative links before committing.
- Never commit local machine paths (for example absolute Windows paths), personal knowledge-vault paths, or credentials.
- Do not add websites, frameworks, logos, or badge walls; keep the repository plain Markdown.
- Review the final diff before committing; commit only task-owned changes.

## Commands

No build, test, or lint tooling exists. Usual workflow: edit Markdown, verify links and privacy, `git add` the intended files, commit with a concise conventional message (`feat:` / `docs:`), and push to `origin/main`.
