# engineer-way-skills

> A fork of [mehmetpekcan/skills](https://github.com/mehmetpekcan/skills), adapted for a **local Obsidian Vault** memory system.

Instead of publishing plans, tasks, and reports to GitHub Issues, these skills read from and write to a local `.docs/` folder structured as an Obsidian vault. This keeps all project knowledge local, linkable, and offline-first.

---

## Vault Folder Convention

All skills in this repo expect the following local folder structure inside your project:

```
your-project/
├── .docs/
│   ├── prd-feature-name.md       ← PRDs (created by create-prd)
│   ├── inspirations/             ← Design references, screenshots, notes
│   │   └── inspiration-note.md
│   └── tasks/                    ← Vertical-slice task files (created by create-issues)
│       ├── task-1-setup-db.md
│       └── task-2-auth-flow.md
├── AGENTS.md                     ← Project coding standards (optional, but recommended)
└── src/
    └── ...
```

**If any of these paths don't exist**, the skills will ask you where to find or create them before proceeding.

---

## Skills Overview

| Skill | What it does |
|---|---|
| `create-prd` | Writes a PRD to `.docs/prd-*.md` using Obsidian `[[links]]` |
| `create-issues` | Breaks a PRD into task files in `.docs/tasks/` |
| `tdd` | Runs TDD loop reading acceptance criteria from vault task files, marks `[x]` when passing |
| `simplify-changes` | Refactors code (YOLO or Control mode), logs changes to the active task file |
| `code-review` | Reviews code against vault task acceptance criteria, logs Blocker/High findings |
| `rubber-duck` | Explains code step-by-step, catches logic flaws, logs TL;DR to active task file |
| `commit-push-pr` | Git commit, push, and PR flow (unchanged from original) |
| `grill-me` | Socratic questioning skill (unchanged from original) |
| `write-a-skill` | Creates new skills following the vault-first convention |

---

## Vault Linking Syntax

Skills use [Obsidian bi-directional links](https://help.obsidian.md/Linking+notes+and+files/Internal+links):

```markdown
[[task-1-setup-db]]          ← links to .docs/tasks/task-1-setup-db.md
[[prd-auth-feature]]         ← links to .docs/prd-auth-feature.md
[[inspiration-login-design]] ← links to .docs/inspirations/inspiration-login-design.md
```

---

## Key Differences from Original

| Original (`mehmetpekcan/skills`) | This Fork |
|---|---|
| PRDs published to GitHub Issues | PRDs saved to `.docs/prd-*.md` |
| Tasks created as GitHub Issues | Tasks created as `.docs/tasks/*.md` |
| TDD has no vault integration | TDD reads task file, marks `[x]` on passing tests |
| Simplify-changes: always shows diff | Simplify-changes: YOLO or Control mode |
| Code review: GitHub/branch context only | Code review: also reads vault task for acceptance criteria |
| No rubber-duck skill | New rubber-duck skill with vault documentation |
| `write-a-skill`: GitHub-oriented | `write-a-skill`: vault-first, local-memory pattern |

---

Original repo: [mehmetpekcan/skills](https://github.com/mehmetpekcan/skills)
