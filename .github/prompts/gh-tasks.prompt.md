---
mode: product-engineer
description: Break down a PRD/tech-spec GitHub issue (epic) into concrete [Task] issues on the project board.
---
# /gh-tasks

Read `.github/GH_WORKFLOW.md` first. Requires an epic issue number as input
(e.g. `/gh-tasks #16`). If no number is given, ask for one.

## Goal
Create one `[Task] <title>` issue per independently implementable unit of work
from the epic's PRD + tech spec, each linked back with `Parent: #<epic>`, each
added to project `5` with `Status = Todo`. No local task files.

## Steps
1. Fetch the epic: `gh issue view <epic> --repo diogogcunha/tictactoe --json title,body,number`
2. Break the "Files to add/change" and "User stories"/"Acceptance criteria"
   sections into small, independently shippable tasks. Each task should be
   completable in a single PR.
3. For each task:
   ```bash
   url=$(gh issue create --repo diogogcunha/tictactoe \
     --title "[Task] <task title>" \
     --body "<what to build, acceptance criteria>\n\nParent: #<epic>")
   n=$(basename "$url")
   item_id=$(gh project item-add 5 --owner eduardosz98 --url "$url" --format json --jq '.id')
   gh project item-edit --project-id PVT_kwHOAoMIQ84Bdrqi --id "$item_id" \
     --field-id PVTSSF_lAHOAoMIQ84BdrqizhYLfZ4 --single-select-option-id f75ad846
   ```
4. Post a single checklist comment on the epic issue linking every task:
   ```bash
   gh issue comment <epic> --repo diogogcunha/tictactoe --body "## Tasks
   - [ ] #<n1>
   - [ ] #<n2>
   ..."
   ```
5. Report the list of created task issue numbers/URLs. Tell the user to run
   `/gh-execute-task #<n>` on any of them.
