# Java Code Visualizer

Interactive HTML visualizers that teach Java and QA automation concepts through 3D scenes, motion, color, and step-by-step code flow.

Built for visual learners. No build step required.

## Quick Start

Clone the repo:

```bash
git clone https://github.com/sys010101/on2_visualization.git
```

Then open any of the main HTML files in Chrome, Firefox, Edge, or Safari.

Note:
- Most root visualizers are standalone Three.js pages.
- `QA automation courses/qa-html-export-v6.html` is a separate course dashboard page.
- Some pages load CDN scripts, so browser network access helps for full local rendering.

## Main Visualizers

### `on2_visualization.html`

Teaches nested loops and `O(N^2)` complexity with a 3D grid, live debugger cues, traversal modes, and comparison views.

What you'll learn:
- why nested loops scale the way they do
- how loop counters and bounds affect execution
- how `O(N)`, `O(N^2)`, and `O(N^3)` feel visually

### `username_validator_vis.html`

Teaches `do-while`, `for`, `continue`, and `break` through a username validation flow with animated character scanning and rule-by-rule feedback.

What you'll learn:
- how a `do-while` loop repeats until valid input is accepted
- how `break` stops a scan early
- how `continue` skips the rest of the current validation path

### `data_structures_vis.html`

Teaches arrays, `ArrayList`, `LinkedList`, `HashSet`, and `HashMap` with visual collection operations and practical Big O intuition.

What you'll learn:
- when different data structures are useful
- how inserts, lookups, and uniqueness behave
- why data structure choice changes runtime behavior

### `oop_visualizer.html`

Teaches OOP basics such as fields, methods, constructors, access control, `static`, `final`, and enums.

What you'll learn:
- how objects combine state and behavior
- how constructors initialize valid state
- how access modifiers and shared class members work

### `inheritance_polymorphism_vis.html`

Teaches inheritance, `super()`, `@Override`, abstract classes, interfaces, and polymorphism using class hierarchy visuals and guided walkthroughs.

What you'll learn:
- how parent-child relationships work in Java
- how overridden behavior changes runtime execution
- how abstract classes and interfaces differ

### `oop_mental_map_vis.html`

Teaches the harder advanced-OOP material through shape-first analogies: vault, dashboard, blueprint, contract, family tree, and one-button-many-results.

What you'll learn:
- encapsulation as protection
- abstraction as a clean control surface
- abstract class vs interface as blueprint vs contract
- inheritance and polymorphism as relationships and runtime behavior

## Course Dashboard

### `QA automation courses/qa-html-export-v6.html`

This is the current session-and-homework dashboard export.

It includes:
- sessions 1 through 10
- homework summaries aligned to the course DOCX files
- integrated review checklists for the homework tabs
- links out to the relevant root visualizers

Important note:
- The source lesson files in `QA automation courses/` are still in Romanian.
- The dashboard itself is the English-facing summary layer.

## Shared Controls

Most root visualizers share a common keyboard pattern:

| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Step forward |
| `R` | Reset |
| `M` | Mute / Unmute |
| `Left/Right` | Change speed |
| `?` | Show help / shortcuts |

Some pages add extra controls such as number keys, top-down view toggles, or concept-mode switching.

## Tech Stack

- Single HTML files with inline HTML, CSS, and JavaScript
- Three.js r128 from CDN for the root 3D visualizers
- Web Audio API on some pages for procedural sound
- Vanilla JS for the root visualizers
- React 18 UMD + ReactDOM UMD + Babel standalone for `QA automation courses/qa-html-export-v6.html`

## Who Is This For?

This repo was built as a learning aid for QA automation students and other visual learners studying beginner-to-intermediate Java concepts.

It is especially aimed at people who understand faster through:
- shapes
- motion
- color
- real-world analogies
- visible cause-and-effect

## Development

No install and no build step for the main HTML files.

Typical workflow:
- edit an HTML file
- refresh it in the browser
- smoke test the changed page

## License

Open source. Use it to learn, teach, or build on top of.
