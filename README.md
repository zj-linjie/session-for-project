<p align="center">
  <img src="./assets/readme/hero.svg" width="100%" alt="Session for Project: durable project memory for Codex" />
</p>

<p align="center">
  <strong>A reusable Codex Skill for curating durable project memory.</strong><br />
  After a milestone, make the project recoverable without a handoff.
</p>

<p align="center">
  <a href="./README.md">English</a> ·
  <a href="./README.zh-CN.md">简体中文</a>
</p>

<p align="center">
  <a href="#what-it-solves">What it solves</a> ·
  <a href="#when-to-use-it">When to use it</a> ·
  <a href="#how-it-works">How it works</a> ·
  <a href="#install">Install</a>
</p>

## What it solves

`session-for-project` is for an established project after a milestone, major refactor, or an explicit request to govern long-term project memory. It turns verified, stable facts into a small, auditable set of project-local pointers and documents so a fresh session can recover the project without relying on chat history.

The schema follows the project rather than a fixed template:

- Keep an `AGENTS.md` router only when an agent workflow needs durable routing.
- Add design, architecture, domain, ADR, or status documents only when verified facts justify them.
- Preserve a healthy existing document structure instead of renaming or copying it to canonical paths.
- Use Git history for tracked, clean, committed documents; archive untracked, transient, external, or explicitly authorized dirty material when it needs protection.

Git, Issues, and Roadmap remain the normal continuity sources for ordinary development. This Skill is a deliberate project-memory refresh, not a step every Session must take.

## When to use it

Run it when the user explicitly asks to establish, refresh, consolidate, or govern project memory—for example:

```text
$session-for-project 为这个成熟项目整理长期记忆
$session-for-project 大重构完成后，把稳定事实沉淀到项目文档
$session-for-project 把已有 handoff 吸收到正式项目记忆
```

Use the normal development workflow for an Issue, Roadmap, PRD, routine Session switch, one-off handoff, or chat compaction. If unfinished work has uncommitted continuity information, prefer a one-time handoff or transient preservation. Explicitly asking to absorb that material into formal project memory is the point at which this Skill becomes appropriate.

The acceptance boundary is simple: a mature-project milestone refresh runs; an Issue completion followed by a fresh Session does not; unfinished dirty continuity is routed to handoff or transient preservation.

## How it works

<p align="center">
  <img src="./assets/readme/workflow.svg" width="100%" alt="Gated project-memory refresh: trigger, ground, select, preserve, validate" />
</p>

The workflow is deliberately gated and adaptive:

1. Confirm explicit project-memory intent and the milestone, major change, or consolidation reason.
2. Ground in current files, instructions, configuration, and worktree state.
3. Select only the memory documents justified by project complexity.
4. Use Git history for clean committed files and snapshot sources Git cannot recover safely.
5. Validate links, paths, dirty-state protection, schema decisions, and the three acceptance scenarios.

## Install

Clone this repository into your Codex skills directory:

```bash
git clone git@github.com:zj-linjie/session-for-project.git "${CODEX_HOME:-$HOME/.codex}/skills/session-for-project"
```

If the directory already exists, update it instead:

```bash
git -C "${CODEX_HOME:-$HOME/.codex}/skills/session-for-project" pull --ff-only
```

The repository contains the Skill entrypoint, its document contract, and this README/visual layer. It does not install dependencies or alter project code.

## Document selection

There is no mandatory four-document bundle. A project may need only an existing instruction file, a router, or one focused authority:

| Need | Document | Decision |
|---|---|---|
| Agent-facing routing | `AGENTS.md` or an existing instruction file | Keep or add only when routing is needed |
| Stable product or visual truth | Design authority | Conditional |
| Runtime, data, workflow, or deployment truth | Architecture authority | Conditional |
| Unrecoverable unfinished intent | `STATUS.md` or equivalent | Conditional |
| Complex domain semantics or durable rationale | Domain doc or ADR | Conditional |

If a need is absent, leave its document absent. A simple project must not acquire empty DESIGN or ARCHITECTURE placeholders merely because this Skill was run.

## The safety model

### Git history first

For a tracked file whose current version is clean and committed, record the pre-change commit SHA, path, and reason in the report and recover the old bytes through Git. Do not create a duplicate full snapshot in `docs/archive/session-memory/`.

Dirty tracked files stay intact unless the user explicitly authorizes absorbing or reorganizing them; then create a focused diff or exact backup first. Untracked, transient, external, or soon-to-be-moved sources still receive exact, checksum-verified snapshots before they change.

Tool-loaded instruction files keep their live role. Existing dirty changes, publishing state, deployments, and external systems remain outside the workflow’s authority.

### Same-directory boundary

Memory documents are project-local. Ignored drafts, uncommitted changes, and machine-local assets do not automatically follow a different Git worktree or another computer; the Skill records that limitation instead of hiding it.

## Responsibilities

| Tool or source | Role |
|---|---|
| Session Docter | Context-cost health, audit, fix, and new-project bootstrap |
| `session-for-project` | Long-term project-memory refresh for an established project |
| Handoff | One unfinished task’s temporary transfer between Sessions |
| Git / Issue / Roadmap | Primary continuity sources during normal development |

## Repository map

```text
SKILL.md                              # model-facing entrypoint and trigger boundary
agents/openai.yaml                   # Codex UI metadata
references/document-contract.md      # schema, history, and acceptance rules
assets/readme/                        # README visuals and editable SVG sources
```

## What it intentionally does not do

- It does not make every Session pass through a project-memory audit.
- It does not replace Git, Issues, Roadmap, one-time handoff, or chat compaction.
- It does not force every project into `AGENTS.md` plus DESIGN/ARCHITECTURE/STATUS placeholders.
- It does not replace a project’s live instruction files or discard their rules.
- It does not invent architecture, design decisions, tests, adoption, or compatibility claims.
- It does not publish content, deploy a site, commit code, or push changes as part of memory setup.
- It does not automatically clean or delete existing project documents.

## License

No license has been declared for this repository yet.
