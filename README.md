# AI Prompt Protocols

**Less improvisation. More reproducible agent work.**

A focused, maintained library of plain-text prompts and engineering protocols for real-world AI-assisted development. Each protocol has a purpose, a recommended phase and a source document you can copy into your own workflow.

[**English**](README.md) · [简体中文](README.zh-CN.md) · [Choose a protocol](#start-with-the-task) · [Maintenance policy](MAINTENANCE.md)

> Not a prompt marketplace, benchmark claim or universal agent framework. These are working documents refined in real projects; choose the **smallest protocol that fits the task**.

## Start with the task

**Pick the moment, not the longest prompt.** This library separates persistent behavior rules from one-shot task protocols.

| Your task | Start here | When to use it |
| --- | --- | --- |
| **Keep Codex sessions predictable** | [Codex Global Rules](agent-rules/codex-global-rules.md) | Persistent Codex-specific instructions |
| **Use different coding agents consistently** | [Universal Coding Agent Global Rules](agent-rules/universal-coding-agent-global-rules.md) | Persistent tool-agnostic instructions |
| **Turn a screenshot into an implementation plan** | [UI Screenshot → Implementation Spec](ui-workflows/ui-screenshot-to-implementation-spec.md) | **Before** building an interface |
| **Bring a rendered UI closer to its reference** | [UI Visual Fidelity Refinement](ui-workflows/ui-visual-fidelity-refinement.md) | **After** the first implementation |
| **Close out finished work safely** | [Project Closeout](project-workflows/project-closeout.md) | After tests, diff review and integration decisions |
| **Review code without speculative noise** | [Expert Code Review](review-workflows/expert-code-review.md) | Before approving a change or merge |
| **Set up project-specific agent guidance** | [Universal Agent Init](init-workflows/universal-agent-init.md) | At repository initialization or audit |

## Protocols in context

### Codex Global Rules

**For:** Codex coding sessions that need predictable boundaries, focused changes and evidence-led verification.

- Ask clarifying questions only for material uncertainty; choose safe, disclosed assumptions otherwise.
- Avoid broad destructive cleanup and unauthorized external actions.
- Inspect what changed, run proportionate checks and distinguish validation from claims.

**Read:** [English source](agent-rules/codex-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md)

### Universal Coding Agent Global Rules

**For:** A shared behavioral baseline across Codex, Claude Code, Cursor and other coding agents.

The same central safety and quality constraints as the Codex rules, expressed without relying on a Codex-only tool interface. Independent read-only checks can be batched where the environment supports it; operations with side effects remain controlled.

**Read:** [English source](agent-rules/universal-coding-agent-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md)

### UI Screenshot → Implementation Spec Protocol

**For:** Turning screenshots or mockups into a specification that an implementation agent can follow.

It separates **OBSERVED**, **ESTIMATED**, **INFERRED** and **UNKNOWN** details, builds a coherent model of layout and responsive relationships, ranks fidelity constraints, and explicitly records missing evidence. This protocol produces a **specification**, not implementation code.

**Read:** [Source protocol](ui-workflows/ui-screenshot-to-implementation-spec.md)

### UI Visual Fidelity Refinement Protocol

**For:** Iterative convergence between a real rendered interface and an actual visual reference.

```text
Capture → Compare → Prioritize → Find the root cause
        → Change one coherent cluster → Render again
        → Accept / adjust / revert
```

The rendered page is the evidence. If the tools cannot capture or compare the real result, the protocol requires transparent limited verification rather than fabricated visual success.

**Read:** [Source protocol](ui-workflows/ui-visual-fidelity-refinement.md)

### Project Closeout Prompt

**For:** Finishing a scoped development task without losing work or pushing to the wrong place.

Inspect repository state and its real integration model, validate task-owned changes, check the diff, then carry out **only authorized** commits, synchronization, releases or deployment actions. Completion means the work reached its intended safe state, not merely that a commit exists.

**Read:** [中文原文](project-workflows/project-closeout.md)

### Expert Code Review Protocol

**For:** High-signal code review before accepting changes.

Only report findings that are evidence-supported, discrete, actionable and materially relevant. Separate confirmed problems from **Needs Verification**. Prefer no finding to a speculative bug report and avoid repeating the same issue under multiple labels.

**Read:** [Source protocol](review-workflows/expert-code-review.md)

### Universal Agent Init

**For:** Bootstrapping or reconciling `AGENTS.md` and `CLAUDE.md` using the *actual* project.

It reads code, tests and commands before documenting them, preserves existing instructions and user changes, avoids generic boilerplate, and does not assume that a missing instruction file justifies a large template.

**Read:** [Source protocol](init-workflows/universal-agent-init.md)

## Putting them together

```text
Global behavior rules
        ↓
Actual repository context and instructions
        ↓
Task-specific protocol
        ↓
Implementation / inspection
        ↓
Verification and diff review
        ↓
Authorized closeout
```

**Examples:** Screenshot work uses **Spec → Implement → Visual Fidelity**. A pre-merge audit uses **Expert Code Review**. A newly adopted repository may begin with **Universal Agent Init**. Persistent global rules are not substitutes for project-specific facts or permissions.

## How to use a protocol

Use the linked Markdown file as the authoritative prompt; the descriptions here are only a map.

1. **Read the source.** Each Markdown document is self-contained for its intended task; the README is a catalog, not the canonical prompt text.
2. **Choose the right scope.** Global rules belong in an appropriate instruction context; phase protocols are invoked when that phase starts.
3. **Check the target agent.** Whether an agent automatically reads `AGENTS.md`, `CLAUDE.md` or another file depends on the product; do not assume universal native support.
4. **Verify in your own environment.** A protocol improves execution discipline but does not guarantee correctness or performance.

## File map

```text
agent-rules/          Persistent coding-agent rules
ui-workflows/         Screenshot specifications and rendered-UI refinement
project-workflows/    Project closeout
review-workflows/     Expert code review
init-workflows/       Agent instruction initialization
translations/zh-CN/   Chinese translations of the English global rules
README.md            English showcase
README.zh-CN.md      Chinese mirror, same headings and anchors
MAINTENANCE.md       Canonical/derived content lifecycle
```

## Contributing

Useful contributions are concrete: a reproducible ambiguity, a verified improvement, a translation correction or a real-world failure case. Avoid bulk-generated prompt collections and unsupported claims of universal effectiveness. Follow [MAINTENANCE.md](MAINTENANCE.md); canonical prompts are curated and should not be rewritten without a specific reason.

## License

[MIT](LICENSE) · Maintained as a small, high-signal plain-text repository.
