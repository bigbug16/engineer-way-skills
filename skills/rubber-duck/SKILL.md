---
name: rubber-duck
description: >-
  Reverse Rubber Duck Debugging. The AI stops coding and explains the current logic, recent changes, or a specific function step-by-step to the user. Logs a summary to the Obsidian Vault.
---
# Reverse Rubber Duck Debugging (Obsidian Vault Context)

You are taking on the role of a developer explaining your own code to a "Rubber Duck" (the user). The goal is to demystify complex logic, ensure the user understands what was just built, and trigger your own step-by-step reasoning to catch hidden logical flaws.

## Step 1: Stop and Explain
When invoked (e.g., `/rubber-duck`), you MUST immediately stop writing or suggesting new code.
Focus on the recently modified code, or the specific file/function the user pointed out.

Explain the code block-by-block using these rules:
- **Be Conversational:** Speak as if you are a developer explaining logic to a peer. 
- **Explain the "Why":** Don't just read the syntax (e.g., don't say "Here is a for loop"). Explain the intent (e.g., "Here I'm looping through the cart items to calculate the total tax").
- **No Jargon Hiding:** If something is complex, break it down into simple terms.

## Step 2: Self-Correction (The Duck Effect)
As you explain the logic step-by-step, critically evaluate your own code.
- If you notice a logical gap, unhandled edge case, or potential bug while explaining, **STOP**.
- Explicitly call out your own mistake to the user (e.g., *"Wait, as I'm explaining this, I realize I forgot to handle the null state here."*).
- Do not fix it yet. Ask the user how they want to proceed.

## Step 3: Vault Documentation (Knowledge Sync)
Once the explanation is complete and the user confirms they understand the logic:
1. Create a brief, high-level summary (TL;DR) of how the logic works (no more than 3-5 bullet points).
2. Open the active task file in the `.docs/tasks/` folder (e.g., `[[task-note]]`).
3. Append this summary at the bottom of the file under the heading `## Rubber Duck Notes`.

**CRITICAL Format Rule:**
You MUST use the exact format below. Replace the date placeholder with today's current date in DD-MM-YYYY format. Do not use plain text paragraphs.

```markdown
## Rubber Duck Notes
**Date:** [Current Date: DD-MM-YYYY]
- **[Component/Block Name]:** [Short plain-words explanation of what it does and why]
- **[Component/Block Name]:** [Short plain-words explanation of what it does and why]
- **Caught Issue:** [If you caught any logic flaw during Step 2, briefly note it here. Otherwise, omit this line.]
```
