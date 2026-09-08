# AI Prompt Protocols

*Practical prompts, agent rules, and workflow protocols refined through real-world AI-assisted development.*

[English](README.md) | [简体中文](README.zh-CN.md)

---

A small, high-signal collection of plain-text Markdown documents for AI-assisted development. Each document is a complete, self-contained prompt — copy it, put it into your agent's context, and use it.

The Chinese mirror of this page lives at [README.zh-CN.md](README.zh-CN.md). The two English rule sets ship with Chinese translations under [`translations/zh-CN/agent-rules/`](translations/zh-CN/agent-rules/). The project-closeout protocol is written in Chinese natively; the UI workflow, code-review, and agent-init protocols are in English. Prompt lifecycle and maintenance rules live in [MAINTENANCE.md](MAINTENANCE.md).

## Choose a Protocol

| Protocol | What it is | Best for | Language |
| --- | --- | --- | --- |
| [Codex Global Rules](#codex-global-rules) | Global behavioral rules for reliable, low-noise Codex coding sessions | Standardizing a Codex-centric workflow | English · 中文译文 |
| [Universal Coding Agent Global Rules](#universal-coding-agent-global-rules) | The same core rules, written tool-agnostic | Teams mixing agents (Codex, Claude Code, Cursor, …) | English · 中文译文 |
| [UI Screenshot → Implementation Spec Protocol](#ui-screenshot--implementation-spec-protocol) | Turns a screenshot / mockup into a precise, executable UI implementation spec | Before implementation: analysis in, spec out | English |
| [UI Visual Fidelity Refinement Protocol](#ui-visual-fidelity-refinement-protocol) | Converges an implemented UI to the reference screenshot | After implementation: visual convergence phase | English |
| [Project Closeout Prompt](#project-closeout-prompt) | Lands finished work safely into the default branch and syncs it | End of a milestone, release, or handoff | 中文 |
| [Expert Code Review Protocol](#expert-code-review-protocol) | High-signal code review: real, actionable, merge-relevant findings only | Pre-merge review of PRs / diffs | English |
| [Universal Agent Init](#universal-agent-init) | Initializes or improves a repo's `AGENTS.md` / `CLAUDE.md` from repository evidence | Bootstrapping or reconciling agent instructions | English |

## Prompt Library

### Codex Global Rules

**What it does.** A reusable set of global behavioral rules for reliable, low-noise Codex coding sessions: how to ask questions, how much to change, what is safe to delete, what counts as done, and how to report.

**Why it works.**

- **Reversible assumptions over questions.** Ask for clarification only when missing information could cause data loss, an irreversible outcome, a materially different result, or require a genuine user choice; otherwise pick the safest reversible assumption and continue.
- **Smallest safe change.** Reuse existing code, architecture, and conventions before adding abstractions, dependencies, or infrastructure; no unrequested features, future-proofing, or unrelated refactors.
- **Deletion blacklist.** Destructive blanket cleanup commands such as `del /s`, `rd /s`, `rmdir /s`, `Remove-Item -Recurse`, `rm -rf`, and `git clean -fd/-fdx` are never used; only explicitly identified and verified paths may be deleted.
- **External side-effect gate.** Push, merge, deploy, publish, release, dependency updates, and remote configuration changes require explicit authorization or a deliberately invoked workflow whose purpose includes that action.
- **Verify before claiming success.** Establish success criteria before non-trivial changes, inspect the final diff, run only relevant checks, and never claim verification that did not actually run.
- **Tool batching.** When the environment supports it, independent read-only inspections are batched within a bounded stage; dependent, conflicting, approving, or destructive steps stay sequential.

**Best for.** Long-running Codex sessions and repos that want one consistent behavior baseline.

**Works with.** Codex (designed for its Code Mode tool calling); the ideas transfer to similar tool-calling agents.

**Read.** [codex-global-rules.md](agent-rules/codex-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md)

<details>
<summary>Preview</summary>

```text
* Ask for clarification only when missing information could materially change the result, cause data loss, create an irreversible outcome, or require a genuine user choice. Otherwise choose the safest reversible assumption, state it when relevant, and continue.
* Never perform broad or uncertain-scope deletion. Do not use destructive blanket cleanup commands such as `rm -rf`, `Remove-Item -Recurse`, `git clean -fd/-fdx`, `del /s`, `rd /s`, or `rmdir /s`. Delete only explicitly identified paths whose purpose and safety have been verified.
* Run only checks relevant to the change. Do not run broad or expensive validation merely for appearance of thoroughness.
```

</details>

### Universal Coding Agent Global Rules

**What it does.** The same core behavioral rules, written without Codex-specific tooling so they apply across coding agents: Codex, Claude Code, Cursor, Gemini CLI, and similar.

**Why it works.**

- **Same core rules.** The behavioral contract — reversible assumptions, minimal change, deletion safety, external side-effect gate, verification — is identical to the Codex set.
- **One honest difference.** The only substantive difference is the Tool Efficiency section: tool-agnostic "batch independent inspections when supported" wording instead of Codex-specific phrasing.
- **Optional Personal Collection.** A generic, opt-in hook for a user-curated collection of prompts and references; it is never treated as project memory or scanned by default, and the path is a placeholder rather than a machine-specific value.
- **Pick per environment.** Use the Codex version for Codex sessions and the universal version for other agents, instead of maintaining two divergent philosophies.
- **Small and syncable.** Two short rule files, not two prompt empires; easy to review and keep in sync.

**Best for.** Individuals or teams who switch between agents, or want one rule set that any coding agent can load.

**Works with.** Codex, Claude Code, Cursor, Gemini CLI, and similar coding agents.

**Read.** [universal-coding-agent-global-rules.md](agent-rules/universal-coding-agent-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md)

<details>
<summary>Preview</summary>

```text
* Ask for clarification only when missing information could materially change the result, cause data loss, create an irreversible outcome, or require a genuine user choice. Otherwise choose the safest reversible assumption, state it when relevant, and continue.
* Never perform broad or uncertain-scope deletion. Avoid destructive blanket cleanup commands and delete only explicitly identified paths whose purpose and safety have been verified.

When the environment supports parallel execution, batch independent inspections or other non-conflicting operations within the same bounded stage.
```

</details>

### UI Screenshot → Implementation Spec Protocol

**What it does.** Converts one or more reference screenshots, mockups, or design images into a concise, structured, implementation-ready UI Implementation Specification — the smallest accurate visual model that explains the screenshot well enough to reproduce it. The agent's job is inspect → model → specify, never implement.

**Why it works.**

- **Source-of-truth hierarchy.** Evidence is ranked: explicit user requirements → screenshots → cross-screenshot consistency → verified project assets / tokens → reasonable measurement or estimate → inference; the screenshot is the visual ground truth unless the user says otherwise.
- **Three evidence labels.** Information that is not directly established is marked ESTIMATED (with a structured `Estimated width: ~240px / Confidence: high` format), INFERRED, or UNKNOWN — ordinary observations are not repeatedly labeled, and uncertainty is never disguised as precision.
- **Anti-hallucination rules.** An explicit do-not-invent list (text, menus, fonts, colors, responsive states, features, branding, tokens), and recognizable products never override the supplied screenshots.
- **Global before local.** An 11-step analysis order: canvas and viewport → global composition → major regions → layout relationships → typography / design system → reusable components → visible content → assets → visible states → responsive evidence → implementation-critical constraints.
- **Relationships over coordinates.** Layout is described by container relationships, ratios, and spacing rhythm, with a preference for a small spacing system over dozens of isolated measurements.
- **Fidelity priorities and handoff.** Requirements are classed CRITICAL / IMPORTANT / COSMETIC, and the output ends with Material Uncertainty and Implementation Directives so implementers never polish cosmetics while critical mismatches remain.

**Best for.** Screenshot reconstruction, design-to-code handoff, and any task where the implementer should not re-guess the visual design.

**Works with.** Output is consumed by Codex, Claude Code, Cursor, Gemini CLI, or similar coding agents.

**Read.** [ui-screenshot-to-implementation-spec.md](ui-workflows/ui-screenshot-to-implementation-spec.md)（英文原文）

<details>
<summary>Preview</summary>

````text
### ESTIMATED
Use when a value can only be approximated.

Example:

```text
Estimated width: ~240px
Confidence: high
```

### INFERRED
Use for behavior or structure that is not directly visible but is reasonably supported.

### UNKNOWN
Use when the available evidence is insufficient.

Do not disguise uncertainty as precision.
````

</details>

### UI Visual Fidelity Refinement Protocol

**What it does.** Compares the actual rendered interface against the supplied reference, identifies meaningful differences, fixes their root causes, and verifies convergence without breaking functionality or maintainability.

**Why it works.**

- **Render is the evidence.** The protocol's core rule is "The implementation is a hypothesis. The rendered page is evidence" — correctness is judged from the rendered page, never from CSS values or DOM structure in isolation.
- **Honest tool-availability gate.** Before starting, the agent determines whether real rendering and comparison are actually available: Full Visual Mode runs the complete iterative loop; Limited Verification Mode makes only evidence-supported code-level changes and must report `VISUAL VERIFICATION REQUIRED`.
- **Stable environment, baseline first.** Reference dimensions, viewport, zoom, DPR, route, scroll, state, and font/asset readiness are pinned, and a baseline render with a ranked difference inventory is captured before any change — do not begin by randomly editing CSS.
- **Global before local tiers.** Comparison runs V0 Structure → V1 Major Visual Impact → V2 Noticeable → V3 Cosmetic; micro polish is off-limits while higher tiers are wrong.
- **Root cause first.** Shared causes (container width, layout model, tokens, inherited styles, breakpoints) are inspected before patching symptoms; one coherent hypothesis per change cluster, no compensating hacks.
- **Acceptance gate and stop conditions.** Each change cluster is classified IMPROVED / NEUTRAL / REGRESSED and rejected if it regresses a higher-priority region; in Full Visual Mode the loop stops only when V0 = 0 and V1 = 0, after a fresh final comparison.

**Best for.** The visual convergence phase after a UI has been implemented from a spec.

**Works with.** UI implementation agents: Codex, Claude Code, Cursor, and similar.

**Read.** [ui-visual-fidelity-refinement.md](ui-workflows/ui-visual-fidelity-refinement.md)（英文原文）

<details>
<summary>Preview</summary>

```text
V0 — Structure
V1 — Major Visual Impact
V2 — Noticeable
V3 — Cosmetic

CAPTURE / OBSERVE
↓
COMPARE
↓
RANK
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

**What it does.** A Git closeout / handoff protocol: when the user explicitly says "wrap up", it safely lands this round's finished work into the state allowed by the repository's own integration workflow — committed, pushed, synced, and clean under the repo's actual model (direct-to-default, feature-branch integration, or PR-based), no matter where the work happened.

**Why it works.**

- **Integration model first.** The protocol identifies how the repository actually integrates work — direct-to-default, feature branch + integration, or protected / PR-based — instead of forcing a `main` + direct-push model onto every repo.
- **End state, not action list.** "Done" is defined by the repo's integration model: for direct-integration repos, work fully committed, pushed, local and remote defaults pointing at the same result, and a clean workspace; for PR-based repos, work pushed and at the point a PR is possible, with any remaining approval step reported as `Remaining integration step`.
- **Authorization boundary.** Invoking the protocol authorizes normal, low-risk, task-owned Git wrap-up (status/diff checks, fetch, commit, push, safe local integration); force push, history rewrites, discarding user changes, and any external side effects require additional explicit authorization.
- **Escalation rules.** Divergence, merge/rebase/reset/force choices, and business-semantic conflicts pause and ask the user first; ordinary mechanical conflicts may be resolved safely.
- **Multi-repository support.** Each repository is checked independently; repos not touched by the round are only read-only verified, never given meaningless commits.
- **Verifiable final state.** Completion is verified per repo with `git status`, `git rev-list --left-right --count HEAD...<upstream>`, and a final diff inspection.

**Best for.** The end of a milestone, a release, or a handoff — when "done" must actually mean done under the repo's own workflow.

**Works with.** Coding agents that can run Git commands (Codex, Claude Code, Cursor, and similar).

**Read.** [project-closeout.md](project-workflows/project-closeout.md)（中文原文）

<details>
<summary>Preview</summary>

```text
收工的判断标准不是“执行过 commit / push”。

真正目标是：

> **本轮需要保留的成果已经被安全保存，并达到当前仓库正式工作流允许的最终状态，没有遗留本轮未处理工作。**

仓库的正式 integration model 可能不同。

先识别，再执行。
```

</details>

### Expert Code Review Protocol

**What it does.** A high-signal code review protocol: reviews code as if deciding whether it is safe and appropriate to merge into production, prioritizing real, actionable issues over comment volume while minimizing speculation.

**Why it works.**

- **Seven qualification criteria.** An issue is reported only when it is evidence-supported, discrete, actionable, materially relevant, specific enough to fix, not merely subjective, and not a duplicate — prefer no finding over a speculative finding.
- **Confidence with a threshold.** Every finding carries a 0.0–1.0 confidence score; the reviewer must actively try to disprove each finding and normally reports only confirmed findings at >= 0.80, with a `Needs Verification` bucket for severe-but-unproven concerns.
- **Risk-first review order.** Review runs from the highest-level risk down: intent/design → correctness/reliability → security → performance/resources → maintainability → tests/contracts.
- **Targeted verification.** When repository and tool access are available, small relevant low-risk checks (focused test, targeted reproduction, narrow typecheck/lint, read-only call-site inspection) may raise or lower confidence in a finding — never broad suites for appearance of thoroughness.
- **Severity system.** P0 (immediate blocker) through P3 (non-blocking improvement), each with concrete examples.
- **Explicit low-value list.** Trivial formatting, subjective style, generic "add tests" comments, and theoretical risks are excluded unless explicitly requested.
- **Honest verdict.** The review ends with exactly one of APPROVE / CHANGES REQUESTED / NEEDS CONTEXT — and never claims to have run tests, builds, or linters that were not actually run.

**Best for.** Pre-merge review of PRs and diffs, security-sensitive reviews, and teams tired of low-signal review noise.

**Works with.** Coding agents asked to review code (Codex, Claude Code, Cursor, and similar); also usable in chat or agent review workflows.

**Read.** [expert-code-review.md](review-workflows/expert-code-review.md)（英文原文）

<details>
<summary>Preview</summary>

```text
Report an issue only when it is:

1. evidence-supported
2. discrete
3. actionable
4. materially relevant
5. specific enough to fix
6. not merely subjective
7. not a duplicate

Prefer no finding over a speculative finding.

Normally report confirmed findings only when confidence is at least 0.80.

A potentially serious concern with insufficient evidence belongs under `Needs Verification`, not as a confirmed bug.

Never present speculation as fact.
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
├── MAINTENANCE.md
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
- prompt lifecycle and maintenance improvements (see [MAINTENANCE.md](MAINTENANCE.md))
- real-world usage feedback

Please do not submit bulk-generated or unverified prompt lists. If a prompt survived real projects, describe where and how it was used.

## License

[MIT](LICENSE)
