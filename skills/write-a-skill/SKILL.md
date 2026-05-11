---
name: write-a-skill
description: >-
  Create concise agent skills adapted for the local Obsidian vault system. New
  skills MUST use local .docs/ paths and [[wikilinks]] instead of GitHub integrations.
  Use when the user wants to create, rewrite, refine, or review a skill.
---
# Write a Skill (Obsidian Vault Context)

## Quick Start

This repo uses a **local Obsidian Vault** memory system. All new skills MUST follow the vault-first pattern — no GitHub Issues, no external publishing.

Create or update `SKILL.md` with:

```md
---
name: skill-name
description: What it does. Use when specific trigger/context appears.
---

## Quick Start
[Minimal usage guidance]

## Workflow
[Only the steps needed to run the skill]
```

## Workflow

1. Clarify the skill's task, triggers, inputs, outputs, and required tools.
2. Determine if the skill needs to **read from** or **write to** the local vault.
3. Write the shortest useful `SKILL.md` using the Vault Integration Rules below.
4. Split rarely needed details into one-level references, e.g. `REFERENCE.md`.
5. Add scripts only for deterministic, repeated, or error-prone operations.
6. Review for clear triggers, concise instructions, and no stale/time-sensitive facts.

## Vault Integration Rules [MUST]

All skills in this repo operate against a local Obsidian Vault. When writing a skill that reads context or writes output, you MUST apply these rules:

**Reading context:**
- Read task files from `.docs/tasks/` (e.g., `.docs/tasks/task-1-name.md`).
- Read PRDs from `.docs/` (e.g., `.docs/prd-feature.md`).
- Read inspiration references from `.docs/inspirations/`.
- If a required path does not exist, ask the user where to find it before proceeding.

**Writing output:**
- MUST save output as local Markdown files — do NOT publish to GitHub Issues, GitHub PRs, or any external tracker.
- Use Obsidian bi-directional link syntax for cross-references: `[[note-name]]`.
- Append log entries (review findings, refactor notes, rubber duck summaries) under clearly named headings at the bottom of the relevant task file.

**Fallback rule (apply to every skill that uses vault paths):**
```
If the target path (e.g., `.docs/tasks/`) does not exist,
ask the user where the relevant notes are stored before proceeding.
```

## General Rules

- MUST keep `SKILL.md` under 100 lines unless complexity truly requires more.
- MUST make the description specific enough for trigger selection.
- MUST write the description in third person and include "Use when..."
- MUST prefer direct instructions over explanations.
- MUST avoid speculative sections, deep nesting, and broad background material.
- MUST keep references one level deep.

## Structure

```text
skill-name/
├── SKILL.md
├── REFERENCE.md   (optional)
├── EXAMPLES.md    (optional)
└── scripts/
    └── helper.js  (optional)
```
