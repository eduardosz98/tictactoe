---
mode: product-manager
description: Brainstorm a new product idea and create it as a tracked GitHub issue (single source of truth) on the project board.
---
# /gh-idea

Read `.github/GH_WORKFLOW.md` first for the overall pipeline and conventions.

## Goal
Turn a raw idea from the user into a `[Idea]` GitHub issue in `diogogcunha/tictactoe`,
added to project `5` (owner `eduardosz98`) with `Status = Todo`. This issue is the
only artifact produced — do not create local markdown files.

## Steps
1. Ask the user for the idea if not already provided in the conversation. Clarify:
   what problem it solves, who it's for, and roughly how big it is.
2. Skim `README.md` (Tech Stack, Architecture, Current Sprint, Future Considerations)
   so the idea fits existing conventions and doesn't duplicate an open issue.
   Check open issues first: `gh issue list --repo diogogcunha/tictactoe --state open`.
3. Draft a short issue body with these sections: `## Problem`, `## Proposed idea`,
   `## Why now`, `## Open questions`.
4. Create the issue:
   ```bash
   gh issue create --repo diogogcunha/tictactoe \
     --title "[Idea] <concise title>" \
     --body "<the drafted body>"
   ```
5. Add it to the project and set Status to Todo:
   ```bash
   item_id=$(gh project item-add 5 --owner eduardosz98 --url <issue-url> --format json --jq '.id')
   gh project item-edit --project-id PVT_kwHOAoMIQ84Bdrqi --id "$item_id" \
     --field-id PVTSSF_lAHOAoMIQ84BdrqizhYLfZ4 --single-select-option-id f75ad846
   ```
6. Report back the issue number and URL. Tell the user to run `/gh-prd #<n>` next.
