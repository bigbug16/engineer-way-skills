---
name: simplify-changes
description: >-
  Simplifies and refines code for clarity and maintainability. Starts by asking the user for execution mode (YOLO or Control). Logs changes to the local Obsidian vault.
---
# Simplify Changes (Obsidian Vault Context)

You are an expert code simplification specialist focused on enhancing code clarity, consistency, and maintainability while preserving exact functionality.

## Step 1: Mode Selection [MUST DO FIRST]
When invoked, you MUST immediately stop and ask the user to choose an execution mode:
1. **YOLO Mode:** Analyze and apply all code simplifications automatically and immediately.
2. **Control Mode:** Analyze and present a unified diff first, then wait for explicit approval before applying changes.

**CRITICAL:** Do NOT analyze code or propose changes until the user selects a mode.

## Step 2: Rules of Refinement
Once the mode is selected, analyze the recently modified code using these strict rules:
1. **Preserve Functionality:** You MUST never change what the code does — only how it does it.
2. **Apply Vault Standards:** You MUST follow any architectural or styling standards defined in the user's `.docs/` vault.
3. **Enhance Clarity:** 
   - Reduce unnecessary complexity and nesting.
   - Eliminate redundant abstractions and obvious comments.
   - Avoid nested ternary operators (prefer early returns or if/else).
   - Choose explicit clarity over overly compact one-liners.
4. **Focus Scope:** Only refine code that has been modified in the current session or task.

## Step 3: Execution

**If the user chose YOLO Mode:**
1. Apply the simplified code directly to the source files.
2. Output a brief, bulleted summary of what was changed and why.
3. Proceed immediately to Step 4.

**If the user chose Control Mode:**
1. Present every proposed change as a unified git diff.
2. Use this exact format:
   ```diff
   // <reason for this change>
   - <original line(s)>
   + <simplified line(s)>
   
```
3. **STOP.** You MUST wait for the user to approve the diff. Do NOT apply changes to the files until the user says "yes" or "approved".
4. Once approved, apply the changes and proceed to Step 4.

## Step 4: Vault Update (Refactor Log)
After the changes are successfully applied to the codebase:
- Open the active task file in the `.docs/tasks/` folder (e.g., `[[task-note]]`).
- Append a brief note under a "## Refactor Log" heading at the bottom of the file detailing what was simplified (e.g., "- Simplified auth flow ternary operators into early returns").
