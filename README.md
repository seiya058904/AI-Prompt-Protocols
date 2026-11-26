<h1 align="center">🧭 AI Prompt Protocols</h1>

<p align="center">
  <img src="assets/readme-cover.svg" width="100%" alt="AI Prompt Protocols illustrated workflow: establish rules, implement a focused task, verify evidence, close out responsibly" />
  <br>
  <sub>ORIGINAL VECTOR WORKFLOW ILLUSTRATION · SELECT THE PROTOCOL THAT FITS THE TASK</sub>
</p>

<p align="center">
  <strong>Less improvisation. More reproducible agent work.</strong>
</p>

<p align="center">
  A small, maintained field guide for AI-assisted software development.<br>
  Eleven focused protocols. Clear boundaries. Evidence before confidence.
</p>

<p align="center">
  <a href="#featured-global-agent-rules">🛡️ Global Rules Deep Dive</a>
  &nbsp;·&nbsp;
  <a href="#start-with-the-task">🗂️ Find a Protocol</a>
  &nbsp;·&nbsp;
  <a href="#putting-them-together">🔁 Workflow</a>
  &nbsp;·&nbsp;
  <a href="MAINTENANCE.md">⚙️ Maintenance</a>
</p>

<p align="center">
  <sub>GLOBAL RULES &nbsp;·&nbsp; UI FIDELITY &nbsp;·&nbsp; CODE REVIEW &amp; AUDIT &nbsp;·&nbsp; REPOSITORY SETUP &nbsp;·&nbsp; CLOSEOUT &amp; CLEANUP</sub>
</p>

---

> **Choose the smallest protocol that fits the moment.**
>
> This is not a prompt marketplace, a claim of benchmark superiority, or a universal agent framework. The documents are practical instructions refined around real engineering work—not substitutes for reading the repository, testing a change, or obtaining permission for side effects.

## Start with the task

Every document has a job. **Persistent rules establish how an agent works; task protocols guide one phase of the work.** They should not all be pasted into every session.

<table>
  <tr>
    <td width="50%" valign="top">
      <h3>01 / Establish the Rules</h3>
      <p><sub>PERSISTENT AGENT BEHAVIOR</sub></p>
      <p>Keep changes narrow, preserve existing work, and require evidence for claims.</p>
      <p><a href="agent-rules/codex-global-rules.md"><strong>Codex Global Rules →</strong></a><br><a href="agent-rules/claude-code-global-rules.md">Claude Code Global Rules →</a><br><a href="agent-rules/antigravity-global-rules.md">Antigravity Global Rules →</a><br><a href="agent-rules/universal-coding-agent-global-rules.md">Universal Coding Agent Rules →</a></p>
    </td>
    <td width="50%" valign="top">
      <h3>02 / Make the Interface Match</h3>
      <p><sub>DESIGN EVIDENCE → REAL RENDER</sub></p>
      <p>Extract a faithful UI specification, then refine the actual implementation against its reference.</p>
      <p><a href="ui-workflows/ui-screenshot-to-implementation-spec.md"><strong>Screenshot → Spec →</strong></a><br><a href="ui-workflows/ui-visual-fidelity-refinement.md">Visual Fidelity Refinement →</a></p>
    </td>
  </tr>
  <tr>
    <td width="50%" valign="top">
      <h3>03 / Establish Confidence</h3>
      <p><sub>REAL REPOSITORY · ACTIONABLE FINDINGS</sub></p>
      <p>Build project-specific instructions from verified facts; review changes or audit a whole repository without inventing defects.</p>
      <p><a href="init-workflows/universal-agent-init.md"><strong>Universal Agent Init →</strong></a><br><a href="review-workflows/unified-code-review-and-repository-audit.md">Unified Code Review &amp; Repository Audit →</a></p>
    </td>
    <td width="50%" valign="top">
      <h3>04 / Finish Without Losing Work</h3>
      <p><sub>VALIDATION · DIFF · AUTHORIZATION</sub></p>
      <p>Finish verified work, deliver confirmed repairs, or audit local cleanup candidates before recycling only user-selected items.</p>
      <p><a href="project-workflows/project-closeout.md"><strong>Project Closeout →</strong></a><br><a href="project-workflows/repository-audit-repair-closeout.md">Audit, Repair &amp; Closeout →</a><br><a href="project-workflows/local-recycle-bin-cleanup.md">Local Recycle Bin Cleanup →</a></p>
    </td>
  </tr>
