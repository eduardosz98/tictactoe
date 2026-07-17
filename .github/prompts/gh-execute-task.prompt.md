---
mode: product-engineer
description: Implement a single [Task] GitHub issue end-to-end and open a PR that closes it, using the issue + its parent epic as the only source of truth.
---
# /gh-execute-task

Read `.github/GH_WORKFLOW.md` first. Requires a task issue number as input
(e.g. `/gh-execute-task #21`). If no number is given, ask for one.

## Goal
Implement exactly what the task issue describes, verify it, and open a PR that
closes it. Never invent scope beyond the issue + its parent epic.

## Steps
1. Fetch the task: `gh issue view <task> --repo diogogcunha/tictactoe --json title,body,number`
2. Extract the `Parent: #<epic>` reference from the body and fetch that issue
   too for full PRD/tech-spec context: `gh issue view <epic> --repo diogogcunha/tictactoe --json title,body`
3. Move the task's project item to `In Progress` before starting:
   ```bash
   item_id=$(gh project item-list 5 --owner eduardosz98 --format json \
     --jq '.items[] | select(.content.number=='"<task>"') | .id')
   gh project item-edit --project-id PVT_kwHOAoMIQ84Bdrqi --id "$item_id" \
     --field-id PVTSSF_lAHOAoMIQ84BdrqizhYLfZ4 --single-select-option-id 47fc9ee4
   ```
4. Create a branch, implement the change following the conventions in
   `README.md` (feature-based folders, pure domain logic in `src/lib/`, named
   exports, functional components).
5. Run/add tests as required by the task's acceptance criteria. Run lint/tests
   before opening the PR.
6. Open the PR referencing the issue so it auto-closes on merge:
   ```bash
   gh pr create --repo diogogcunha/tictactoe --title "<task title>" \
     --body "Closes #<task>

   <summary of the change>"
   ```
7. Report the PR URL and tell the user to run `/gh-qa #<task>` next. Do **not**
   set the project item to `Done` yourself — that decision belongs to the
   Quality Engineer after verification.
