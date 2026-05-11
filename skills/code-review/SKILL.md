---
name: code-review
description: >-
  Review a PR, branch, or local changes against local Obsidian task requirements.
  Provides severity-ranked findings, bug-fix verification, and logs the report to the Vault.
---
# Code Review (Obsidian Vault Context)
## Quick start
Review the intended change first, then report only actionable findings ordered by severity.
Decide what to review in this order:
1. If the user provides a PR URL, PR number, branch name, or explicit target, review that target.
2. If no target is provided, review staged and changed files in the current working tree.
3. If the target is ambiguous, ask the user to clarify before reviewing.
## Workflows
### 1. Gather Vault Context [CRITICAL]
Before reviewing the code, you MUST understand what was supposed to be built.
- Look for the active or relevant task markdown file in `.docs/tasks/`.
- If `.docs/tasks/` does not exist, ask the user where task notes are stored before proceeding.
- Focus on the "Acceptance criteria" and the intended behavior described in the task.
### 2. Choose Review Target & Diff
For PRs or branches, identify the base branch and inspect:
- `git diff <base-branch>...HEAD`
- `git log <base-branch>...HEAD --oneline`
For local changes, inspect staged and unstaged diffs, and note which files are untracked.
### 3. Review Priorities
Focus on bugs, regressions, security risks, data loss, concurrency hazards, broken contracts, missing validation, and meaningful test gaps.
**Vault Alignment:** Check if the changes fully satisfy the "Acceptance criteria" from the local Obsidian task. The Vault's acceptance criteria are the primary source of truth — do not let the implementation's existing tests define acceptance by themselves.
Keep style, naming, and preference comments out unless they block correctness or maintainability.
Ground every finding in code evidence. Explain why the behavior is wrong, when it happens, and what impact it has.
### 4. Bug-Fix Verification
If the target appears to be a bug fix, verify the fix independently before completing the review. Decide this from the branch name, commit messages, diff, tests, issue references, and user prompt. If it is unclear whether the change is a bug fix, ask the user to clarify.
Follow this required flow even when the branch already includes test coverage:
1. Understand the intended bug and expected behavior from the Obsidian task and user prompt.
2. Write your own temporary reproduction artifact, using the smallest suitable mechanism:
   - a bash script for CLI, integration, build, config, or workflow bugs
   - a unit or integration test for code-level behavior
3. Keep the artifact temporary and separate from the reviewed change unless the user asks to keep it.
4. Check out the base branch or use a clean worktree for the base branch.
5. Run the exact reproduction artifact against the base branch and confirm the bug reproduces.
6. Return to the fix branch.
7. Run the exact same artifact against the fix branch and confirm it passes.
8. If the base branch does not reproduce the bug, or the fix branch does not pass, raise that as a finding or blocker.
Use the Vault's acceptance criteria — not the implementation's existing tests — as the primary source of truth for what "fixed" means.
### 5. Severity Levels
Use these severities:
- **Blocker**: Must fix before merge. Fails the Vault's acceptance criteria, causes severe production breakage, data loss, security exposure, or an unverified/failed bug-fix claim.
- **High**: Likely user-facing bug, regression, security or reliability risk, broken API contract, unsafe migration, or missing critical validation.
- **Medium**: Real correctness, maintainability, test, or edge-case issue with limited blast radius.
- **Low**: Minor issue that is worth fixing but does not materially affect behavior or safety.
### 6. Output & Vault Sync
Lead with findings in the chat, ordered by severity. If there are no findings, say so clearly.
For each finding include:
- severity
- affected file or area
- concise problem statement
- impact
- suggested fix or direction
After findings, include only brief supporting context:
- bug-fix reproduction result, when applicable
- verification commands run
- residual risks or checks not run
Do not bury findings under summaries. Do not fix the code unless the user explicitly asks.
**Vault Update:**
After presenting the findings to the user, locate the task file as follows:
- Use the task file referenced in the user's prompt if one is specified.
- If no task file is specified and multiple tasks exist in `.docs/tasks/`, ask the user which task this review belongs to before writing.
- If `.docs/tasks/` does not exist, ask the user where to log the findings.
Append the review summary under the heading `## Code Review Findings` in the identified task file.
Only log Blocker and High severity issues in the Vault. If there are no major issues, log "Status: Approved".
Format for the Vault:
```markdown
## Code Review Findings
- **Date:** [Current Date: DD-MM-YYYY]
- **Status:** [Approved / Needs Work]
- **[Severity] - [File]:** [Short problem statement] (Omit if Approved)
```