</table>

| Task | Recommended starting point | When |
| --- | --- | --- |
| Predictable Codex sessions | [Codex Global Rules](agent-rules/codex-global-rules.md) | Persistent Codex instructions (`~/.codex/AGENTS.md`) |
| Predictable Claude Code sessions | [Claude Code Global Rules](agent-rules/claude-code-global-rules.md) | Persistent Claude Code instructions (`~/.claude/CLAUDE.md`) |
| Predictable Antigravity (Gemini) sessions | [Antigravity Global Rules](agent-rules/antigravity-global-rules.md) | Persistent Antigravity instructions (`~/.gemini/config/GEMINI.md`) |
| Consistent cross-agent behavior | [Universal Coding Agent Global Rules](agent-rules/universal-coding-agent-global-rules.md) | Persistent, tool-independent instructions |
| Interpret a screenshot | [UI Screenshot → Implementation Spec](ui-workflows/ui-screenshot-to-implementation-spec.md) | Before implementation |
| Match a reference image | [UI Visual Fidelity Refinement](ui-workflows/ui-visual-fidelity-refinement.md) | After a working first render |
| Finish a scoped task | [Project Closeout](project-workflows/project-closeout.md) | After verification and diff review |
| Review a change, a scope, or a whole repository | [Unified Code Review & Repository Audit](review-workflows/unified-code-review-and-repository-audit.md) | Before merging, or as an audit-only health check |
| Set up repository guidance | [Universal Agent Init](init-workflows/universal-agent-init.md) | On project adoption or instruction audit |
| Audit, repair, and deliver a repository | [Repository Audit, Repair & Closeout](project-workflows/repository-audit-repair-closeout.md) | Explicitly authorized execution and Git delivery |
| Review local cleanup candidates | [Local Recycle Bin Cleanup](project-workflows/local-recycle-bin-cleanup.md) | Read-only audit, then user-selected recycling |

---

## Featured: Global Agent Rules

If you adopt only one document from this repository, start with the **Global Agent Rules**—using **[Universal Coding Agent Global Rules](agent-rules/universal-coding-agent-global-rules.md)** as the core blueprint.

Unlike task-specific protocols that you invoke occasionally, global rules sit in your agent's persistent configuration and govern **every single turn across every repository**. The universal core is deliberately compact—**52 lines (~1,040 words)** organized into six interlocking sections (`Working Style`, `Repository Work`, `Project Closeout`, `Boundaries`, `Tool Efficiency`, and `Optional Personal Collection`)—so every line carries real operational weight without bloating your context window.

### 1. Safety & Guardrails Against Dangerous Operations

Most coding-agent disasters happen when an agent tries to be "helpful" beyond its authorization: wiping untracked files, pushing unreviewed commits, rewriting Git history, upgrading packages mid-fix, or quietly bypassing friction. `Universal Coding Agent Global Rules` addresses these failure modes with explicit, unambiguous hard stops:

- **No blanket destructive deletion:** Commands such as `rm -rf`, `Remove-Item -Recurse`, `git clean -fd/-fdx`, `del /s`, `rd /s`, and `rmdir /s` are explicitly prohibited. Deletion is restricted to individually identified, explicitly approved, safety-verified paths. Even during an authorized project closeout, approved cleanup items go to the **Recycle Bin** first; permanent deletion requires separate approval, and if safe recycling is unavailable, the agent must report that instead of deleting.
- **Hard wall on external mutations and dependency drift:** Outside an explicitly scoped closeout, pushes, merges, deployments, releases, publishing, remote configuration changes, and adding or upgrading dependencies all require explicit authorization or a user-invoked workflow—**repository conventions alone grant none**. Installing already-declared dependencies from trusted project sources remains allowed without mutating version declarations or lockfiles.
- **No history destruction or secret exposure:** Force-pushing and history rewrites require separate explicit authorization and may never be used merely to synchronize a branch. Secrets may never be exposed or committed.
- **No permission-system circumvention:** The agent is explicitly barred from modifying permission/approval settings, bypassing approval prompts, or working around a user denial (`never bypass a denial`).
- **Protection of unrelated user work and branches:** Unrelated local modifications must be preserved and excluded from edits and commits—never discarded, stashed, or bundled just to make `git status` look clean. Default, protected, long-lived, shared, and uncertain branches are protected; remote branch deletion requires separate explicit authorization.
- **Explicit opt-in for project closeout:** Routine task completion never silently triggers workspace cleanup, branch deletion, merges, or pushes. Closeout rules activate **only** on an explicit request to wrap up or finalize a project, strictly within the agreed scope.

