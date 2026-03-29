# Java Code Visualizer

Interactive 3D visualizations that teach Java programming concepts to beginners. Instead of textbook explanations, each visualization **shows** how the code executes step by step — making abstract concepts tangible.

> Built for visual learners. No math required.

## Quick Start

No install, no build step. Just open an HTML file in your browser.

```
git clone https://github.com/sys010101/on2_visualization.git
```

Then open any `.html` file in Chrome, Firefox, Edge, or Safari.

## Visualizations

### O(N²) Loop Visualizer — `on2_visualization.html`

A 3D grid of cubes that light up one by one as nested `for` loops execute, making the "N times N" pattern click visually.

**What you'll learn:** Why nested loops are O(N²), what that looks like at different scales, and how it compares to O(N) and O(N³).

**Features:**
- Adjustable grid size (N = 3–14) and animation speed
- Live code editor — change loop bounds and watch the grid adapt
- Live debugger — see variable values, loop counters, and condition checks in real-time
- Step-through mode for one operation at a time
- Heat map, traversal toggle (row-major vs column-major)
- Comparison panel: O(N) vs O(N²) vs O(N³) with "what if N = 10,000?" projections
- Sound effects and completion celebration

---

### Username Validator Visualizer — `username_validator_vis.html`

3D character cubes representing a username, scanned by three validation rules. Watch `do-while`, `continue`, and `break` in action.

**What you'll learn:** How a do-while loop repeats until valid input is given, how `continue` skips remaining checks, and how `break` exits a for loop early.

**Features:**
- Type any username — 3D cubes rebuild live as you type
- Three validation rules animate step by step:
  - **Rule 1:** Length check (6–12 characters)
  - **Rule 2:** No spaces (scans each character with `break` on match)
  - **Rule 3:** At least one digit (scans each character with `break` on match)
- On failure, the do-while loops back and waits for new input — just like the real program
- Preset examples that demonstrate each failure path
- Concept badge explains the current keyword (do-while, continue, break)
- Resizable code panel

## Controls

All visualizations share these controls:

| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Step forward |
| `R` | Reset |
| `M` | Mute / Unmute |
| `Left/Right` | Change speed |
| `?` | Show keyboard shortcuts |

Plus visualization-specific shortcuts (see `?` overlay in each).

## Tech Stack

- **Single HTML files** — no build tools, no dependencies to install
- **Three.js r128** (CDN) — 3D rendering, particles, lighting
- **Web Audio API** — procedural sound effects
- **Vanilla JS** — no frameworks

## Who Is This For?

Originally built as a learning aid for QA Automation Engineers and students studying basic Java programming concepts. Great for anyone who is a visual learner and wants to understand code flow without wading through theory.

## License

Open source. Use it to learn, teach, or build on top of.
