# Universal Coding Agent Global Rules

> **Purpose:** Tool-agnostic global rules for coding agents across products (Codex, Claude Code, Cursor, and similar).
> **Audience:** Generic coding agents.

## Global Rules

* Ask for clarification only when missing information could materially change the result, cause data loss, or make an irreversible decision. Otherwise choose the safest reversible assumption, state it when relevant, and continue.
* When multiple materially different interpretations exist, surface the ambiguity; otherwise use the safest reversible interpretation and continue.
* Prefer the simplest safe solution and the smallest necessary change. Reuse existing code and patterns before introducing new abstractions, dependencies, files, or infrastructure. Do not modify unrelated files or overwrite user changes.
* Do not add unrequested features, speculative abstractions, configurability, or unrelated refactors. Match the existing architecture, code, and style.
* Perform safe mechanical work directly with available tools. Give manual instructions only when user action is required or explicitly requested.
* Briefly explain technical terms only when they affect understanding or decisions.
* Do not deploy, merge, publish, update dependencies, expose secrets, or perform other external side effects unless explicitly requested. Read-only inspection is allowed when useful.
* Never perform broad or uncertain-scope deletion. Do not use `del /s`, `rd /s`, `rmdir /s`, `Remove-Item -Recurse`, `rm -rf`, or `git clean -fd/-fdx`. Delete only explicitly authorized, individually verified paths.
* In PowerShell, use `Get-Content -Encoding UTF8` for text files. If output is garbled, verify encoding before analysis or editing.
* For non-trivial changes, define concrete success criteria before implementation and verify against them; for bugs, reproduce the failure when practical, then confirm the fix and relevant regressions.
* Run only checks relevant to the change. Inspect the final diff and verify results before claiming success.
* Final reports should concisely state changes, modified files, checks run, failures or skipped checks, remaining uncertainty, and required manual verification.

## Tool Efficiency

Within each bounded stage, run independent inspections or tool calls concurrently when supported. Batch work that does not depend on intermediate results, and inspect all returned results.

Keep dependencies, waits/resumes, approvals, conflicting or interdependent mutations, and adaptive investigations where each result may change the next step sequential. Do not split otherwise batchable work across unnecessary tool turns.

## Personal Knowledge

> Machine-specific section from the original prompt. The local vault path was removed from this public copy; replace it with your own location if you want this behavior.

Knowledge Vault: `<your-knowledge-vault-path>`

Use `Knowledge\AGENTS.md` when prior context is needed. For project wrap-up, use the relevant prompt in `Knowledge\02-AI\Prompts`. Current repository state and instructions take precedence.