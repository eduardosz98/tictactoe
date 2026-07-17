# Project Context — Tic Tac Toe Web Game

## Project Overview
A classic Tic Tac Toe (Noughts and Crosses) game with a modern web interface. Two players take turns marking spaces on a 3×3 grid, aiming to get three in a row. The project supports both local 2-player mode and single-player vs. an AI opponent.

## Tech Stack
- **Frontend:** React 18, TypeScript, Tailwind CSS, Vite
- **State Management:** React Context + useReducer
- **AI Logic:** Minimax algorithm with alpha-beta pruning (pure TypeScript, no external AI libs)
- **Testing:** Vitest + React Testing Library
- **Linting:** ESLint + Prettier

## Architecture
- **Single-page application** — no backend required for the core game
- **src/features/** — feature-based folder structure:
  - `board/` — game grid, cell components, win detection logic
  - `game/` — game state machine, turn management, scoring
  - `ai/` — minimax engine, difficulty levels, AI move selection
  - `ui/` — shared UI components (buttons, modals, scoreboard)
  - `settings/` — game configuration (board size, player names, AI difficulty)
- **No router needed** — single view with modal overlays for menus and results

## Coding Conventions
- Feature-based folder structure under `src/features/`
- React functional components with hooks, no class components
- Named exports for all components and utilities
- Pure game logic is separated from React components (domain model in `src/lib/`)
- CSS uses Tailwind utility classes; custom styles only for animations
- AI engine is a pure function: `getBestMove(board, player) => Move`
- Constants and types in `src/lib/constants.ts` and `src/lib/types.ts`

## Current Sprint / Milestone
Building the first playable version with:
- Interactive 3×3 game board with click-to-move
- Local 2-player mode (hot-seat)
- AI opponent with 3 difficulty levels (Easy, Medium, Unbeatable)
- Score tracking across rounds
- Win/draw detection with animated highlights
- Responsive layout (mobile + desktop)

## Future Considerations
- Online multiplayer via WebSockets (low priority)
- 4×4 and 5×5 board variants
- Move history with undo/redo
- Replay saved games
- Sound effects and animations

## Key Contacts
- **Product Owner:** Diogo
- **Tech Lead:** Diogo
- **Designer:** Diogo
