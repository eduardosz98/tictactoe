---
mode: design-engineer
description: Turn a [PRD] GitHub issue into a UI/UX design spec plus a real HTML/CSS prototype, using Claude for design reasoning.
---
# /gh-design

Read `.github/GH_WORKFLOW.md` first. Requires a PRD issue number as input
(e.g. `/gh-design #16`). If no number is given, ask for one.

## Goal
Add a `## Design` section to the PRD issue's body (in place, no new issue) and
produce a reviewable static prototype file. This runs on Claude specifically
for design reasoning — do not switch models mid-task.

## Steps
1. Fetch the PRD: `gh issue view <n> --repo diogogcunha/tictactoe --json title,body`
2. Re-read the `## User stories` and `## Acceptance criteria` sections; map
   each UI-relevant one to a concrete screen state.
3. Design the following, grounded in the existing `src/features/*` structure
   described in `README.md`:
   - Layout/grid and spacing (Tailwind scale).
   - Component breakdown, one bullet per component, mapped to a
     `src/features/<name>/` folder it will eventually live in.
   - All states: idle, in-progress, win (with highlight), draw, AI-thinking
     (if relevant to the PRD), and both difficulty/mode variants if relevant.
   - Responsive behavior: mobile vs desktop breakpoints.
4. Build a standalone prototype at `design/<n>-<slug>.html`: plain HTML using
   `<script src="https://cdn.tailwindcss.com"></script>`, static markup for
   each state above (no real interactivity required — visual reference only).
5. Write the `## Design` section into the PRD issue body containing: a short
   rationale per decision, a Mermaid component-tree diagram, and a link/path
   to the prototype file (`design/<n>-<slug>.html`).
   ```bash
   gh issue edit <n> --repo diogogcunha/tictactoe \
     --body-file <(printf '%s' "<original body>\n\n<new design section>")
   ```
6. Commit the prototype file locally (`git add design/ && git commit`). If you
   lack push access to the repo, say so explicitly and leave the commit
   staged/local rather than silently skipping it.
7. Report the issue URL and prototype path. Tell the user to run
   `/gh-techspec #<n>` next.