### 2. Execution Efficiency, Tool Discipline & Token Savings

Every extra turn in a coding agent replays conversation history, burns tokens, and adds latency. These rules cut wasted rounds, bloated context, and runaway execution without sacrificing engineering rigor:

- **Minimal persistent prompt footprint:** At **52 lines** (and **30 lines** in the Gemini-oriented Antigravity variant), the rules impose a very small fixed token cost per turn compared to sprawling multi-page system prompts.
- **Context discipline (`Inspect only what the task needs`):** Stops agents from dumping entire repositories into context or scanning unrelated directories. Even the optional personal collection footer is strictly on-demand (`Never scan it routinely`).
- **Smart batching that cuts turn overhead:** Independent reads, searches, and checks are batched instead of split across unnecessary back-and-forth turns—reducing repeated input-context replay—while dependent investigations, conflicting edits, approvals, waits, and destructive actions stay strictly sequential so batching never causes race conditions or wasted work.
- **No silent multi-agent token explosions:** `Do not spawn subagents or agent teams unless explicitly requested` prevents an agent from spinning up parallel sub-conversations that multiply token spend behind your back.
- **Lean verification and anti-retry discipline:** Checks stay proportionate to scope and risk, targeting under five minutes for local tests and GitHub Actions CI through targeted tests, path filters, caching, and skipping nonessential builds/tests on docs-only changes. The agent must diagnose failures before rerunning, avoid blind retry loops or "validation for appearance," and stop optional testing once criteria and relevant risks are covered.
- **Prefer the right tool over brittle automation:** Favors CLIs, APIs, and MCP tools over slow UI automation, reserves browser verification for when UI behavior genuinely matters, and prevents common Windows encoding bugs (`Get-Content -Encoding UTF8`). The tool-specific variants (`Codex`, `Claude Code`, `Antigravity`) also encode clean browser-automation routing (`ego-browser` for logged-in/personal workflows vs. `playwright` for isolated visual/trace checks).
- **Concise, result-first communication:** Leads with the outcome or required action, skips formal plans when complexity doesn't warrant one, and offers 1–3 prioritized, non-repetitive next steps instead of filler summaries.

### 3. Instruction Following, Precedence & Anti-Hallucination Discipline

A common complaint with coding agents is that they either ignore project conventions, ask too many trivial questions, over-engineer simple fixes, or claim success without verifying. The rules establish a clear operating contract:

- **Clear instruction hierarchy:** Explicit user instructions and the `Boundaries` section always hold. Applicable repository instructions (`AGENTS.md`, `CLAUDE.md`, `.claude/rules/`, nested instruction files) override global defaults, and the agent must proactively read applicable non-autoloaded or nested files *before* modifying files.
- **Decisive execution without reckless guessing:** Completes authorized work directly using available tools. Asks only when ambiguity materially affects correctness, safety, scope, or a genuine user choice; otherwise states a safe assumption when relevant and proceeds. Authorization stays valid within its original scope and only reconfirms when scope or risk materially changes.
- **Constructive handling of blockers:** When blocked by a missing permission or external failure, the agent finishes independent safe work first, identifies the exact blocker, and asks only for the minimum approval needed.
- **Smallest safe change + durable project memory:** Grounds every edit in current repository evidence, reuses existing code, and forbids unsolicited features, abstractions, dependencies, configuration, or refactors. Durable project preferences go into versioned repository files (`AGENTS.md`, `CLAUDE.md`, skills, `docs/`) rather than private agent memory that breaks across machines or directory moves.
- **Evidence-before-confidence completion gate:** Before reporting done, the agent must inspect the final diff and repository state, then report actual changes, checks actually run, failures or skipped checks, and remaining uncertainty—explicitly forbidding claims of unperformed verification.

