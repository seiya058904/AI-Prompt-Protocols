# AI Prompt Protocols

*Practical prompts, agent rules, and workflow protocols refined through real-world AI-assisted development.*

[English](README.md) | [简体中文](README.zh-CN.md)

---

A small, high-signal collection of plain-text Markdown documents for AI-assisted development. Each document is a complete, self-contained prompt — copy it, put it into your agent's context, and use it.

The Chinese mirror of this page lives at [README.zh-CN.md](README.zh-CN.md). The two English rule sets ship with Chinese translations under [`translations/zh-CN/agent-rules/`](translations/zh-CN/agent-rules/); the three workflow protocols are written in Chinese natively.

## Choose a Protocol

| Protocol | What it is | Best for | Language |
| --- | --- | --- | --- |
| [Codex Global Rules](#codex-global-rules) | Global behavioral rules for long-running Codex coding sessions | Standardizing a Codex-centric workflow | English · 中文译文 |
| [Universal Coding Agent Global Rules](#universal-coding-agent-global-rules) | The same core rules, written tool-agnostic | Teams mixing agents (Codex, Claude Code, Cursor, …) | English · 中文译文 |
| [UI Screenshot → Implementation Spec Protocol](#ui-screenshot--implementation-spec-protocol) | Turns a screenshot / mockup into a precise, executable UI implementation spec | Before implementation: analysis in, spec out | 中文 |
| [UI Visual Fidelity Refinement Protocol](#ui-visual-fidelity-refinement-protocol) | Converges an implemented UI to the reference screenshot | After implementation: visual convergence phase | 中文 |
| [Project Closeout Prompt](#project-closeout-prompt) | Lands finished work safely into the default branch and syncs it | End of a milestone, release, or handoff | 中文 |

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

**What it does.** Turns a UI screenshot, design mockup, or page capture into a structured, executable UI implementation spec that another coding agent can build from — without re-seeing the screenshot.

**Why it works.**

- **Three certainty levels.** Every spec item is labeled OBSERVED (directly visible), ESTIMATED (approximate, marked with `~`), or INFERRED (reasonable guess), so the reader always knows what is fact and what is assumption.
- **Anti-hallucination rules.** If something cannot be determined, the spec must say `Unknown` or `Inferred — confidence: low` — never fill it in from memory or from knowledge of other products.
- **Global before local.** The analysis is ordered: canvas and viewport → spatial map → section hierarchy → layout architecture → design tokens → typography → components — so nothing is missed and hierarchy is explicit.
- **Relationships over coordinates.** Elements are described by their relationship to containers and neighbors, not by pretending pixels are precise.
- **Explicit handoff outputs.** The required output format ends with an Uncertainty Register and Implementation Directives, so the implementing agent knows what is uncertain and what must be built.

**Best for.** Screenshot reconstruction, design-to-code handoff, and any task where the implementer should not re-guess the visual design.

**Works with.** Output is consumed by Codex, Claude Code, Cursor, Gemini CLI, or similar coding agents.

**Read.** [ui-screenshot-to-implementation-spec.md](ui-workflows/ui-screenshot-to-implementation-spec.md)（中文原文）

<details>
<summary>Preview</summary>

```text
必须明确区分以下三类信息：

### OBSERVED

可以直接从截图确认的事实。

### ESTIMATED

可以从截图合理估计，但无法直接获得精确值。

使用 `~` 表示估值。

例如：

`~24px`

而不是伪装成：

`24px`

### INFERRED

截图没有直接展示，但根据 UI 结构进行的合理推断。

所有 INFERRED 内容必须明确标记。

绝不能把推测写成截图事实。
```

</details>

### UI Visual Fidelity Refinement Protocol

**What it does.** After a page is implemented, runs a render → screenshot → compare → fix loop against the reference screenshot until the remaining visual differences are small enough that further changes stop paying off.

**Why it works.**

- **Render is the evidence.** The protocol forbids judging by code ("CSS is set to 24px, so the spacing must be right") — every judgment is based on a real browser render: **Implementation is hypothesis. Rendered screenshot is evidence.**
- **Pinned comparison environment.** Fonts, loading, and rendering conditions are pinned (for example, wait for fonts before taking screenshots) so comparisons are apples-to-apples.
- **Classified differences.** Every found difference is labeled STRUCTURE / GEOMETRY / TYPOGRAPHY / COLOR / SURFACE / ASSET before any code changes.
- **Preserve existing functionality.** This is a visual-refinement task, not an architecture rewrite: rewriting, large refactors, and business/API changes are explicitly forbidden.
- **Proven convergence.** The loop stops only when a real difference list shows convergence, backed by actual screenshots.

**Best for.** The visual convergence phase after a UI has been implemented from a spec.

**Works with.** UI implementation agents: Codex, Claude Code, Cursor, and similar.

**Read.** [ui-visual-fidelity-refinement.md](ui-workflows/ui-visual-fidelity-refinement.md)（中文原文）

<details>
<summary>Preview</summary>

```text
任何视觉判断都必须以真实 Browser Render 为依据。

禁止：

> “CSS 已经设置为 24px，所以间距应该正确。”

必须：

> 截图确认真实渲染中的视觉间距是否与参考图一致。

**Implementation is hypothesis.  
Rendered screenshot is evidence.**
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
