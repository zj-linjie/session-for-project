---
name: session-for-project
description: "Create or refresh durable, project-local memory for ordinary Codex session continuity: a concise AGENTS.md router plus design, architecture, and active-status documents, with historical snapshots of prior equivalents. Use when a user asks to preserve project context across new sessions, reduce routine handoffs, establish agent-facing project docs, or repair stale project memory. Do not use for one-off chat compaction or a handoff to a different workspace."
metadata:
  short-description: Build durable project session memory
---

# Session Memory for a Project

Build project-local memory that lets a new Codex session safely resume work in the same directory without a routine handoff.

## Boundaries

- Resolve the target from the user’s explicit path; otherwise use the current workspace root. The target may be a Git repository, a non-Git directory, or an empty project.
- This workflow authorizes documentation and project-local reference-asset changes only. Preserve application code, content, dirty-worktree changes, publishing state, deployments, commits, and external systems unless the user separately asks to change them.
- Read every applicable existing agent-instruction file before editing. Preserve all live constraints even when reorganizing them.
- Archive by exact snapshot before rewriting or superseding. A snapshot never grants permission to delete a live instruction file.
- Treat the environment as source of truth. Derive facts from the target’s files, configuration, commands, and state; do not turn assumptions or prior-chat memory into project rules.

Read [references/document-contract.md](references/document-contract.md) completely before creating or refreshing the project memory documents.

## Workflow

1. **Ground.** Resolve the target root, inspect applicable instructions, detect repository state when available, and discover existing project-memory documents with a bounded search that excludes dependencies, generated output, caches, VCS internals, and prior session-memory archives.
2. **Classify.** Separate live instruction files, canonical memory documents, transient continuity files, historical QA, durable domain references, and unrelated human documentation. Every document that will be changed, absorbed, or superseded must be accounted for.
3. **Snapshot.** Before changing those documents, copy their exact bytes into a timestamped project-local archive with a manifest and checksums. Preserve relative paths. Never overwrite an earlier snapshot.
4. **Synthesize.** Create or refresh the canonical set: root `AGENTS.md`, `docs/DESIGN.md`, `docs/ARCHITECTURE.md`, and `docs/STATUS.md`. Keep `AGENTS.md` as a short trigger-based router; move branch-specific facts to the matching document. If the project has no UI, runtime, or active work, state that plainly instead of inventing content.
5. **Localize references.** Replace active-document links to temporary or agent-cache files with project-relative copies when the referenced assets exist and are genuinely authoritative. Record missing or ambiguous sources in `docs/STATUS.md` rather than fabricating replacements.
6. **Retire safely.** Move clearly transient, fully absorbed legacy documents into the new archive only after checking that no active file points to them. Keep `AGENTS.md`, `CLAUDE.md`, and other tool-loaded instruction files in place; snapshot and update them without removing their live role.
7. **Validate.** Check archive completeness, local Markdown links, the status size bound, absence of ephemeral absolute paths in active memory docs, formatting, and the three recovery routes: implementation/system work, visual/product work, and continuation of unfinished work.
8. **Report.** Name the canonical documents, archive path, status state, copied assets, validation results, and unresolved risks. Explain the same-directory boundary when ignored or untracked state will not follow another worktree or machine.

## Completion criteria

The task is complete only when every pre-existing memory document is either preserved as live, represented in the archive manifest, or explicitly identified as unrelated; all four canonical documents exist; their facts are supported by the target; and a fresh session can choose the correct detailed document from `AGENTS.md` without reading a handoff.
