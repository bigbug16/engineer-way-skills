---
name: create-prd
description: >-
  Turn the current conversation context into a PRD and save it to the local Obsidian vault (.docs folder). Use when user wants to create a PRD from the current context.
---
This skill takes the current conversation context, user inspirations, and codebase understanding and produces a PRD. Do NOT interview the user — just synthesize what you already know.

## Process

1. Explore the repo to understand the current state of the codebase. 
**CRITICAL:** Before writing anything, look for and read any relevant markdown notes or image references in the `.docs/inspirations/` folder. Use the project's domain glossary vocabulary throughout the PRD, and respect any existing ADRs.

2. Sketch out the major modules you will need to build or modify to complete the implementation. Actively look for opportunities to extract deep modules that can be tested in isolation. Check with the user that these modules match their expectations and which modules they want tests written for.

3. Write the PRD using the template below.
**CRITICAL ACTION:** Do NOT publish this to GitHub or any external issue tracker. Save the output directly as a markdown file in the `.docs/` folder (e.g., `.docs/prd-feature-name.md`).
**LINKING RULE:** When referencing existing ideas, user inspirations, or architectural decisions, you MUST use Obsidian's bi-directional linking format (e.g., `[[inspiration-note-name]]`).

<prd-template>

## Related Context
*List any relevant inspiration or context notes here using `[[ ]]` links.*

## Problem Statement
The problem that the user is facing, from the user's perspective.

## Solution
The solution to the problem, from the user's perspective.

## User Stories
A LONG, numbered list of user stories. Each user story MUST be in the format of:
1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

This list of user stories MUST be concise and not overengineer every little detail & edge cases.

## Implementation Decisions
A list of implementation decisions that were made. This can include:
- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

## Testing Decisions
A list of testing decisions that were made. Include:
- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope
A description of the things that are out of scope for this PRD.

## Further Notes
Any further notes about the feature.

</prd-template>
