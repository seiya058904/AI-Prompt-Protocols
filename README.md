<h1 align="center">🧭 AI Prompt Protocols</h1>

<p align="center">
  <strong>Less improvisation. More reproducible agent work.</strong>
</p>

<p align="center">
  A small, maintained field guide for AI-assisted software development.<br>
  Seven focused protocols. Clear boundaries. Evidence before confidence.
</p>

<p align="center">
  <a href="README.md"><strong>English</strong></a>
  &nbsp;·&nbsp;
  <a href="README.zh-CN.md">简体中文</a>
  &nbsp;·&nbsp;
  <a href="#start-with-the-task">🗂️ Find a Protocol</a>
  &nbsp;·&nbsp;
  <a href="#putting-them-together">🔁 Workflow</a>
  &nbsp;·&nbsp;
  <a href="MAINTENANCE.md">⚙️ Maintenance</a>
</p>

<p align="center">
  <sub>GLOBAL RULES &nbsp;·&nbsp; UI FIDELITY &nbsp;·&nbsp; CODE REVIEW &nbsp;·&nbsp; REPOSITORY SETUP &nbsp;·&nbsp; PROJECT CLOSEOUT</sub>
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
      <p><a href="agent-rules/codex-global-rules.md"><strong>Codex Global Rules →</strong></a><br><a href="agent-rules/universal-coding-agent-global-rules.md">Universal Coding Agent Rules →</a></p>
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
      <p>Build project-specific instructions from verified facts; review code without inventing defects.</p>
      <p><a href="init-workflows/universal-agent-init.md"><strong>Universal Agent Init →</strong></a><br><a href="review-workflows/expert-code-review.md">Expert Code Review →</a></p>
    </td>
    <td width="50%" valign="top">
      <h3>04 / Finish Without Losing Work</h3>
      <p><sub>VALIDATION · DIFF · AUTHORIZATION</sub></p>
      <p>Inspect the real working tree, complete proportionate verification, and perform only permitted integration actions.</p>
      <p><a href="project-workflows/project-closeout.md"><strong>Project Closeout →</strong></a></p>
    </td>
  </tr>
</table>

| Task | Recommended starting point | When |
| --- | --- | --- |
| Predictable Codex sessions | [Codex Global Rules](agent-rules/codex-global-rules.md) | Persistent Codex instructions |
| Consistent cross-agent behavior | [Universal Coding Agent Global Rules](agent-rules/universal-coding-agent-global-rules.md) | Persistent, tool-independent instructions |
| Interpret a screenshot | [UI Screenshot → Implementation Spec](ui-workflows/ui-screenshot-to-implementation-spec.md) | Before implementation |
| Match a reference image | [UI Visual Fidelity Refinement](ui-workflows/ui-visual-fidelity-refinement.md) | After a working first render |
| Finish a scoped task | [Project Closeout](project-workflows/project-closeout.md) | After verification and diff review |
| Review a change | [Expert Code Review](review-workflows/expert-code-review.md) | Before approving or merging |
| Set up repository guidance | [Universal Agent Init](init-workflows/universal-agent-init.md) | On project adoption or instruction audit |

## Protocols in context

The files linked below are the **authoritative prompts**. This README is the map—not a second, abbreviated copy to give an agent instead of the real document.

### Codex Global Rules

**Use when:** a Codex session needs stable expectations around scope, edits, testing, and external actions. It emphasizes the smallest safe change, preservation of existing work, proportionate checks, and accurate completion reports.

**Read:** [English source](agent-rules/codex-global-rules.md) · [Chinese translation](translations/zh-CN/agent-rules/codex-global-rules.md)

### Universal Coding Agent Global Rules

**Use when:** the same working principles must survive a change of coding assistant. The rules avoid assuming one tool interface, while retaining explicit boundaries for destructive operations, repository state, and permission to act.

**Read:** [English source](agent-rules/universal-coding-agent-global-rules.md) · [Chinese translation](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md)

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

### Expert Code Review Protocol

**Use before accepting a change.** Report issues that are supported by evidence, specific enough to fix, materially relevant, and not duplicates. Separate a confirmed finding from a question that **needs verification**; zero invented findings is better than a long speculative report.

**Read:** [Source protocol](review-workflows/expert-code-review.md)

### Universal Agent Init

**Use when adopting or auditing a repository.** Inspect the actual source, commands, tests, and existing instructions before updating `AGENTS.md` or `CLAUDE.md`. Preserve valuable guidance and user changes instead of pasting a generic template.

**Read:** [Source protocol](init-workflows/universal-agent-init.md)

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

**Example paths:** *Screenshot → Spec → Implementation → Fidelity Refinement.* Or *Change → Expert Review → Verified Closeout.* The protocols complement real project instructions; they never override explicit permissions or facts on disk.

## How to use a protocol

1. **Open the canonical Markdown file.** Each phase protocol is designed to stand alone; this index cannot replace it.
2. **Place it in the right context.** Global rules may belong in persistent instructions; a review or visual-refinement protocol is for the relevant task.
3. **Check your agent's conventions.** Automatic loading of `AGENTS.md`, `CLAUDE.md`, and other instruction files depends on the tool, not on a universal standard.
4. **Verify the outcome yourself.** A disciplined prompt does not guarantee correct code, an accurate review, or a matching visual result.

## File map

```text
agent-rules/          Codex-specific and universal persistent rules
ui-workflows/         Screenshot specification and visual refinement
project-workflows/    Deliberate, authorized task closeout
review-workflows/     Evidence-led code review
init-workflows/       AGENTS.md / CLAUDE.md initialization
translations/zh-CN/   Chinese translations of canonical English rules
README.md            English navigation and protocol showcase
README.zh-CN.md      Chinese mirror with matching headings / anchors
MAINTENANCE.md       Canonical-source and translation lifecycle
```

## Contributing

The most valuable improvement is **a specific, reproducible failure or a verified correction**. Document the context, why the protocol was insufficient, and the smallest change that solves it. Keep source files authoritative and the English/Chinese README navigation aligned.

See [MAINTENANCE.md](MAINTENANCE.md) for canonical-versus-derived content, translations, stable paths, and review requirements. Avoid speculative prompt expansion, duplicate `-v2` files, and claims of universal effectiveness.

## License

Released under the **[MIT License](LICENSE)**. This is a small, plain-text protocol library—no application, build system, or installation step is required.

---

<p align="center"><sub>LESS IMPROVISATION. MORE EVIDENCE. WORK THAT CAN BE REVIEWED.</sub></p>