### Failure Mode → Rule Mapping

| Common Agent Failure Mode | Rule in `Universal Coding Agent Global Rules` | Practical Effect |
| --- | --- | --- |
| Wiping untracked files with `git clean -fdx` or `rm -rf` | Explicit ban on blanket recursive deletes; path-by-path approval + Recycle Bin default on closeout | Eliminates catastrophic one-command data loss |
| Pushing, merging, or upgrading packages unasked | Hard authorization wall on external mutations and dependency changes | Keeps Git history, remotes, and lockfiles under human control |
| Silently multiplying token spend via subagents | `Do not spawn subagents or agent teams unless explicitly requested` | Predictable token usage and single-thread clarity |
| Burning context & turns on one-by-one file reads | `Inspect only what the task needs` + batch independent operations | Fewer round-trips, smaller context replay, faster execution |
| Endless blind test reruns or slow full-suite CI on doc edits | Diagnose before rerunning; proportionate checks; lean <5-min CI; skip nonessential builds for docs-only changes | Faster feedback loops without theatrical validation |
| Overwriting or stashing the user's uncommitted work | Preserve unrelated user changes; never discard, stash, or commit them for cleanliness | Safe coexistence with in-progress local edits |
| Claiming "verified" without running checks or reading diff | Inspect final diff & repo state; report actual checks run/skipped; never claim unperformed verification | High-trust completion reports with honest caveats |
| Endless clarifying questions—or silently guessing on risky changes | Act directly; ask only on material correctness/safety/scope/choice ambiguity | Smooth autonomy on routine work, hard stops on real decisions |

### Verbatim Excerpt (`Universal Coding Agent Global Rules`)

From [agent-rules/universal-coding-agent-global-rules.md](agent-rules/universal-coding-agent-global-rules.md):

```markdown
## Boundaries

- Do not spawn subagents or agent teams unless explicitly requested.
- Outside scoped closeout, external mutations (including pushes, merges, deployments, releases, publishing, and remote configuration changes) and dependency updates require explicit authorization or a user-invoked workflow covering them. Repository conventions alone grant none.
- Installing existing dependencies from trusted project sources is allowed when needed, without changing declared versions or lockfiles. Adding or upgrading dependencies requires authorization.
- Force-pushing and history rewrites require separate explicit authorization; never use them just to synchronize. Never expose or commit secrets.
- Delete only individually identified, explicitly approved, and safety-verified paths. Never perform broad or uncertain deletion or use blanket cleanup commands such as `rm -rf`, `Remove-Item -Recurse`, `git clean -fd/-fdx`, `del /s`, `rd /s`, or `rmdir /s`.
- Respect the agent's permission and approval system; never change settings or bypass approval prompts to evade these boundaries.

## Tool Efficiency

- Inspect only what the task needs. Prefer suitable CLIs, APIs, or MCP tools over UI automation; use browser verification when UI behavior matters.
- Batch independent operations only when results cannot change one another's scope, and do not split batchable work across unnecessary turns. Keep dependent investigations, conflicting changes, approvals, waits/resumptions, and destructive or irreversible actions sequential.
- In PowerShell, prefer `Get-Content -Encoding UTF8`; check encoding if text is garbled.
```

> **Quick adaptation tip:** The universal prompt defaults to concise Simplified Chinese replies (line 5) and ends with an optional `<your-collection-repository-path>` hook. Change the reply language to your preferred language and replace or remove the optional personal-collection footer before pasting it into your agent's global rules file.

### Choosing Among the Four Global Rule Variants

All four files under `agent-rules/` enforce the same core engineering principles, adapted to each agent ecosystem:

