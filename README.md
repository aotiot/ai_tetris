
# 🟦 Tetris — Project Documentation

---

## Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [Controls](#controls)
4. [How to Use](#how-to-use)
5. [Technical Architecture](#technical-architecture)
6. [Game Mechanics](#game-mechanics)
7. [File Structure](#file-structure)
8. [Scoring System](#scoring-system)
9. [Customization](#customization)

---

## Overview

**Tetris** is a complete, single-file web-based Tetris game built entirely in HTML, CSS, and JavaScript. No
external libraries, frameworks, or build tools are required.

- **Single File**: Everything is contained in one `tetris.html` file
- **Zero Dependencies**: Pure Vanilla JavaScript
- **Responsive**: Works on desktop and mobile
- **Modern Polish**: SRS rotation, lock delay, ghost piece, line animations

---

## Features

| Feature | Description |
|---|---|
| **All 7 Tetrominoes** | I, O, T, S, Z, J, L with proper shapes |
| **SRS Rotation** | Standard Rotation System with wall kicks |
| **Ghost Piece** | Transparent preview of landing position |
| **Next Piece** | Preview of the upcoming piece |
| **Line Clear Animation** | Flashing animation on row completion |
| **Score & Leveling** | Dynamic speed based on level |
| **Combo System** | Bonus points for consecutive clears |
| **Lock Delay** | Fair holding mechanic |
| **Pause / Resume** | `P` key to pause |
| **Game Over** | Screen with final stats and restart option |
| **Mobile Controls** | Touch buttons for mobile devices |
| **Dark Theme** | Modern dark UI with neon colors |
| **Responsive Layout** | Adapts to different screen sizes |

---

## Controls

### Keyboard (Desktop)

| Key | Action |
|-----|--------|
| `←` / `Right` | Move left / right |
| `↑` | Rotate clockwise |
| `↓` | Soft drop (move down) |
| `Space` | Hard drop |
| `P` | Pause / Resume |

### Alternative Keys

| Key | Action |
|-----|--------|
| `X` | Rotate clockwise (alternative to `↑`) |
| `Z` | Rotate counter-clockwise (alternative to `↓`) |

### Mobile Touch Controls

| Button | Action |
|--------|--------|
| ← | Move left |
| → | Move right |
| ↓ | Soft drop |
| ↻ | Rotate |
| ⤓ | Hard drop |

---

## How to Use

### Desktop

1. **Save** the HTML file:
   ```bash
   # Save as tetris.html
   ```

2. **Open** in any modern web browser:
   ```bash
   # Double-click the file, or
   # Navigate to: file://path/to/tetris.html
   ```

3. **Click** "Start Game" to begin playing.

### Mobile

1. Save the file to your device
2. Open in a mobile browser (Safari, Chrome, Firefox)
3. Use the touch controls at the bottom of the screen

### View in Source

- No build step required
- Just open the file in a browser

---

## Technical Architecture

### Project Structure

```
tetris/
├── tetris.html          # Single HTML file
│   ├── <head>
│   │   ├── <meta>       # Viewport, charset
│   │   └── <style>      # All CSS styling
│   ├── <body>
│   │   ├── #game-container
│   │   │   ├── #left-panel
│   │   │   │   ├── Score Panel
│   │   │   │   ├── Level Panel
│   │   │   │   ├── Lines Panel
│   │   │   │   ├── Combo Panel
│   │   │   │   └── Next Piece Panel
│   │   ├── #board-container
│   │   │   ├── #board (CSS Grid)
│   │   │   └── #overlay (Start / Pause / Game Over)
│   │   ├── #controls-info
│   │   └── .mobile-controls
│   └── <script>         # All JavaScript logic
```

### Game Loop

```
┌─────────────────────────────────────┐
│          gameLoop(timestamp)         │
│                                     │
│  1. Check pause state              │
│  2. Calculate delta time            │
│  3. If drop interval reached:       │
│     └─ dropPiece()                 │
│  4. renderBoard()                  │
│  5. renderGhost()                  │
│  6. updateUI()                     │
│  7. requestAnimationFrame()        │
└─────────────────────────────────────┘
```

### Key JavaScript Modules

| Module | Description |
|--------|-------------|
| **Constants** | Grid size, shapes, colors, scoring tables |
| **Game State** | Board, pieces, score, level, timer |
| **Piece Logic** | Spawn, rotate, move, lock, drop |
| **Collision** | Grid boundaries and occupied cells |
| **SRS System** | Rotation with wall kicks |
| **Line Clear** | Detection and animated removal |
| **Rendering** | DOM-based board drawing |
| **Input** | Keyboard and touch event handling |
| **UI Update** | Score, level, combo display |

---

## Game Mechanics

### Grid

- **10 columns** wide
- **20 rows** tall
- Each cell = **30px × 30px**

### Tetromino Definitions

| Piece | Shape | Color |
|-------|-------|-------|
| **I** | 4 blocks in a row | Cyan `#00ffff` |
| **O** | 2×2 square | Yellow `#ffff00` |
| **T** | T shape | Magenta `#aa00ff` |
| **S** | S shape | Green `#00ff00` |
| **Z** | Z shape | Red `#ff0000` |
| **J** | J shape | Blue `#0000ff` |
| **L** | L shape | Orange `#ffaa00` |

### Scoring System

| Lines Cleared | Base Points |
|---------------|-------------|
| 1 line | 40 |
| 2 lines | 100 |
| 3 lines | 300 |
| 4 lines (Tetris) | 1200 |

**Combo Bonus**: `combo × 50` points added for consecutive clears.

### Leveling

| Lines Cleared | Level Reached |
|---------------|---------------|
| 0–9 | Level 1 |
| 10+ | Level 2 |
| 30+ | Level 3 |
| 50+ | Level 4 |
| 70+ | Level 5 |

**Drop speed** decreases from 800ms (L1) down to 10ms (L14+).

### Lock Delay

- When a piece touches the ground, a **0.5s lock delay** begins
- The player can still move the piece during this time
- If the piece remains stationary for the full delay, it locks

---

## SRS Rotation System

The game uses **Super Rotation System (SRS)** for rotation with wall kicks:

- **Clockwise**: `↑` or `X`
- **Counter-clockwise**: `↓` or `Z`

Each piece type has specific **kick offsets** that are tried when a rotation would cause a collision. This ensures
fair and predictable rotation near walls and other pieces.

---

## Mobile Controls Layout

```
    [ ← ] [ ↓ ] [ → ]
        [ ↻ ] [ ⤓ ]
```

- Buttons are **55px × 55px**
- Active state turns **neon green**
- `touchstart` events prevent default behavior

---

## Customization

### Colors

Modify the `COLORS` object in the `<script>` section:

```javascript
const COLORS = {
  I: '#00ffff',      // Cyan
  O: '#ffff00',      // Yellow
  T: '#aa00ff',      // Magenta
  S: '#00ff00',      // Green
  Z: '#ff0000',      // Red
  J: '#0000ff',      // Blue
  L: '#ffaa00'       // Orange
};
```

### Line Scores

Modify the `LINE_POINTS` array:

```javascript
const LINE_POINTS = [0, 40, 100, 300, 1200];
```

### Level-Up Intervals

Modify the `LEVEL_UP_LINES` array:

```javascript
const LEVEL_UP_LINES = [0, 10, 30, 50, 70];
```

### Grid Size

Modify `COLS` and `ROWS`:

```javascript
const COLS = 10;
const ROWS = 20;
```

---

## Browser Compatibility

| Browser | Version | Status |
|---------|---------|--------|
| Chrome | 60+ | ✅ |
| Firefox | 60+ | ✅ |
| Safari | 14+ | ✅ |
| Edge | 60+ | ✅ |
| Mobile Safari | 14+ | ✅ |
| Mobile Chrome | 60+ | ✅ |

---

## Known Limitations

- **Single-player only** — no multiplayer support
- **No high score persistence** — scores reset on refresh
- **Desktop-only keyboard input** — touch controls are for mobile
- **No sound effects** — purely visual gameplay

---

## Development Notes

- **Canvas vs DOM**: DOM-based rendering is used for simplicity and ease of adding animations
- **No external assets**: All graphics are CSS-generated
- **ES6+**: Uses modern JavaScript features (arrow functions, template literals, destructuring)
- **No frameworks**: Pure vanilla HTML/CSS/JS

---

## License

This project is provided as-is. Feel free to modify, fork, and use freely.

---

*Built with 💻 in a single HTML file*
