---
name: create-issues
description: >-
  Break a plan, spec, or PRD into independently-grabbable tasks and save them as markdown files in the local Obsidian vault (.docs/tasks folder).
---

# To Issues (Local Obsidian Vault)

Break a plan into independently-grabbable tasks using vertical slices (tracer bullets) and save them locally.

## Process

### 1. Gather context
Work from whatever is already in the conversation context. If the user passes a PRD reference, fetch it from the local `.docs/` folder (e.g., `prd.md`) and read its full body.

### 2. Explore the codebase (optional)
If you have not already explored the codebase, do so to understand the current state of the code. Task descriptions should use the project's domain glossary vocabulary, and respect ADRs in the area you're touching.

### 3. Draft vertical slices
Break the plan into **tracer bullet** tasks. Each task is a thin vertical slice that cuts through ALL integration layers end-to-end.

Slices may be 'HITL' (Human in the Loop) or 'AFK' (Away from Keyboard). HITL slices require human interaction/approval. AFK slices can be implemented directly. Prefer AFK over HITL where possible.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 4. Quiz the user
Present the proposed breakdown as a numbered list. For each slice, show:
- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Blocked by**: which other slices must complete first
- **User stories covered**: which user stories this addresses

Ask the user:
- Does the granularity feel right?
- Are the dependency relationships correct?
- Should any slices be merged or split further?

Iterate until the user approves the breakdown.

### 5. Save the tasks to the Obsidian Vault
- If `.docs/tasks/` does not exist, ask the user where task notes are stored before creating any files.
- For each approved slice, create a new Markdown file in the `.docs/tasks/` folder (e.g., `.docs/tasks/task-1-setup-db.md`).

**LINKING RULE:** Publish tasks in dependency order (blockers first) so you can reference real task files in the "Blocked by" field using Obsidian bi-directional links (e.g., `[[task-1-setup-db]]`).

<task-template>
> **Parent PRD:** [[prd]]

## What to build
A concise description of this vertical slice. Describe the end-to-end behavior.
*If there are related design inspirations, link them here (e.g., `[[inspiration-note]]`).*

## Acceptance criteria
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by
- [[blocking-task-name]] (if any)
- Or "None - can start immediately" if no blockers.

</task-template>

