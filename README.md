# AI Prompt Protocols

A curated collection of practical AI prompts, coding-agent global rules, and workflow protocols refined through real-world use.

## Philosophy

Many prompts look impressive on paper and fall apart in real work. This repository only keeps prompts that have been used in real projects and refined over time: explicit behavioral constraints, verified against real renders and real Git histories, designed to reduce agent drift and unsolicited improvisation.

> tested workflows > clever wording

It is a small, high-signal, plain-text knowledge repository — not a prompt dump, a marketplace, a tutorial site, or a SaaS product.

## Collections

### Agent Rules

- [Codex Global Rules](agent-rules/codex-global-rules.md) — a reusable global rule set for long-running Codex coding sessions
- [Universal Coding Agent Global Rules](agent-rules/universal-coding-agent-global-rules.md) — a tool-agnostic global rule set for coding agents in general

### UI Workflows

- [UI Screenshot → Implementation Spec Protocol](ui-workflows/ui-screenshot-to-implementation-spec.md) — turns a screenshot or design mockup into a precise, executable UI implementation specification
- [UI Visual Fidelity Refinement Protocol](ui-workflows/ui-visual-fidelity-refinement.md) — iteratively converges an implemented UI toward a reference screenshot through render → compare → fix loops

### Project Workflows

- [Project Closeout Prompt](project-workflows/project-closeout.md) — a Git closeout / handoff protocol that safely lands finished work into the repository's default branch

## Usage

The documents are plain-text prompts. Typical ways to use them:

- as initial context for ChatGPT, Claude, Codex, Cursor, or other coding agents
- as global rules where a product supports them natively (for example Codex `AGENTS.md` or Claude Code `CLAUDE.md`)
- as a phase-specific task prompt — for example, generate an implementation spec from a screenshot, then run the visual fidelity refinement loop
- alongside a project's own `AGENTS.md` / `CLAUDE.md`

Note: not every product exposes the same "global rules" mechanism. Treat these documents as context you provide to the model; rely on a native global-rules feature only where the product actually documents one.

## Recommended Workflow

Global Rules → Project-specific context → Task protocol → Verification → Closeout

## Repository Structure

```text
.
├── README.md
├── LICENSE
├── .gitignore
├── agent-rules/
│   ├── codex-global-rules.md
│   └── universal-coding-agent-global-rules.md
├── ui-workflows/
│   ├── ui-screenshot-to-implementation-spec.md
│   └── ui-visual-fidelity-refinement.md
└── project-workflows/
    └── project-closeout.md
```

## Contributing

Everything in this repository has been used in real work and refined over time.

Welcome:

- bug reports and ambiguity reports
- concrete improvements to existing prompts
- real-world usage feedback

Please do not submit bulk-generated or unverified prompt lists. If a prompt survived real projects, describe where and how it was used.

## License

[MIT](LICENSE)
