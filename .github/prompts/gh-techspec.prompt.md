---
agent: product-engineer
description: Append a Technical Specification section to an existing [PRD] GitHub issue.
---
# /gh-techspec

Read `.github/GH_WORKFLOW.md` first. Requires a PRD issue number as input
(e.g. `/gh-techspec #16`). If no number is given, ask for one. Run `/gh-design`
first if the issue has no `## Design` section yet — the tech spec must
reconcile with it.

## Goal
Add a `## Technical Specification` section to the PRD issue's body, in place.
No new issue, no local file.

## Steps
1. Fetch the PRD: `gh issue view <n> --repo diogogcunha/tictactoe --json title,body`
2. Explore the current codebase structure (`src/features/*`, `src/lib/*`) so the
   spec follows existing conventions from `README.md` (feature-based folders,
   pure domain logic in `src/lib/`, named exports, etc.).
3. Draft the spec with sections: `## Architecture`, `## Files to add/change`,
   `## Data model / types`, `## Edge cases`, `## Testing plan`, `## Risks`.
4. Append it to the existing body (don't remove the PRD content) and retitle
   to keep the `[PRD]` prefix (tech spec lives inside the same PRD issue):
   ```bash
   gh issue edit <n> --repo diogogcunha/tictactoe \
     --body-file <(printf '%s' "<original body>\n\n<new tech spec section>")
   ```
5. Report the updated issue URL and tell the user to run `/gh-tasks #<n>` next.
