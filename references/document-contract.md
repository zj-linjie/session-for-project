# Project Memory Document Contract

Use this contract to audit, snapshot, synthesize, and validate project session memory. Adapt the content to the target; the canonical paths and safety rules are stable, but project facts are not.

## 1. Bounded discovery

Resolve the target root to an absolute path. If it is a Git repository, inspect `git status --short`, a small recent log, and tracked plus untracked documentation. For non-Git targets, inspect the directory without assuming version control.

Search the root and documentation areas for case-insensitive names such as:

- `AGENTS.md`, `AGENT.md`, `CLAUDE.md`, or another tool-loaded instruction file;
- `DESIGN.md`, `design-qa.md`, visual specifications, or product requirements;
- `ARCHITECTURE.md`, `PROJECT.md`, `CONTEXT.md`, system diagrams, or decision records;
- `STATUS.md`, `NOW.md`, `HANDOFF.md`, checkpoints, or continuation notes.

Exclude `.git`, dependency/vendor directories, build output, caches, generated packages, large content collections unrelated to project operations, and `docs/archive/session-memory/`. Prefer `rg`/`rg --files`; fall back only when unavailable. Read configuration and a small set of relevant entrypoints to verify architecture facts. Do not scan user content or secrets merely to fill the documents.

### Classification

| Class | Examples | Default treatment |
|---|---|---|
| Live instructions | `AGENTS.md`, `CLAUDE.md` | Read fully, snapshot before edits, keep in place, preserve every live constraint. |
| Canonical memory | existing design, architecture, or status docs | Snapshot, then update or consolidate into the canonical path. |
| Transient continuity | `HANDOFF.md`, `NOW.md`, checkpoint notes | Snapshot; move into the new archive only when fully absorbed and unreferenced. |
| Historical evidence | old QA reports, superseded mock notes | Snapshot or relocate to the archive; do not route new sessions to them as current truth. |
| Durable domain reference | ADRs, API docs, schemas, research | Keep in place and point to it from the relevant canonical document when needed. |
| Unrelated human docs | README, contribution guide, user manual | Keep unchanged unless the user explicitly includes them. |

Do not use filenames alone to decide. Inspect enough content and inbound references to classify correctly. Account for overlapping documents without duplicating their rules in multiple active locations.

## 2. Historical snapshot

Create a new archive before the first rewrite:

```text
docs/archive/session-memory/<UTC-YYYYMMDDTHHMMSSZ>/
├── manifest.md
└── <original relative paths...>
```

If the timestamp already exists, add a numeric suffix rather than overwriting it. Copy exact pre-change bytes and preserve relative paths beneath the snapshot. The manifest records:

- creation time and project-relative target;
- original path, snapshot path, checksum, classification, and intended action;
- referenced local assets copied into the project;
- files deliberately left untouched and why.

Use a stable checksum such as SHA-256. Confirm every manifest snapshot exists and matches before rewriting its source. The archive may preserve obsolete absolute paths as historical evidence; active memory documents may not depend on them.

Snapshotting is the default meaning of “archive.” Moving the live source is a separate operation. Move only clearly transient or historical files after their live information has been absorbed, inbound references have been updated, and project instructions permit the move. Never move or delete tool-loaded instructions by default.

## 3. Canonical documents

### Root `AGENTS.md`: always-loaded router

Keep it concise. It should contain only:

- one-sentence project identity when discoverable;
- a resume protocol that checks current state before implementation;
- strong trigger pointers:
  - continuation or unclear dirty state → `docs/STATUS.md`;
  - UI, product, visual, or asset work → `docs/DESIGN.md`;
  - architecture, data, workflow, build, or deployment work → `docs/ARCHITECTURE.md`;
- cross-cutting permission and safety guardrails that apply to nearly every branch;
- the rule for recording durable decisions and clearing transient status.

Keep commands, dependency lists, file inventories, detailed design tokens, workflows, and historical explanations out of the always-loaded file when the environment or a branch document can supply them.

If an `AGENTS.md` already exists, preserve its effective scope and every still-live instruction. Refactor, do not silently weaken. If nested agent files exist, respect their scoped ownership instead of flattening them into the root.

### `docs/DESIGN.md`: durable product and experience truth

Record only confirmed, future-relevant decisions: product identity, audience-facing behavior, interaction patterns, visual system, approved references, responsive/accessibility expectations, and the rule for incorporating new durable feedback.

- Copy authoritative external local reference assets into a project-relative location such as `docs/assets/design/` and update active links.
- Preserve exact bytes unless conversion is required and authorized.
- If no visual or product surface exists, say so in a short document and identify which user-facing output conventions, if any, belong here.
- Distinguish current requirements from historical QA evidence.

### `docs/ARCHITECTURE.md`: durable system and workflow truth

Describe verified runtime boundaries, data sources, persistence, state transitions, authority boundaries, build/deployment seams, and non-obvious invariants. Co-locate a decision with its rationale when that rationale prevents regression.

- Reference configuration or code as the exact interface source instead of copying easy-to-discover command lists and schemas.
- State local-only, ignored, untracked, machine-specific, or worktree-specific persistence limits.
- Do not invent an architecture for an empty directory. Record the absence of runtime decisions and how future decisions should be added.

### `docs/STATUS.md`: lightweight unfinished state

Keep it at roughly 40 lines or fewer. Use one of two explicit states:

- `State: active` — only for a meaningful unfinished objective whose intent cannot be recovered reliably from code and version-control state;
- `State: clear` — no active cross-session work.

For an active status, include last verification time, objective, verified facts, changes to preserve, next concrete action, user decision or blocker, and known failing checks. Record uncertainty as uncertainty. Do not report a stale test result as current.

When the listed objective completes, reset the file to `State: clear` and a short no-active-work statement. Do not accumulate completed history here; the archive and version control already provide history.

## 4. Active-document quality rules

- One meaning has one authoritative home. Use pointers instead of duplication.
- Durable user decisions go to design or architecture; temporary progress goes to status.
- Active docs use project-relative links. Remove dependencies on `/tmp`, agent-generation caches, deleted worktrees, and machine-specific home paths when project-local copies can be made.
- Preserve exact-path and data-access restrictions already present in project instructions.
- Keep public/publish/deploy authority boundaries explicit when the project has such actions.
- Existing dirty changes belong to the user. Documentation may describe unresolved state but must not clean, publish, commit, or reinterpret it.
- A same-directory memory system does not transfer ignored data, uncommitted work, or machine-local assets to another worktree or computer. State that limitation when relevant.

## 5. Validation

Complete all applicable checks:

1. Every changed or superseded pre-existing memory document has a manifest entry and checksum-valid snapshot.
2. `AGENTS.md`, `docs/DESIGN.md`, `docs/ARCHITECTURE.md`, and `docs/STATUS.md` exist.
3. Every project-relative Markdown link resolves; authoritative copied assets exist and match their sources.
4. Active memory docs do not depend on temporary or agent-cache absolute paths.
5. `docs/STATUS.md` stays within the size bound and matches current observable state.
6. In Git targets, `git diff --check` passes and unrelated dirty changes remain intact.
7. Route simulation succeeds:
   - a system implementation request reaches architecture facts and authority boundaries;
   - a visual/product request reaches design decisions and approved project-local references;
   - a continuation request reaches the actual unfinished objective or a clear state.
8. Project-required documentation or asset checks pass. Run broader application tests only when project instructions require them or the memory change also affects runtime files.

Report partial failures rather than repairing unrelated project state. A pre-existing failing test or ambiguous legacy document belongs in `docs/STATUS.md` and the final report when it affects safe continuation.