| Variant | File | Typical Location | Footprint & Adaptation |
| --- | --- | --- | --- |
| **Universal** *(Reference Blueprint)* | [universal-coding-agent-global-rules.md](agent-rules/universal-coding-agent-global-rules.md) | Any coding agent's global prompt | 52 lines · Tool-agnostic core with completeness sweep, mechanical-check ratchet, external API/version verification, and 2-round review cap; omits specific browser-automation tools |
| **Codex** | [codex-global-rules.md](agent-rules/codex-global-rules.md) | `~/.codex/AGENTS.md` | 33 lines · Multi-repository audits and browser-automation routing |
| **Claude Code** | [claude-code-global-rules.md](agent-rules/claude-code-global-rules.md) | `~/.claude/CLAUDE.md` | 37 lines · Slimmed core + tool-efficiency habits + browser routing + Claude-specific anti-pause execution, plain-language UI copy, and on-demand `project-closeout` skill pointer |
| **Antigravity (Gemini)** | [antigravity-global-rules.md](agent-rules/antigravity-global-rules.md) | `~/.gemini/config/GEMINI.md` | 30 lines · Compact Gemini-oriented wording with anti-hallucination (`[推测]`), anti-sycophancy, external API/version verification, completeness sweep, mechanical checks, on-demand `project-closeout` skill pointer, and browser routing |

> **Honest boundary:** Well-crafted global rules substantially reduce avoidable mistakes, instruction drift, and wasted turns, but prompts are a **behavioral layer—not a security sandbox**. Keep them paired with your tool's real permission prompts, version control, and human review for irreversible actions.

---

## Protocols in context

The files linked below are the **authoritative prompts**. This README is the map—not a second, abbreviated copy to give an agent instead of the real document.

Navigation and explanations are in English. Prompts are presented in their original language; no translated copy is required.

### Codex Global Rules

**Use when:** a Codex session needs stable expectations around scope, edits, testing, browser-automation routing, and external actions. It includes first-report coverage for multi-repository audits and routes browser work between `ego-browser` and `playwright`.

**Read:** [English source](agent-rules/codex-global-rules.md)

### Claude Code Global Rules

**Use when:** Claude Code needs persistent engineering discipline plus Claude-specific guardrails—executing authorized milestones without premature pauses, keeping user-facing UI copy concise and plain-language, sweeping coupled siblings, turning repeatable defects into mechanical checks, capping reviews at 2 rounds, and routing `ego-browser` vs. `playwright`.

**Read:** [English source](agent-rules/claude-code-global-rules.md)

### Antigravity Global Rules

**Use when:** an Antigravity (Gemini) session needs compact, high-signal engineering discipline tailored to Gemini's strengths and failure modes—explicitly separating verified repo facts from `[推测]` inferences, pushing back on flawed plans, sweeping coupled edits, adding mechanical checks, guarding workflows/swarms and Antigravity settings, and routing `ego-browser` vs. `playwright`.

**Read:** [English source](agent-rules/antigravity-global-rules.md)

### Universal Coding Agent Global Rules

**Use when:** the same working principles must survive a change of coding assistant. The rules avoid assuming one tool interface or specific browser-automation tools, while retaining explicit boundaries for destructive operations, repository state, and permission to act.

**Read:** [English source](agent-rules/universal-coding-agent-global-rules.md)

### UI Screenshot → Implementation Spec Protocol

**Use before coding.** Translate supplied screenshots into a specification that another agent can implement. Separate what is visible from what is only estimated or inferred; document the layout relationships and the gaps that the reference cannot prove.

**Output:** a grounded implementation specification, **not** generated UI code.

**Read:** [Source protocol](ui-workflows/ui-screenshot-to-implementation-spec.md)

### UI Visual Fidelity Refinement Protocol

**Use after the first real render.** Compare the rendered page with its reference, prioritize meaningful mismatches, find their causes, then edit a coherent cluster and render again.

```text
Reference + current render
            ↓
       Compare the evidence
            ↓
     Fix a meaningful cause
            ↓
   Render again → keep / revise / revert
```

If a browser or screenshot comparison is unavailable, the protocol requires **an explicit verification limitation**, not a claim of visual success.

**Read:** [Source protocol](ui-workflows/ui-visual-fidelity-refinement.md)

### Project Closeout Prompt

**Use only when explicitly invoked.** This Chinese-language protocol distinguishes ordinary status checks from an authorized request to finish and integrate work. It reviews changes and validation before proceeding with permitted Git operations; destructive or production-affecting actions require their own approval.

**Read:** [Chinese canonical source](project-workflows/project-closeout.md)

