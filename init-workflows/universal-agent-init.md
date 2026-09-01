# Universal Agent Init

> **Purpose:** Initialize or improve a repository's persistent agent instructions (`AGENTS.md` / `CLAUDE.md`) from repository evidence, keeping them lean and verified.
> **Audience:** Coding agents asked to set up or refine repository agent instructions (Codex, Claude Code, Cursor, and similar).

Initialize or improve this repository's persistent AI-agent instructions.

Leave the repository with a concise, accurate root `AGENTS.md` for shared guidance and a root `CLAUDE.md` as the Claude Code entry point. Preserve useful existing instructions, avoid duplication, verify what you add, and locally commit only task-owned changes.

## 1. Inspect first

Work from the repository root; if Git is available, resolve the Git root rather than assuming the current directory is correct.

Before editing, inspect the working tree and note pre-existing staged, unstaged, or untracked changes, especially to `AGENTS.md` or `CLAUDE.md`. Never reset, discard, overwrite, stash, or silently include unrelated user work.

Derive instructions from repository evidence. Inspect what is relevant and available:

- structure, entry points, README/CONTRIBUTING, and architecture/development docs
- package/build config, scripts, formatter, lint, tests, CI/workflows, and recent Git history
- sources of truth, generated-code boundaries, and representative code conventions
- existing root or nested `AGENTS.md`, `AGENTS.override.md`, `CLAUDE.md`, `.claude/CLAUDE.md`, `.claude/rules/`, and other repository-local agent rules

Use other repository-local instructions only as evidence and for contradiction checks; do not broadly rewrite them.

Never copy personal, machine-local, organization-global, system, developer, or outside-repository instructions into committed files. Do not use `~/.codex/AGENTS.md`, `~/.claude/CLAUDE.md`, `CLAUDE.local.md`, or similar private/global files as repository source material.

Do not invent commands, paths, architecture, policies, tools, or conventions you cannot verify.

## 2. AGENTS.md

Use the root `AGENTS.md` for repository-wide guidance. For a simple repository, aim roughly for 200–400 words; exceed that only when complexity justifies it.

Include only durable, actionable information future agents repeatedly need:

- project purpose, architecture boundaries, important directories, and sources of truth
- exact build, run, test, lint, format, or validation commands
- non-obvious repository-specific conventions
- generated files or areas that must not be edited directly
- important workflow, compatibility, security, or modification constraints
- verification expectations and costly/non-obvious failure modes

Prefer concrete condition → action rules over vague advice. Point to authoritative files instead of copying long policies. Keep narrow subdirectory guidance in its nested scope rather than flattening it into the root.

If `AGENTS.md` already exists, read it completely. Preserve meaningful intent and make only useful changes: fix stale facts, add verified omissions, remove obsolete guidance, resolve contradictions, make vague rules concrete, or remove duplication. Prefer the smallest effective diff; if it is already good, leave it unchanged.

If a root `AGENTS.override.md` exists, recognize that it takes precedence for Codex. Do not delete it blindly. Reconcile it only when its intent is clear and safe; otherwise preserve it and report that it may shadow `AGENTS.md`.

## 3. CLAUDE.md

Claude Code reads `CLAUDE.md` and supports `@path` imports. Shared repository guidance should not be duplicated.

If no root `CLAUDE.md` exists, normally create exactly:

```md
@AGENTS.md
```

Add content below the import only for genuine Claude Code-specific repository instructions.

If `CLAUDE.md` already exists, read it completely. Preserve meaningful Claude-specific guidance. Where safe and semantically clear, replace duplicated shared guidance with `@AGENTS.md` and keep only Claude-specific material.

Inspect `.claude/CLAUDE.md` and `.claude/rules/` when present so the root file does not create avoidable conflicts or duplication. Do not rewrite them unless genuinely required.

## 4. Handle existing files conservatively

- Neither root file exists: create `AGENTS.md`, then `CLAUDE.md` importing it.
- Only one exists: preserve and improve it only where useful, create the missing file, and consolidate shared guidance only where safe.
- Both exist: review both, preserve useful instructions, reduce duplication, and keep shared guidance in `AGENTS.md`.

Never blindly overwrite either file.

## 5. Keep instructions lean

Persistent instructions consume model context. Do not add generic advice such as “write clean code,” “use best practices,” “test thoroughly,” or “avoid bugs” unless it has concrete repository-specific meaning.

Do not restate README, formatter, linter, CI, or contribution rules when an exact command or short pointer is sufficient. Do not create extra rules, hooks, skills, MCP config, custom agents, or other tooling unless this repository clearly requires it for this task.

## 6. Verify

Before finishing:

- re-read final `AGENTS.md` and `CLAUDE.md`
- verify every added path, command, filename, and claimed convention against repository evidence
- confirm `@AGENTS.md` resolves to the intended root file
- check for stale, contradictory, duplicated, speculative, overly broad, or incorrectly scoped guidance
- ensure no secrets, credentials, private paths, personal information, or private/global agent instructions were copied into committed files
- inspect the final Git diff, run `git diff --check` when Git is available, and confirm unrelated files were not modified

Do not install dependencies, rewrite lockfiles, or perform unrelated setup solely to validate this documentation task.

## 7. Commit safely

If this is a Git repository and meaningful changes were made, create a local commit automatically only when task-owned changes can be isolated safely.

- Never include unrelated pre-existing changes.
- Commit only instruction files changed by this task, using explicit pathspecs so unrelated staged files are excluded.
- If a target file already had uncommitted user changes, preserve them. Commit only task-owned hunks if they can be separated safely; otherwise leave the result uncommitted and report why.
- Do not amend an existing commit unless explicitly requested, and do not create an empty commit.

Use a concise repository-appropriate message such as `docs: initialize agent instructions` or `docs: refine agent instructions`.

If this is not a Git repository, skip the commit. Never push, create a pull request, or modify remote state unless explicitly requested.

## 8. Final response

Briefly report the state of `AGENTS.md` and `CLAUDE.md`, the main improvements, verification performed, any important precedence issue such as `AGENTS.override.md`, and the commit hash/message or exact reason no commit was made.

Do the work rather than only describing what should be done.
