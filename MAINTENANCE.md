# MAINTENANCE.md

Prompt lifecycle and long-term maintenance rules for this repository.

These rules govern how prompts are added, modified, replaced, renamed, translated, synced, deprecated, and deleted. Keep them simple — Git history, clear commits, and stable paths are the version system.

## 1. Source of Truth

### Canonical Prompts

The prompt documents under the category directories are the source of truth for behavior and semantics:

```text
agent-rules/
ui-workflows/
project-workflows/
review-workflows/
init-workflows/
```

### Derived Content

Derived from canonical prompts:

- `translations/...` — Chinese translations of English rule documents
- `README.md` showcase — What it does, Why it works, Preview, Read links
- `README.zh-CN.md` corresponding sections

When a canonical prompt changes, check whether the derived content needs synchronization. README and translations are never the source of truth.

## 2. Workflow

Every maintenance task follows:

```text
inspect → understand → edit canonical source → sync derived content → verify → review diff → commit
```

One logical change = one clear commit. Do not bundle unrelated prompt changes into a single commit. Never force push.

## 3. Adding a Prompt

Before adding a prompt, ask:

1. Does it solve a real, distinct problem?
2. Is it already covered by an existing prompt?
3. Does it deserve to be a long-term maintained document?
4. Which existing category does it belong to?

Prefer reusing existing categories. Do not create a new category for a single prompt; create one only when existing categories clearly cannot express a new long-term category.

Every new prompt has at least:

```md
# Name

> **Purpose:** ...
> **Audience:** ...
```

followed by the complete, ready-to-use prompt.

After adding, check: README showcase, README.zh-CN, Repository Structure, AGENTS.md Layout (if it maintains a concrete list), and translation needs.

## 4. Modifying a Prompt

Read the full prompt first. Do not regenerate the whole file from a one-line request.

Prefer the smallest change that fully achieves the requested improvement.

Preserve: verified rules, original structure, constraint strength, important edge conditions, special exceptions, existing design intent.

Allowed on explicit request: add rules, remove stale rules, change behavior, restructure, remove duplication, fix ambiguity, improve executability.

You must be able to explain why the change is better than the previous behavior. Do not rewrite merely because another wording "looks nicer".

### Refinement

Small, safe improvements: clarify ambiguity, add missing edge cases, remove duplication, improve execution order, fix wrong rules, improve clarity.

Keep the same file and path.

### Behavioral Change

Changes the core strategy, the default behavior, removes important constraints, changes permission / Git / security policy, changes the output protocol, or changes scope. Be extra careful.

After a behavioral change, explicitly check:

- README "Why it works" is still correct
- Preview still represents the current version
- Chinese translation is not outdated
- Purpose / Audience are still accurate

The commit message must reflect that this is a substantive change, not formatting.

## 5. Replacing a Prompt

If the new version solves the same problem and is simply better: **update the original file in place.** Keep the stable path.

Do not create `-v2.md`, `-final.md`, `-new.md`, or similar duplicates. Git already keeps the history; stable paths matter more than keeping historical copies in the tree.

Create a new prompt only when it serves a different purpose, the behavior model is clearly different, or both versions are worth keeping independently.

## 6. No Manual Backups

Do not create `old/`, `backup/`, `previous/`, `archive-copy/`, `xxx-old.md`, `xxx-backup.md` merely to "protect against a bad new version". Use `git log` and `git diff` before editing; restore from Git history when needed.

Only a deprecated prompt with independent public value gets a formal Deprecated / Archive status (see §9).

## 7. Renaming and Moving

For renames or moves, use a Git-aware rename (`git mv`), then search the whole repository for the old path and update:

- README.md
- README.zh-CN.md
- translations
- AGENTS.md
- cross-references in other prompts

Never leave broken links.

## 8. Translation Synchronization

Canonical source wins. Every translation must link back to the canonical source.

After a canonical change, decide whether the semantics changed:

- Markdown cleanup, typos, or non-semantic metadata: no mechanical full re-translation needed.
- Semantic rule changes: sync the corresponding translation.

Do not rewrite translation paragraphs that did not change. Translations must preserve the original strength of `must` / `never` / `should` / `do not`, conditions, exceptions, and risk levels. A translation is not a "softer explanation".

## 9. Deprecating a Prompt

If a prompt is fully superseded, first decide whether the public version still has value:

- No value: delete the file; Git history keeps it.
- Compatibility or historical value: keep it and mark it at the top:

```md
> **Status:** Deprecated
> **Superseded by:** [New Prompt](...)
```

README no longer presents deprecated prompts as primary recommendations. Do not keep multiple prompts with unclear "which is latest" status visible long-term.

## 10. Deleting a Prompt

Before deleting, check: README links, translations, cross-references in other prompts, AGENTS.md references, and whether a replacement exists.

When deleting a canonical prompt, also remove translations and showcase content that no longer make sense. Do not leave orphan files.

## 11. README Synchronization

README is the presentation layer, not the prompt source of truth.

After every substantive prompt change, check: name, Purpose, What it does, Why it works, Best for, Works with, Preview, full-prompt link, and the Chinese entry. Update only the affected parts.

The README Preview must come from the current prompt — never reference rules that were deleted or changed.

README.md and README.zh-CN.md keep the same information architecture and factual content. Not a word-for-word translation, but no factual divergence.

## 12. AGENTS.md Responsibility

AGENTS.md stays concise: key principles only, linking to this file:

> See MAINTENANCE.md for prompt lifecycle, replacement, translation, and synchronization rules.

## 13. No Version System (for now)

Do not add: SemVer per prompt, per-file version numbers, a manifest/database, README generation scripts, npm/Python tooling, CI pipelines, or release automation. Git history + clear commits + stable paths are enough at this scale. Revisit automation only when the repository actually grows.

## 14. Git Workflow

Before each maintenance task: inspect → understand → edit canonical → sync derived → verify → review diff → commit.

Do not commit half-finished changes as multiple meaningless commits. One logical change, one clear commit.

Recommended messages:

```text
docs: refine <name> protocol
docs: add prompt maintenance workflow
docs: update <name> protocol
docs: replace outdated <name> rules
docs: sync Chinese translation
```

Do not bundle unrelated prompt changes into one commit. Never force push.

## 15. Verification Checklist

After any add / modify / replace / delete, check at least:

### Canonical

- target prompt is complete; no accidental cuts
- rules are not contradictory
- Purpose / Audience still correct

### Derived

- translation needs synchronization
- README showcase is accurate
- Preview is still valid
- README Chinese version is in sync

### Repository

- all relative links exist
- no orphan translations
- no stale path references
- no private local machine paths (for example absolute Windows drive-letter paths)
- no credentials / secrets
- no unrelated modifications

### Git

Review `git diff` and `git status`; confirm the diff contains only the current task.