### Unified Code Review & Repository Audit

**Use before accepting a change, or for an audit-only health check.** This Chinese-language protocol selects CHANGE (a PR, diff, or commit range), REPOSITORY (a whole-repository audit), or FOCUSED (specified files or modules) mode automatically. It pins a baseline, separates confirmed findings from those that still need verification, and hands off repair guidance; zero invented findings is better than a long speculative report.

**Boundary:** audit only by default. It does not repair code, create commits or PRs, push, or deploy.

**Read:** [Chinese canonical source](review-workflows/unified-code-review-and-repository-audit.md)

### Universal Agent Init

**Use when adopting or auditing a repository.** Inspect the actual source, commands, tests, and existing instructions before updating `AGENTS.md` or `CLAUDE.md`. Preserve valuable guidance and user changes instead of pasting a generic template.

**Read:** [Source protocol](init-workflows/universal-agent-init.md)

### Repository Audit, Repair & Closeout

**Use only when explicitly authorizing an agent to take over the current repository.** This execution protocol adds confirmed repairs, regression checks, commits, remote delivery, and final workspace verification to a full audit. It preserves pre-existing work and distinguishes code synchronization from production release.

**Authorization:** explicit invocation includes pushing to the verified existing remote and, where permitted and all review gates are satisfied, PR integration. Read its scope before invoking it; adding it to this library does not execute it.

**Read:** [Chinese canonical source](project-workflows/repository-audit-repair-closeout.md)

### Local Recycle Bin Cleanup

**Use when local project files or disk usage need an informed cleanup decision.** The agent first audits a specified scope and explains each candidate's purpose, size, consequences, and recovery conditions. Only candidates selected by the user may then be moved to the system recycle bin after a fresh boundary check.

**Safety boundary:** no permanent deletion or emptying the recycle bin. Moving files to the recycle bin is not proof of freed disk space. Requires access to the actual host files and a supported system recycling mechanism.

**Read:** [Chinese canonical source](project-workflows/local-recycle-bin-cleanup.md)

## Putting them together

```text
Global behavior rules
        ↓
Repository facts and existing instructions
        ↓
One task-specific protocol, when appropriate
        ↓
Implement or inspect
        ↓
Verify evidence and review the diff
        ↓
Authorized closeout
```

**Example paths:** *Screenshot → Spec → Implementation → Fidelity Refinement.* Or *Change → Unified Review (CHANGE mode) → Verified Closeout.* The protocols complement real project instructions; they never override explicit permissions or facts on disk.

## How to use a protocol

1. **Open the canonical Markdown file.** Each phase protocol is designed to stand alone; this index cannot replace it.
2. **Place it in the right context.** Global rules may belong in persistent instructions; a review or visual-refinement protocol is for the relevant task.
3. **Check your agent's conventions.** Automatic loading of `AGENTS.md`, `CLAUDE.md`, and other instruction files depends on the tool, not on a universal standard.
4. **Verify the outcome yourself.** A disciplined prompt does not guarantee correct code, an accurate review, or a matching visual result.

## File map

```text
agent-rules/          Codex, Claude Code, Antigravity, and universal persistent rules
ui-workflows/         Screenshot specification and visual refinement
project-workflows/    Task closeout, audit-to-repair delivery, and local cleanup
review-workflows/     Unified code review and repository audit
init-workflows/       AGENTS.md / CLAUDE.md initialization
README.md            English navigation and protocol showcase
MAINTENANCE.md       Prompt lifecycle and language policy
```

## Contributing

The most valuable improvement is **a specific, reproducible failure or a verified correction**. Document the context, why the protocol was insufficient, and the smallest change that solves it. Keep source files authoritative and the English README accurate.

See [MAINTENANCE.md](MAINTENANCE.md) for canonical-versus-derived content, language policy, stable paths, and review requirements. Avoid speculative prompt expansion, duplicate `-v2` files, and claims of universal effectiveness.

## License

Released under the **[MIT License](LICENSE)**. This is a small, plain-text protocol library—no application, build system, or installation step is required.

---

<p align="center"><sub>LESS IMPROVISATION. MORE EVIDENCE. WORK THAT CAN BE REVIEWED.</sub></p>
