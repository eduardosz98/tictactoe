---
agent: quality-engineer
description: Verify a [Task] issue's PR against its acceptance criteria and design spec, then approve and mark it Done.
---
# /gh-qa

Read `.github/GH_WORKFLOW.md` first. Requires a task issue number as input
(e.g. `/gh-qa #21`). If no number is given, ask for one.

## Goal
Independently verify the task's implementation before it can be considered
Done. Never take the Product Engineer's word for it — run things yourself.

## Steps
1. Fetch the task and its linked PR:
   ```bash
   gh issue view <task> --repo diogogcunha/tictactoe --json title,body,number
   gh pr list --repo diogogcunha/tictactoe --search "<task> in:body" --json number,url,title
   ```
2. Extract `Parent: #<epic>` from the task body and fetch the epic issue for
   the PRD's `## Acceptance criteria` and the `## Design` section (states,
   responsive behavior) to check against.
3. Checkout the PR locally, run lint/tests/build:
   ```bash
   gh pr checkout <pr-number> --repo diogogcunha/tictactoe
   npm run lint && npm test && npm run build
   ```
4. Manually verify each acceptance criterion from the epic against the actual
   behavior. If a criterion isn't covered by an automated test, add a small,
   targeted test for it now.
5. Record the verdict as a PR review:
   - All criteria met and tests green → `gh pr review <pr-number> --approve --body "<checklist of verified criteria>"`
   - Otherwise → `gh pr review <pr-number> --request-changes --body "<numbered list of gaps>"`
6. Only on approval, move the task's project item to `Done`:
   ```bash
   item_id=$(gh project item-list 5 --owner eduardosz98 --format json \
     --jq '.items[] | select(.content.number=='"<task>"') | .id')
   gh project item-edit --project-id PVT_kwHOAoMIQ84Bdrqi --id "$item_id" \
     --field-id PVTSSF_lAHOAoMIQ84BdrqizhYLfZ4 --single-select-option-id 98236657
   ```
7. Report the verdict and, if approved, remind the user the PR is ready to
   merge.
