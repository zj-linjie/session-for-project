# Project Memory Document Contract

Use this contract to audit, snapshot, synthesize, and validate project memory after an explicit project-memory governance request. Adapt the content to the target: document paths, facts, and complexity are project-specific. The contract does not turn routine Session changes, Issues, Roadmaps, or handoffs into a memory-refresh task.

## 1. Bounded discovery

Resolve the target root to an absolute path. If it is a Git repository, inspect `git status --short`, a small recent log, and tracked plus untracked documentation. For non-Git targets, inspect the directory without assuming version control.

Search the root and documentation areas for case-insensitive names such as:

- `AGENTS.md`, `AGENT.md`, `CLAUDE.md`, or another tool-loaded instruction file;
- `DESIGN.md`, `design-qa.md`, visual specifications, or product requirements;
- `ARCHITECTURE.md`, `PROJECT.md`, `CONTEXT.md`, system diagrams, or decision records;
- `STATUS.md`, `NOW.md`, `HANDOFF.md`, checkpoints, or continuation notes.

Exclude `.git`, dependency/vendor directories, build output, caches, generated packages, large content collections unrelated to project operations, and `docs/archive/session-memory/`. Prefer `rg`/`rg --files`; fall back only when unavailable. Read configuration and a small set of relevant entrypoints to verify architecture facts. Do not scan user content or secrets merely to fill documents.

### Classification

| Class | Examples | Default treatment |
|---|---|---|
| Live instructions | `AGENTS.md`, `CLAUDE.md` | Read fully, preserve the live role, and never move or delete by default. Apply the history policy in section 2 before an authorized rewrite. |
| Existing healthy memory | design, architecture, status, `CONTEXT.md`, or ADR documents | Keep the existing authority and links. Refresh only the parts justified by the request; do not rename or copy solely for canonical naming. |
| Transient continuity | `HANDOFF.md`, `NOW.md`, checkpoint notes | Preserve with an exact snapshot when it will be changed or absorbed. Move only after full absorption and reference checks. |
| Historical evidence | old QA reports, superseded mock notes | Keep as human reference or snapshot/relocate when explicitly in scope; do not route new sessions to it as current truth. |
| Durable domain reference | API docs, schemas, research | Keep in place and point to it from selected memory documents when needed. |
| Unrelated human docs | README, contribution guide, user manual | Keep unchanged unless the user explicitly includes them. |

## 2. Conditional history policy

Determine each source’s state immediately before editing. Snapshotting is conditional, not a blanket precondition for every existing document.

### A. Tracked and committed

A file is in this category when it is tracked and its current bytes are clean relative to the pre-change `HEAD`. Git is the authoritative history source. Do not create a duplicate full byte snapshot just because the file is being refreshed.

Record in the user-facing report:

- the pre-change commit SHA (`HEAD` or the exact base used);
- the project-relative file path;
- why the file is being changed;
- the Git recovery command, such as `git show <sha>:<path>`.

Use `git diff <old>..<new>` after the change when a comparison is useful. If a committed file is also a live instruction file, protect its role while editing; the absence of an archive copy does not reduce that protection.

### B. Tracked with uncommitted changes

Do not overwrite, clean, reinterpret, or silently absorb a dirty file. If the file is outside the requested refresh, leave it untouched and report it. If the user explicitly authorizes absorbing or reorganizing its current changes, create an exact backup or focused diff before rewriting and record the backup’s purpose. Preserve unrelated dirty hunks.

### C. Untracked, transient, external, or being moved

Git does not provide a reliable recovery source for these files. Before rewriting, absorbing, or moving one, copy its exact bytes into a new archive:

```text
docs/archive/session-memory/<UTC-YYYYMMDDTHHMMSSZ>/
├── manifest.md
└── <original relative paths...>
```

If the timestamp already exists, add a numeric suffix rather than overwriting it. For an external source, record its source identity and preserve a safe project-relative representation in the manifest. The manifest records creation time, project-relative target, original path, snapshot path, SHA-256 checksum, classification, intended action, referenced local assets copied into the project, and files deliberately left untouched with the reason.

Confirm every required snapshot exists and matches before rewriting its source. If no source requires a snapshot, do not create an empty archive merely to satisfy this contract; report that committed Git history was used instead.

### D. Tool-loaded instruction files

Protect the file’s live role and scope in all categories. Never move or delete it as part of consolidation. A clean tracked instruction file may use Git history; a dirty or untracked one follows the corresponding safe-backup rule above.

## 3. Document selection

The memory schema follows actual project complexity. Decide each candidate as `required`, `conditional`, or `unnecessary` before synthesis and record the evidence for the decision.

| Document or source | Required | Conditional | Unnecessary |
|---|---|---|---|
| Existing live instruction file | Preserve every existing file that the project or tool loads. | Add a new instruction file only when the project’s own workflow establishes that need. | Creating an absent instruction file only to host this Skill’s output. |
| Root `AGENTS.md` router | Keep an existing router’s live role; create one when an agent workflow has multiple memory branches or durable project instructions that need routing. | Use an existing instruction file’s pointer when one clear route is enough. | No agent-facing workflow or no durable route to expose. |
| `docs/STATUS.md` or equivalent | Preserve an existing active status document that contains unrecoverable cross-Session intent. | Create or maintain it only for a meaningful unfinished objective that Git, Issues, and Roadmap cannot recover. | No active continuity, or status is already fully represented by normal development sources. |
| `docs/DESIGN.md` or existing design authority | Preserve an existing healthy design authority. | Create or refresh one when stable UI, UX, product, accessibility, or visual constraints need long-term routing. | No user-facing surface or no durable design facts. |
| `docs/ARCHITECTURE.md` or existing system authority | Preserve an existing healthy system authority. | Create or refresh one when cross-module, runtime, data-flow, workflow, deployment, or authority-boundary facts need long-term routing. | No such verified system facts, especially in a simple or empty project. |
| `CONTEXT.md` / domain document | Preserve an existing authoritative domain document. | Add or refresh one when complex domain semantics cannot be recovered from code and existing references. | No complex domain model or when an existing reference already serves it. |
| ADR | Preserve real, durable decisions already recorded. | Add one only for a real long-lived decision whose rationale prevents regression. | No durable decision or when the rationale is already authoritative elsewhere. |

