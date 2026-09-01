# AGENTS.md

## Repository purpose

AI-Prompt-Protocols is a small, high-signal, plain-text library of prompts, coding-agent rules, and workflow protocols for AI-assisted development. Every document is a complete, self-contained prompt — copy it into an agent's context and use it. The repository is intentionally Markdown-only: no build system, no dependencies, no website.

## Layout

- `MAINTENANCE.md` — prompt lifecycle and maintenance rules.
- `agent-rules/` — global behavioral rule sets (English): `codex-global-rules.md` and the tool-agnostic `universal-coding-agent-global-rules.md`. Chinese translations live in `translations/zh-CN/agent-rules/`.
- `ui-workflows/` — UI screenshot → implementation spec, and UI visual fidelity refinement (English).
- `project-workflows/project-closeout.md` — Git closeout / handoff protocol (Chinese).
- `review-workflows/expert-code-review.md` — high-signal code review protocol (English).
- `init-workflows/universal-agent-init.md` — AGENTS.md / CLAUDE.md bootstrap protocol (English).
- `README.md` — English home page; `README.zh-CN.md` — Chinese mirror with the same information architecture.

## Ground rules

- The prompt documents are curated content. Do not make unsolicited substantive rewrites to curated prompts. When a task explicitly requests a prompt update, modify the canonical prompt deliberately and follow `MAINTENANCE.md` for synchronization and verification.
- See [MAINTENANCE.md](MAINTENANCE.md) for prompt lifecycle, replacement, translation, and synchronization rules.
- English rule documents may ship Chinese translations under `translations/zh-CN/agent-rules/`; the translation must link back to the English original and treat it as canonical.
- `README.md` and `README.zh-CN.md` must stay in sync: same headings and anchors (showcase headings keep the English document names), same relative links. Verify links resolve before committing.
- Never commit local machine paths (for example absolute Windows paths), personal knowledge-vault paths, or credentials.
- Do not add websites, frameworks, logos, or badge walls; keep the repository plain Markdown.
- Review the final diff before committing; commit only task-owned changes.

## Commands

No build, test, or lint tooling exists. Usual workflow: edit Markdown, verify links and privacy, `git add` the intended files, commit with a concise conventional message (`feat:` / `docs:`), and push to `origin/main`.
