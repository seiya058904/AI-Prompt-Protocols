# AI Prompt Protocols

*Practical prompts, agent rules, and workflow protocols refined through real-world AI-assisted development.*

[English](README.md) | [简体中文](README.zh-CN.md)

---

A small, high-signal collection of plain-text Markdown documents for AI-assisted development. Each document is a complete, self-contained prompt — copy it, put it into your agent's context, and use it.

The Chinese mirror of this page lives at [README.zh-CN.md](README.zh-CN.md). The two English rule sets ship with Chinese translations under [`translations/zh-CN/agent-rules/`](translations/zh-CN/agent-rules/). The project-closeout protocol is written in Chinese natively; the UI workflow, code-review, and agent-init protocols are in English.

## Choose a Protocol

| Protocol | What it is | Best for | Language |
| --- | --- | --- | --- |
| [Codex Global Rules](#codex-global-rules) | Global behavioral rules for long-running Codex coding sessions | Standardizing a Codex-centric workflow | English · 中文译文 |
| [Universal Coding Agent Global Rules](#universal-coding-agent-global-rules) | The same core rules, written tool-agnostic | Teams mixing agents (Codex, Claude Code, Cursor, …) | English · 中文译文 |
| [UI Screenshot → Implementation Spec Protocol](#ui-screenshot--implementation-spec-protocol) | Turns a screenshot / mockup into a precise, executable UI implementation spec | Before implementation: analysis in, spec out | English |
| [UI Visual Fidelity Refinement Protocol](#ui-visual-fidelity-refinement-protocol) | Converges an implemented UI to the reference screenshot | After implementation: visual convergence phase | English |
| [Project Closeout Prompt](#project-closeout-prompt) | Lands finished work safely into the default branch and syncs it | End of a milestone, release, or handoff | 中文 |
| [Expert Code Review Protocol](#expert-code-review-protocol) | High-signal code review: real, actionable, merge-relevant findings only | Pre-merge review of PRs / diffs | English |
| [Universal Agent Init](#universal-agent-init) | Initializes or improves a repo's `AGENTS.md` / `CLAUDE.md` from repository evidence | Bootstrapping or reconciling agent instructions | English |

## Prompt Library

### Codex Global Rules

**What it does.** A reusable set of global behavioral rules for long-running Codex coding sessions: how to ask questions, how much to change, what is safe to delete, what counts as done, and how to report.

**Why it works.**

- **Reversible assumptions over questions.** Ask for clarification only when missing information could cause data loss, an irreversible decision, or a materially different result; otherwise pick the safest reversible assumption and continue.
- **Smallest safe change.** Reuse existing code and patterns before adding abstractions, dependencies, or infrastructure; no unrequested features or unrelated refactors.
- **Deletion blacklist.** `del /s`, `rd /s`, `rmdir /s`, `Remove-Item -Recurse`, `rm -rf`, and `git clean -fd/-fdx` are never used; only explicitly authorized, individually verified paths may be deleted.
- **Verify before claiming success.** Define success criteria before non-trivial changes, inspect the final diff, and run only relevant checks.
- **Tool batching.** In Code Mode, independent functions.exec-available calls are batched into one call with `Promise.allSettled` / `Promise.all` semantics.

**Best for.** Long-running Codex sessions and repos that want one consistent behavior baseline.

**Works with.** Codex (designed for its Code Mode tool calling); the ideas transfer to similar tool-calling agents.

**Read.** [codex-global-rules.md](agent-rules/codex-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md)

<details>
<summary>Preview</summary>

```text
* Ask for clarification only when missing information could materially change the result, cause data loss, or make an irreversible decision. Otherwise choose the safest reversible assumption, state it when relevant, and continue.
* Never perform broad or uncertain-scope deletion. Do not use `del /s`, `rd /s`, `rmdir /s`, `Remove-Item -Recurse`, `rm -rf`, or `git clean -fd/-fdx`. Delete only explicitly authorized, individually verified paths.
* Run only checks relevant to the change. Inspect the final diff and verify results before claiming success.
```

</details>

### Universal Coding Agent Global Rules

**What it does.** The same core behavioral rules, written without Codex-specific tooling so they apply across coding agents: Codex, Claude Code, Cursor, and similar.

**Why it works.**

- **Same 12 core rules.** The behavioral contract — reversible assumptions, minimal change, deletion safety, verification — is identical to the Codex set.
- **One honest difference.** The only substantive difference is the Tool Efficiency section: generic "run independent inspections concurrently when supported" instead of Codex's functions.exec specifics.
- **Pick per environment.** Use the Codex version for Codex sessions and the universal version for other agents, instead of maintaining two divergent philosophies.
- **Small and syncable.** Two short rule files, not two prompt empires; easy to review and keep in sync.

**Best for.** Individuals or teams who switch between agents, or want one rule set that any coding agent can load.

**Works with.** Codex, Claude Code, Cursor, and similar coding agents.

**Read.** [universal-coding-agent-global-rules.md](agent-rules/universal-coding-agent-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md)

<details>
<summary>Preview</summary>

```text
* Ask for clarification only when missing information could materially change the result, cause data loss, or make an irreversible decision. Otherwise choose the safest reversible assumption, state it when relevant, and continue.
* Never perform broad or uncertain-scope deletion. Do not use `del /s`, `rd /s`, `rmdir /s`, `Remove-Item -Recurse`, `rm -rf`, or `git clean -fd/-fdx`. Delete only explicitly authorized, individually verified paths.

Within each bounded stage, run independent inspections or tool calls concurrently when supported. Batch work that does not depend on intermediate results, and inspect all returned results.
```

</details>

### UI Screenshot → Implementation Spec Protocol

**What it does.** v2.0: converts one or more reference screenshots, mockups, or design images into a concise, structured, implementation-ready UI Implementation Specification — the smallest accurate visual model that explains the screenshot well enough to reproduce it. The agent's job is inspect → model → specify, never implement.

**Why it works.**

- **Source-of-truth hierarchy.** Evidence is ranked: explicit user requirements → screenshots → cross-screenshot consistency → verified project assets / tokens → spec estimates → inference; when lower-priority evidence conflicts with higher-priority evidence, the higher priority wins.
- **Four evidence labels.** Every non-trivial statement is OBSERVED, ESTIMATED (with a structured `Estimated: ~240px / Plausible range: 232–248px / Confidence: high` format), INFERRED, or UNKNOWN — do not disguise uncertainty as precision.
- **Anti-hallucination rules.** An explicit do-not-invent list (text, menus, logos, fonts, animations, responsive layouts, tokens), and recognizable products never override the supplied screenshots.
- **Global before local.** An 11-step analysis order: source/canvas → composition → major regions → layout relationships → design system → component patterns → exact content → assets → interaction clues → responsive evidence → implementation-critical constraints.
- **Relationships over coordinates.** Layout is described by container relationships, ratios, and spacing rhythm, with a preference for a small spacing system over dozens of isolated measurements.
- **Priority and handoff.** Requirements are classed CRITICAL / IMPORTANT / COSMETIC, and the output ends with an Uncertainty Register and Implementation Directives so implementers never polish cosmetics while critical mismatches remain.

**Best for.** Screenshot reconstruction, design-to-code handoff, and any task where the implementer should not re-guess the visual design.

**Works with.** Output is consumed by Codex, Claude Code, Cursor, Gemini CLI, or similar coding agents.

**Read.** [ui-screenshot-to-implementation-spec.md](ui-workflows/ui-screenshot-to-implementation-spec.md)（英文原文）

<details>
<summary>Preview</summary>

````text
### OBSERVED
Directly visible or verifiable from the supplied material.

### ESTIMATED
Visually measurable only approximately.

Format important estimates as:

```text
Estimated: ~240px
Plausible range: 232–248px
Confidence: high
```

### INFERRED
Not directly shown, but a reasonable conclusion from the visible structure or existing verified project context.

Format:

```text
Inferred: sidebar likely collapses at narrow widths
Confidence: medium
```

### UNKNOWN
Not supported strongly enough to estimate or infer safely.

Do not disguise uncertainty as precision.
````

</details>

### UI Visual Fidelity Refinement Protocol

**What it does.** v2.0: repeatedly compares the actual browser render with reference screenshots, identifies the highest-impact differences, corrects their root causes, and stops only when remaining differences are low-impact or not reasonably reducible.

**Why it works.**

- **Render is the evidence.** The protocol's core rule is "The implementation is a hypothesis. The browser render is evidence" — correctness is judged from the rendered page, never from CSS values or DOM structure in isolation.
- **Deterministic environment, baseline first.** Viewport, zoom, DPR, route, scroll, state, and font/asset readiness are pinned, and a baseline render with a ranked difference inventory is captured before any change — do not begin by randomly editing CSS.
- **Global before local tiers.** Comparison runs Tier 1 Structure → Tier 2 Major Geometry → Tier 3 Typography → Tier 4 Color / Surface → Tier 5 Component Detail → Tier 6 Micro Polish; micro polish is off-limits while higher tiers are wrong.
- **Visual severity and acceptance gate.** Differences are graded V0–V3 (visual-specific, not bug priority); each change cluster is classified IMPROVED / NEUTRAL / REGRESSED and rejected if it regresses a higher-priority region.
- **Root cause first.** Shared causes (container width, tokens, inherited styles, breakpoints) are inspected before patching symptoms; one coherent hypothesis per change cluster, no compensating hacks.
- **Explicit stop conditions.** The loop stops only when V0 = 0 and V1 = 0, after a fresh final comparison and a documented Remaining Difference Register.

**Best for.** The visual convergence phase after a UI has been implemented from a spec.

**Works with.** UI implementation agents: Codex, Claude Code, Cursor, and similar.

**Read.** [ui-visual-fidelity-refinement.md](ui-workflows/ui-visual-fidelity-refinement.md)（英文原文）

<details>
<summary>Preview</summary>

```text
The implementation is a hypothesis.
The browser render is evidence.

CAPTURE / OBSERVE
↓
COMPARE
↓
RANK DIFFERENCES
↓
IDENTIFY ROOT CAUSE
↓
PATCH ONE COHERENT CLUSTER
↓
RENDER AGAIN
↓
ACCEPT / ADJUST / REVERT
↓
REPEAT
```

</details>

### Project Closeout Prompt

**What it does.** A Git closeout / handoff protocol: when the user says "wrap up", it safely lands this round's finished work into the repository's default branch — committed, merged, pushed, synced, and clean — no matter where the work happened (main, a feature branch, a worktree, detached HEAD, or un-pushed local commits).

**Why it works.**

- **End state, not action list.** "Done" is defined as: work fully committed, in the default branch, pushed, local and remote default branches pointing at the same result, and a clean workspace — not merely "I committed and pushed".
- **Intermediate states are explicitly not done.** Pushing a feature branch to the remote is not closeout; opening a PR is not closeout.
- **Escalation rules.** Low-risk steps run autonomously; genuinely risky or irreversible steps — `git reset --hard`, force push — pause and ask the user first.
- **Worktree / detached HEAD awareness.** The protocol accounts for work stranded in other worktrees or on detached HEAD and forbids leaving this round's results there.
- **Verifiable final state.** The concrete completion check is `git rev-list --left-right --count HEAD...origin/<default>` returning `0 0`.

**Best for.** The end of a milestone, a release, or a handoff — when "done" must actually mean done.

**Works with.** Coding agents that can run Git commands (Codex, Claude Code, Cursor, and similar).

**Read.** [project-closeout.md](project-workflows/project-closeout.md)（中文原文）

<details>
<summary>Preview</summary>

```text
“收工”不是简单提交当前文件，也不是只把当前分支 push 到远端。

真正的目标是：

> **把本轮已经完成的成果从它实际所在的位置安全收敛到仓库的最终正式状态。**

**仅仅把 feature branch / worktree branch push 到远端，不算收工。**

**仅仅创建 PR 但尚未合并，也不算完全收工。**
```

</details>

### Expert Code Review Protocol

**What it does.** A high-signal code review protocol: reviews code as if deciding whether it is safe and appropriate to merge into production, prioritizing real, actionable issues over comment volume.

**Why it works.**

- **Six qualification criteria.** An issue is reported only when it is supported by evidence, discrete and actionable, materially relevant, specific enough to fix, not a subjective preference, and not a duplicate — prefer no finding over a speculative or low-confidence finding.
- **Confidence with a threshold.** Every finding carries a 0.0–1.0 confidence score; the reviewer must try to disprove each finding and normally reports only findings at >= 0.80, with a `Needs Verification` bucket for severe-but-unproven concerns.
- **Risk-first review order.** Review runs from the highest-level risk down: intent and design → correctness → security → performance → maintainability → language/framework → tests → documentation.
- **Severity system.** P0 (immediate blocker) through P3 (non-blocking improvement), each with concrete examples.
- **Explicit low-value list.** Trivial formatting, style preferences, generic "add more tests" comments, and theoretical micro-optimizations are excluded unless explicitly requested.
- **Honest verdict.** The review ends with exactly one of APPROVE / CHANGES REQUESTED / NEEDS CONTEXT — and never claims to have run tests, builds, or linters that were not actually run.

**Best for.** Pre-merge review of PRs and diffs, security-sensitive reviews, and teams tired of low-signal review noise.

**Works with.** Coding agents asked to review code (Codex, Claude Code, Cursor, and similar); also usable in chat or agent review workflows.

**Read.** [expert-code-review.md](review-workflows/expert-code-review.md)（英文原文）

<details>
<summary>Preview</summary>

```text
Report an issue only when it is:

1. supported by the available code or context
2. discrete and actionable
3. materially relevant
4. specific enough for the author to understand and fix
5. not merely a subjective preference
6. not a duplicate of another finding

Prefer no finding over a speculative or low-confidence finding.

Normally report only findings with confidence >= 0.80.

A potentially severe issue with insufficient evidence may instead be placed under `Needs Verification`, clearly explaining what missing evidence would confirm or dismiss it.

Never present speculation as a confirmed bug.
```

</details>

### Universal Agent Init

**What it does.** Initializes or improves a repository's persistent AI-agent instructions: a lean, accurate root `AGENTS.md` for shared guidance, plus a root `CLAUDE.md` as the Claude Code entry point — derived from repository evidence and verified before committing.

**Why it works.**

- **Evidence over invention.** Every added path, command, and convention must be verified against the repository; inventing commands, policies, or architecture is forbidden, and personal / machine-local / global instruction files are never copied into committed files.
- **Lean by design.** Targets roughly 200–400 words for a simple repository: persistent instructions consume model context, and generic advice ("write clean code") is excluded unless it has concrete repository-specific meaning.
- **One source of truth.** `CLAUDE.md` normally contains exactly `@AGENTS.md`; shared guidance lives in one file and is not duplicated.
- **Conservative handling.** Existing instruction files are never blindly overwritten; smallest effective diff; `AGENTS.override.md` precedence is recognized and reported.
- **Verification and safe commit.** Re-reads the finals, verifies every path and command, checks the diff (`git diff --check`), scans for secrets and private paths, and commits only task-owned files with explicit pathspecs — never pushes or opens PRs unless explicitly asked.

**Best for.** Bootstrapping agent instructions in a new repository, or reconciling existing `AGENTS.md` / `CLAUDE.md` files without losing user work.

**Works with.** Coding agents with repository access: Codex (via `AGENTS.md`), Claude Code (via `CLAUDE.md`), Cursor, and similar.

**Read.** [universal-agent-init.md](init-workflows/universal-agent-init.md)（英文原文）

<details>
<summary>Preview</summary>

````text
If no root `CLAUDE.md` exists, normally create exactly:

```md
@AGENTS.md
```

Persistent instructions consume model context. Do not add generic advice such as “write clean code,” “use best practices,” “test thoroughly,” or “avoid bugs” unless it has concrete repository-specific meaning.
````

</details>

## Recommended Workflow

The repository is organized around one pipeline: rules first, project context second, then a task protocol, then verification, then closeout.

```text
Global Rules
     ↓
Project Context
     ↓
Task Protocol
     ↓
Verification
     ↓
Closeout
```

Typical scenarios:

- **Coding session** → load [Global Rules](#codex-global-rules) as persistent instructions, then work directly.
- **Screenshot reconstruction** → run the [Spec Protocol](#ui-screenshot--implementation-spec-protocol) on the screenshot → implement from the spec → run [Visual Fidelity Refinement](#ui-visual-fidelity-refinement-protocol) to converge the render.
- **Finished project** → run [Project Closeout](#project-closeout-prompt) to land and sync the work.
- **Before merge** → run [Expert Code Review](#expert-code-review-protocol) on the PR or diff before merging.
- **New repository** → run [Universal Agent Init](#universal-agent-init) to set up `AGENTS.md` / `CLAUDE.md` before long-running sessions.

## Philosophy

Many prompts look impressive on paper and fall apart in real work. This repository only keeps documents that have been used in real projects and refined over time: explicit behavioral constraints, verified against real renders and real Git histories, designed to reduce agent drift and unsolicited improvisation.

> tested workflows > clever wording

This repository makes no benchmark claims and promises no guaranteed outcomes. The only evidence offered is that these documents have been used in real work and refined over time — try them, and keep what survives your own projects.

It is a small, high-signal, plain-text knowledge repository — not a prompt dump, a marketplace, a tutorial site, or a SaaS product.

## Usage

The documents are plain-text prompts. Three ways to use them:

1. **Copy as context.** Paste the full document into a chat or agent session as initial instructions.
2. **Persistent instructions.** Where the product supports a global-rules file natively, put the rules there — Codex reads `AGENTS.md`, Claude Code reads `CLAUDE.md`. For other tools, check your tool's documentation; treat these documents as context you provide unless a native mechanism is documented.
3. **Phase-specific protocol.** The Spec, Fidelity, and Closeout documents are phase prompts: run them at the right moment (before implementation, during visual convergence, at project end), not as standing rules.

The rule documents are written to fit a "global rules" slot, but whether a tool reads such a file natively is a product feature — check your tool's documentation before relying on it.

## Repository Structure

```text
.
├── README.md
├── README.zh-CN.md
├── LICENSE
├── .gitignore
├── agent-rules/
│   ├── codex-global-rules.md
│   └── universal-coding-agent-global-rules.md
├── ui-workflows/
│   ├── ui-screenshot-to-implementation-spec.md
│   └── ui-visual-fidelity-refinement.md
├── project-workflows/
│   └── project-closeout.md
├── review-workflows/
│   └── expert-code-review.md
├── init-workflows/
│   └── universal-agent-init.md
└── translations/
    └── zh-CN/
        └── agent-rules/
            ├── codex-global-rules.md
            └── universal-coding-agent-global-rules.md
```

## Contributing

Everything in this repository has been used in real work and refined over time.

Welcome:

- bug reports and ambiguity reports
- concrete improvements to existing prompts
- translation fixes for the Chinese documents
- real-world usage feedback

Please do not submit bulk-generated or unverified prompt lists. If a prompt survived real projects, describe where and how it was used.

## License

[MIT](LICENSE)