“Required” describes preservation or a demonstrated routing need; it does not mean every project receives that file. “Conditional” describes documents created only when evidence supports them. “Unnecessary” means leave the candidate absent or untouched. A new/simple project may therefore produce no DESIGN or ARCHITECTURE placeholder, and a project with a healthy differently named structure does not need a migration.

### Canonical placement

Use canonical paths as defaults for new documents, not as a reason to duplicate an existing authority:

- `AGENTS.md` is a concise router, not a full project snapshot;
- a design authority records confirmed product and experience truth;
- a system authority records verified runtime, data, workflow, deployment, and authority boundaries;
- a status authority records only unrecoverable unfinished intent;
- domain docs and ADRs remain where their project ownership already places them.

If no visual or product surface, system boundary, or active continuity exists, leave the corresponding document absent. If an existing document is healthy, point to it from the smallest useful router rather than copying it.

When a router is needed, keep it to the project identity, a current-state resume check, pointers to the selected detail documents, and cross-cutting safety rules. Keep commands, dependency inventories, detailed design tokens, and historical explanations in the environment or branch-specific documents.

A selected design authority records only confirmed, future-relevant product identity, audience-facing behavior, interaction patterns, visual system, approved references, responsive/accessibility expectations, and the rule for incorporating durable feedback. A selected system authority records verified runtime boundaries, data sources, persistence, state transitions, authority boundaries, build/deployment seams, non-obvious invariants, and local-only or machine-specific persistence limits. Reference configuration or code as the exact source instead of copying easy-to-discover commands and schemas; never invent architecture for an empty project.

When a status authority is selected, keep it roughly 40 lines or fewer. Use `State: active` only for meaningful unfinished intent that cannot be recovered from code and version-control state, and include last verification time, objective, verified facts, changes to preserve, next concrete action, user decision or blocker, and known failing checks. When the objective completes, reset a retained file to `State: clear` and a short no-active-work statement; do not accumulate completed history there.

## 4. Active-document quality rules

- One meaning has one authoritative home. Use pointers instead of duplication.
- Durable user decisions go to design, architecture, domain, or ADR authority; temporary progress goes to the selected status authority.
- Active docs use project-relative links. Remove dependencies on `/tmp`, agent-generation caches, deleted worktrees, and machine-specific home paths when project-local copies can be made.
- Preserve exact-path and data-access restrictions already present in project instructions.
- Keep public/publish/deploy authority boundaries explicit when the project has such actions.
- Existing dirty changes belong to the user. Documentation may describe unresolved state but must not clean, publish, commit, or reinterpret it.
- A same-directory memory system does not transfer ignored data, uncommitted work, or machine-local assets to another worktree or computer. State that limitation when relevant.
- Keep a selected status authority within the size bound and follow its state/content rule in section 3.

## 5. Validation

Complete all applicable checks:

1. The schema decision for every candidate is recorded as required, conditional, or unnecessary, with evidence.
2. Every changed or superseded source that needs a snapshot has a checksum-valid manifest entry; clean tracked sources have a pre-change SHA and recovery path in the report instead of a duplicate archive copy.
3. Existing live instruction files remain in place with their effective constraints intact.
4. Every project-relative Markdown link resolves; authoritative copied assets exist and match their sources.
5. Active memory docs do not depend on temporary or agent-cache absolute paths.
6. Any active `STATUS.md` stays within the size bound and matches current observable state.
7. In Git targets, `git diff --check` passes and unrelated dirty changes remain intact.
8. The three scenario acceptance routes below pass.
9. Project-required documentation or asset checks pass. Run broader application tests only when project instructions require them or the memory change also affects runtime files.

### Three scenario acceptance

Use these synthetic requests as a trigger and routing regression check. Record the decision and evidence in the final report.

| Scenario | Synthetic request | Expected decision | Required evidence |
|---|---|---|---|
| Milestone refresh | “这个成熟项目刚完成一次大重构，请整理稳定事实，让 fresh Session 不依赖 handoff 也能恢复。” | Run this Skill. Select only the documents justified by verified product, system, domain, or active-state facts. | The explicit memory intent and milestone pass the gate; committed sources use Git SHA records; only untracked/transient sources receive archives. |
| Routine Issue transition | “Issue #42 做完了，我准备开一个 fresh Session，下一步继续开发。” | Do not run this Skill. Continue through normal Git/Issue/Roadmap workflow. | The request has no memory-governance intent; no discovery, archive, schema synthesis, or four-document creation is performed. |
| Unfinished dirty continuity | “这个未完成任务必须跨 Session，当前有未提交的 handoff/临时状态，请保留现场。” | Recommend a one-time handoff or safe transient preservation. Do not run a full project-memory refresh. | Dirty information is not overwritten; no full memory audit occurs; a later explicit request to absorb it can enter this Skill with the appropriate backup policy. |

The first scenario must execute the refresh path; the second must be rejected at the trigger gate; the third must be routed to handoff/transient preservation. Passing means the response and actions match those decisions, not merely that the phrases appear in a document.
