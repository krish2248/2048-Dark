# 2048 Dark Edition

A sleek, dark-themed implementation of the classic **2048** puzzle game — built entirely in a single HTML file with zero dependencies (no build tools, no frameworks).

## **[Play Now](https://krish2248.github.io/2048-Dark/)**

---

## Preview

| Dark Mode | Light Mode |
|-----------|------------|
| Deep black/green palette with glow effects | Clean, paper-white aesthetic |

---

## Features

### Core Gameplay
- **Classic 2048 mechanics** — slide tiles in four directions, merge matching numbers, reach the 2048 tile to win
- **Smooth tile animations** — spawn (scale-in), slide (CSS transitions), and merge (pop) animations
- **Pixel-perfect grid** — tile positions are calculated from actual DOM measurements, so the layout works at any size
- **Undo system** — up to 5 moves of undo history (press `Z` or tap the Undo button)
- **Hint system** — 3 hints per game that suggest the best direction based on potential merges
- **Continue playing** — after reaching 2048, keep going to build even higher tiles

### Controls
| Input | Action |
|-------|--------|
| Arrow keys / WASD | Move tiles |
| Mouse drag / Touch swipe | Move tiles |
| Z | Undo last move |

### Stats & Achievements
- **11 tracked stats** — best score, highest tile, games played/won/lost, win rate, time played, current/longest streak, fastest win, best score time
- **13 achievements** — from "First Move" to "Master" (reach 2048), including speed and streak challenges
- **PDF report export** — download a styled dark-themed PDF of your full stats and achievement progress via jsPDF

### Achievements List
| Achievement | Condition |
|-------------|-----------|
| First Move | Play 1 game |
| Centurion | Score ≥ 100 |
| High Roller | Score ≥ 1,000 |
| Legend | Score ≥ 10,000 |
| First Win | Win 1 game |
| Hat Trick | Win 3 games |
| Speed Demon | Win under 5 minutes |
| Grinder | 1+ hour total play time |
| On Fire | 3-day play streak |
| Devoted | 7-day play streak |
| Half Way | Reach 512 tile |
| Tile Hunter | Reach 1024 tile |
| Master | Reach 2048 tile |

### Theming
- **Dark mode** (default) — black background (`#0D0D0D`) with green accent (`#4CAF70`) and glow effects
- **Light mode** — cream/white palette, toggled via the sun/moon icon
- Theme preference is saved to `localStorage` and persists across sessions

### Persistence
All game data is stored in `localStorage`:
- **Game save** — board state, score, and elapsed time are auto-saved so you can close the tab and resume later via "Continue Game"
- **Stats** — all statistics and unlocked achievements persist across sessions
- **Streak tracking** — daily play streaks are tracked with automatic date detection

---

## Project Structure

```
2048-Dark/
├── index.html    # The entire application — HTML + CSS + JS in one file
└── README.md     # This file
```

### Architecture (inside `index.html`)

The entire app lives in a single file, organized into clear sections:

#### HTML (Lines 1–698)
- **Three screens** managed by `.screen.active` class toggling:
  - `#home-screen` — title, best score display, menu buttons (Continue / New Game / Stats / Share)
  - `#game-screen` — score bar, 4×4 grid, action buttons, bottom navigation (Undo / Hint / Stats / Share)
  - `#stats-screen` — performance stats grid, achievements grid, PDF export, reset button
- **Three overlay dialogs**: Win, Game Over, and Reset Confirmation
- **Toast notification** element for transient messages

#### CSS (Lines 9–522)
- **CSS custom properties** for theming — all colors defined in `:root` (dark) and `body.light` (light), enabling instant theme switching with a single class toggle
- **Grid layout** — the game board uses `display: grid` with `grid-template-columns: repeat(4, 1fr)` for the background cells
- **Tile overlay** — tiles are absolutely positioned inside a `.tile-overlay` div that sits over the grid, allowing smooth CSS transition movement
- **Animations** — `@keyframes tileIn` (spawn scale), `@keyframes tilePop` (merge bounce), `@keyframes bump` (score update)
- **Responsive design** — percentage-based widths with `max-width` constraints, works on desktop and mobile

#### JavaScript (Lines 700–1393)

| Section | What it does |
|---------|-------------|
| **Config** (700–728) | Tile color map (green gradient from dark to bright), achievement definitions with condition functions |
| **Theme** (744–755) | Toggle dark/light mode, update SVG icons, persist preference |
| **Game State** (760–767) | Board array (16 cells), score, undo stack, hint counter, timer |
| **Tile Rendering** (776–852) | DOM-based tile management — `getCellRect()` calculates pixel positions from actual grid dimensions, `createTileEl`/`moveTileEl`/`updateTileEl`/`removeTileEl` manage individual tile elements |
| **Game Logic** (857–1058) | `startGame()` initializes or loads a save, `move()` handles sliding + merging with tile ID tracking, `spawnRender()` adds random 2/4 tiles, `doUndo()` restores previous state, `doHint()` evaluates all directions |
| **Timer** (1063–1071) | Tracks elapsed play time, pauses on tab hide, resumes on tab focus |
| **Save System** (1076–1081) | Auto-saves board/score/time to localStorage, cleared on game end |
| **Stats Engine** (1086–1128) | Load/save stats, daily streak calculation, achievement checking with toast notifications |
| **Win/Lose** (1134–1154) | Updates stats, checks achievements, shows overlay dialogs |
| **PDF Generation** (1205–1277) | Builds a styled A4 report using jsPDF with dark background, green accents, stat cards, and achievement badges |
| **Input Handling** (1293–1324) | Mouse drag, touch swipe, and keyboard (arrows + WASD + Z for undo) |
| **Event Wiring** (1328–1386) | All button click handlers, visibility change (pause/resume), window resize (re-render tiles) |

---

## Tech Stack

- **HTML5** — semantic structure, no framework
- **CSS3** — custom properties, grid, flexbox, animations, backdrop-filter
- **Vanilla JavaScript** — no libraries for game logic
- **[jsPDF](https://github.com/parallax/jsPDF)** — loaded via CDN for PDF report generation
- **[Google Fonts](https://fonts.google.com/)** — Space Mono (monospace, for numbers) + Outfit (UI text)

---

## How It Works

1. The game board is a flat array of 16 integers (0 = empty)
2. Each tile on screen is a DOM element tracked by a unique ID in a `tiles` object
3. On each move, the `move()` function processes 4 lines (rows or columns depending on direction), compacts non-zero values, merges adjacent pairs, and updates both the data array and the DOM
4. Tile positions are calculated from actual grid dimensions via `getCellRect()`, so they align perfectly regardless of screen size
5. After each move, a new tile spawns (90% chance of 2, 10% chance of 4)
6. The game checks for win (any tile ≥ 2048) and loss (no empty cells + no adjacent matches)

---

## Local Development

No build step needed. Just open the file:

```bash
# Clone the repo
git clone https://github.com/krish2248/2048-Dark.git
cd 2048-Dark

# Open in browser
start index.html        # Windows
open index.html         # macOS
xdg-open index.html     # Linux
```

Or use any local server:

```bash
npx serve .
# or
python -m http.server 8000
```

---

## License

MIT — do whatever you want with it.

---

Built with vanilla HTML, CSS, and JS. No frameworks. No build tools. Just vibes.
