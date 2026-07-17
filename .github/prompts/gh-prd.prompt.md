---
mode: product-manager
description: Expand an existing [Idea] GitHub issue into a full PRD by rewriting that same issue in place.
---
# /gh-prd

Read `.github/GH_WORKFLOW.md` first. Requires an idea issue number as input
(e.g. `/gh-prd #16`). If no number is given, ask for one.

## Goal
Rewrite the target issue's body into a complete PRD and retitle it. Do **not**
create a new issue or any local file — the idea issue becomes the PRD issue.

## Steps
1. Fetch current content: `gh issue view <n> --repo diogogcunha/tictactoe --json title,body,number`
2. Ask the user any clarifying questions needed to write a real PRD (target users,
   success metrics, constraints, out-of-scope items) if the idea issue doesn't
   already answer them.
3. Write the new body with sections: `## Problem`, `## Goals`, `## Non-goals`,
   `## User stories`, `## Acceptance criteria`, `## Out of scope`, `## Open questions`.
   Preserve useful content from the original idea body.
4. Update the issue in place:
   ```bash
   gh issue edit <n> --repo diogogcunha/tictactoe \
     --title "[PRD] <title without prefix>" \
     --body-file <(printf '%s' "<new body>")
   ```
5. Leave project Status as `Todo` (planning isn't implementation yet).
6. Report the updated issue URL and tell the user to run `/gh-techspec #<n>` next.
