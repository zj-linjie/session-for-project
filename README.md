<p align="center">
  <img src="./assets/readme/hero.svg" width="100%" alt="Session for Project: durable project memory for Codex" />
</p>

<p align="center">
  <strong>A reusable Codex Skill for keeping project context in the repository.</strong><br />
  New session? Read the project. Continue the work.
</p>

<p align="center">
  <a href="#what-it-solves">What it solves</a> ·
  <a href="#how-it-works">How it works</a> ·
  <a href="#install">Install</a> ·
  <a href="#use-it">Use it</a>
</p>

## What it solves

Chat history is a poor place to keep a project’s durable truth. Important decisions get repeated in handoffs, temporary paths go stale, and a new session has to reconstruct intent from a dirty worktree.

`session-for-project` turns that context into a small, auditable document system:

- `AGENTS.md` stays short and routes each kind of task to the right detail.
- `docs/DESIGN.md` keeps confirmed product and visual decisions.
- `docs/ARCHITECTURE.md` keeps verified data flow, state, authority, and deployment boundaries.
- `docs/STATUS.md` keeps only unfinished intent that code and Git cannot reveal.
- Previous equivalents are copied into a timestamped archive before they are refreshed.

It works for Git repositories, non-Git directories, and empty projects. It is designed for ordinary new sessions in the same project directory, so routine handoffs become the exception instead of the storage layer.

## How it works

<p align="center">
  <img src="./assets/readme/workflow.svg" width="100%" alt="Five-stage recovery loop: ground, snapshot, route, validate, resume" />
</p>

The workflow is deliberately conservative:

1. Ground in the current files, instructions, and worktree state.
2. Snapshot existing memory documents with checksums before changing them.
3. Route durable facts into the canonical documents instead of duplicating them.
4. Validate links, paths, status size, archives, and the three recovery routes.
5. Let the next session choose the relevant document from `AGENTS.md`.

## Install

Clone this private repository into your Codex skills directory:

```bash
git clone git@github.com:zj-linjie/session-for-project.git "${CODEX_HOME:-$HOME/.codex}/skills/session-for-project"
```

If the directory already exists, update it instead:

```bash
git -C "${CODEX_HOME:-$HOME/.codex}/skills/session-for-project" pull --ff-only
```

The repository contains the Skill entrypoint, its document contract, and this README/visual layer. It does not install dependencies or alter project code.

## Use it

From the project directory, invoke the Skill directly:

```text
$session-for-project 为当前项目建立跨会话记忆
```

Or point it at another directory:

```text
$session-for-project 为 /path/to/project 建立跨会话记忆
```

The Skill reads the project’s existing instructions first. It preserves live `AGENTS.md` and `CLAUDE.md` roles, archives prior memory documents, and reports anything it cannot safely infer.

## The safety model

### Durable vs. temporary

Durable product decisions belong in `DESIGN.md`. Durable system and workflow decisions belong in `ARCHITECTURE.md`. An unfinished objective belongs in `STATUS.md` until it is complete, then the status returns to `clear`.

### Archive before rewrite

The archive is an exact, checksum-verified snapshot under:

```text
docs/archive/session-memory/<UTC-timestamp>/
```

Tool-loaded instructions remain in place. Clearly transient files such as an absorbed handoff may move into the archive only after references are checked.

### Same-directory boundary

The memory documents are project-local. Ignored drafts, uncommitted changes, and machine-local assets do not automatically follow a different Git worktree or another computer; the Skill records that limitation instead of hiding it.

## Repository map

```text
SKILL.md                              # model-facing entrypoint
agents/openai.yaml                   # Codex UI metadata
references/document-contract.md      # audit, snapshot, and validation rules
assets/readme/                        # README visuals and editable SVG sources
```

## What it intentionally does not do

- It does not replace a project’s live instruction files without preserving their rules.
- It does not invent architecture, design decisions, tests, adoption, or compatibility claims.
- It does not publish content, deploy a site, commit code, or push changes as part of memory setup.
- It does not turn every old README, API document, or ADR into session memory.

## License

No license has been declared for this private repository yet.
