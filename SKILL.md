---
name: session-for-project
description: "Curate durable project memory for an established project after an explicit milestone, major refactor, or project-memory consolidation request: audit stable facts, route only needed project-local memory documents, and preserve recoverability. Use only when the user explicitly asks to establish, refresh, consolidate, or govern long-term project memory. Do not invoke for routine session changes, one-off handoffs or compaction, roadmap/PRD/issue work, or continuity already recoverable from Git, issues, or roadmap."
metadata:
  short-description: Curate durable project memory for mature projects
---

# Project Memory Curator

Turn stable facts from a mature project into a small, auditable set of project-local memory documents. This Skill is a **project-memory refresh**, not a routine Session transition, chat summarizer, or replacement for Git, Issues, or Roadmap.

## Trigger boundary

Run the workflow only when the user explicitly asks to establish, refresh, consolidate, or govern the project’s long-term memory. A milestone, major refactor, or request to absorb temporary continuity into formal project documents is a strong reason to run it.

Typical requests that run this Skill:

- “为这个项目建立长期记忆。”
- “整理 / 刷新项目记忆。”
- “项目完成这次大重构后，把稳定事实沉淀下来。”
- “把已有 handoff / 临时状态吸收到正式项目文档。”
- “让以后的 fresh session 不依赖 handoff 也能恢复这个项目。”

Treat these as ordinary development continuity and use the normal workflow instead:

- opening or closing an Issue;
- creating or updating a Roadmap or PRD;
- switching Sessions for unfinished work;
- one-off handoff or chat compaction;
- work whose context is already recoverable from committed Git history, Issues, or Roadmap.

For unfinished work that must cross Sessions and has uncommitted continuity information, recommend a one-time handoff or transient preservation. Do not perform a full project-memory audit unless the user separately asks to absorb that material into long-term project memory.

If the request does not pass this gate, stop before discovery and explain that the project’s normal continuity source is sufficient.

## Boundaries

- Resolve the target from the user’s explicit path; otherwise use the current workspace root. The target may be a Git repository, a non-Git directory, or an empty project.
- This workflow authorizes documentation and project-local reference-asset changes only. Preserve application code, content, dirty-worktree changes, publishing state, deployments, commits, and external systems unless the user separately asks to change them.
- Read every applicable existing agent-instruction file before editing. Preserve all live constraints even when reorganizing them.
- Derive facts from the target’s files, configuration, commands, and state. Do not turn assumptions or prior-chat memory into project rules.
- Select documents by actual project complexity. Do not create placeholder DESIGN, ARCHITECTURE, STATUS, domain, or ADR files merely to satisfy a template.
- Keep Git, Issues, and Roadmap as the primary continuity sources for ordinary development. Store only durable facts or unfinished intent that those sources cannot reliably reveal.

Read [references/document-contract.md](references/document-contract.md) completely before creating or refreshing project memory documents.

## Workflow

1. **Ground.** Confirm the explicit project-memory request and its milestone, major change, or consolidation reason. Resolve the target root, inspect applicable instructions, detect repository state when available, and discover existing project-memory documents with a bounded search that excludes dependencies, generated output, caches, VCS internals, and prior session-memory archives.

2. **Classify.** Separate live instruction files, canonical memory documents, transient continuity files, historical QA, durable domain references, and unrelated human documentation. Account for every document that may be changed, absorbed, or superseded.

3. **Select the schema.** Apply the required / conditional / unnecessary rules in the document contract. Keep an existing healthy document structure and add only the router or pointers that are actually needed; do not rename or copy documents solely to match canonical names.

4. **Choose the history source.** Inspect each source’s Git state before editing:

   - A tracked file at a committed version uses Git history by default. Record its pre-change commit SHA, path, and modification reason in the report; do not create a duplicate full byte snapshot.
   - A tracked file with uncommitted changes remains intact unless the user explicitly authorizes absorbing or reorganizing those changes. When authorized, make a diff or exact backup first and state its purpose.
   - An untracked, transient, external, or soon-to-be-moved file still receives an exact snapshot before it is rewritten, absorbed, or moved because Git does not provide a reliable history for it.
   - A tool-loaded instruction file keeps its live role. Never move or delete it as part of consolidation.

5. **Synthesize.** Create or refresh only the selected documents. Keep an `AGENTS.md` router short when an agent workflow needs routing. Put confirmed product and visual decisions in a design document, verified system and workflow facts in an architecture document, and only unrecoverable unfinished intent in a status document. Use existing authoritative domain docs or ADRs when they already serve the need.

6. **Localize references.** Replace active-document links to temporary or agent-cache files with project-relative copies when the referenced assets exist and are genuinely authoritative. Preserve exact bytes unless conversion is required and authorized. Record missing or ambiguous sources instead of fabricating replacements.

7. **Retire safely.** Move clearly transient or historical files into the new archive only after their information is fully absorbed, inbound references are checked, and project instructions permit the move. Keep live instruction files in place.

8. **Validate.** Check the conditional schema decisions, archive completeness for files that require snapshots, Git-history records for committed files, local Markdown links, absence of ephemeral absolute paths in active memory docs, status size, formatting, and the three scenario acceptance routes in the document contract.

9. **Report.** Name the selected documents and why each exists, the documents deliberately not created, the archive path and checksums when used, the pre-change SHAs used for committed sources, copied assets, validation results, and unresolved risks. Explain the same-directory boundary when ignored or untracked state will not follow another worktree or machine.

## Completion criteria

The task is complete only when the explicit project-memory request has been handled; every changed, absorbed, or superseded document follows the conditional history policy; only complexity-justified memory documents exist or are maintained; existing healthy authority was preserved; active links and paths validate; and all three acceptance scenarios pass:

1. A mature-project milestone or major-refactor memory refresh runs and produces only the needed durable memory.
2. A routine Issue completion followed by a fresh Session does not run this Skill.
3. Unfinished work crossing Sessions with uncommitted continuity information recommends one-time handoff or safe transient preservation rather than a full project-memory refresh.
