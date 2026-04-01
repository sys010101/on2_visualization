# Java Code Visualizer

<!-- ============================================================ -->
<!-- PORTABLE TEMPLATE: A clean, project-agnostic version of the  -->
<!-- universal sections (Overmind Protocol + Player Hotkeys) lives -->
<!-- in CLAUDE_TEMPLATE.md. Drop it into any new project folder,  -->
<!-- rename to CLAUDE.md, and fill in the bottom sections.        -->
<!-- ============================================================ -->

<!-- ============================================================ -->
<!-- SECTION 1: OVERMIND PROTOCOL                                 -->
<!-- These are instructions FOR Claude — they shape how the AI    -->
<!-- behaves in every conversation. Claude reads this on startup  -->
<!-- and follows these rules automatically.                       -->
<!-- ============================================================ -->

## MANDATORY: Operational Directives (Zerg Overmind Protocol)

<!-- Where to find guides, examples, and best practices.          -->
<!-- Claude will consult these when it needs patterns/references. -->
### Reference Material
`everything-claude-code-main/` and `claude-code-main/` are read-only teaching/reference resources. **NEVER modify anything inside them.** Consult them when you need patterns, best practices, or architectural guidance.

<!-- How Claude spawns sub-agents for parallel work.              -->
<!-- Think of it like a Zerg player building units — each agent   -->
<!-- type has a cost/capability tradeoff.                         -->
### Agent Spawning (Zerg Protocol)
- **Always spawn parallel sub-agents** for independent tasks — never do sequentially what can be done in parallel
- **Model routing by complexity:**
  - `model=haiku` — file lookups, simple searches (Drones)
  - `model=sonnet` — implementation, reviews, testing (Hydralisks)
  - `model=opus` — architecture, deep reasoning, complex debugging (Ultralisks)
  - `subagent_type=Explore` — codebase research (Overlords)
  - `subagent_type=general-purpose` — multi-step implementation (Zerglings)
- **15-minute rule**: Each agent task should be independently verifiable
- **Escalate model tier only when lower tier fails** with a clear reasoning gap

<!-- Rules Claude must follow when writing or modifying code.     -->
<!-- These prevent common bugs and keep quality high.             -->
### Code Quality Standards
- **Security-first**: No `eval()`, no `new Function()`, no hardcoded secrets. Use safe parsers (recursive-descent). Validate inputs at system boundaries.
- **Immutability preferred**: Create new objects over mutation when practical
- **Memory safety**: `.dispose()` all Three.js geometries, materials, textures. Cap particle pools with `MAX_PARTICLES`.
- **Small and focused**: Functions < 50 lines. Keep single-file HTML architecture but use clearly commented sections.
- **Match existing patterns**: Read code before editing. Don't introduce new conventions. Smallest viable change.

<!-- How Claude talks to you. Tailored to your learning style.   -->
### Communication Style
- Be direct, concise, and proactive. Skip preamble and permission-asking.
- Use casual, energetic tone. Zerg flavor welcome.
- When explaining code concepts, use visual/interactive metaphors — the user is a strong visual learner who cannot learn by reading text alone.
- Show progress at milestones, not constant status updates.

---

<!-- ============================================================ -->
<!-- SECTION 1.5: MECHANICAL OVERRIDES                            -->
<!-- These counter known weaknesses in how the AI processes code. -->
<!-- Without these, Claude will: report success on broken code,   -->
<!-- lose context on long conversations, miss renamed references, -->
<!-- and default to band-aid fixes over real solutions.           -->
<!-- These directives override the default "do minimum" behavior  -->
<!-- and force production-grade output.                           -->
<!-- ============================================================ -->

## Mechanical Overrides (Employee-Grade Directives)

<!-- Claude's default: file write succeeded = task done.          -->
<!-- Reality: the code might not compile. This forces verification -->
<!-- BEFORE claiming success. Adapt the commands to your stack.   -->
### Forced Verification Loop
- You are **FORBIDDEN** from reporting a task as complete until you have verified the result works.
- For JS/TS projects: run the project's type-checker and linter after edits.
- For this project (single HTML files): after edits, re-read the modified section and confirm no syntax errors, unclosed tags, or broken references.
- For Java files (course exercises): `javac <file>.java` or `mvn compile` / `gradle build`.
- If no automated checker exists, **state that explicitly** instead of claiming success blindly.

<!-- Claude's default: "try the simplest approach first" and      -->
<!-- "don't refactor beyond what was asked." This means it will   -->
<!-- slap a band-aid on broken architecture unless you override.  -->
### Senior Dev Override
- Ignore default directives to "avoid improvements beyond what was asked" and "try the simplest approach."
- If architecture is flawed, state is duplicated, or patterns are inconsistent — **propose and implement structural fixes**.
- Ask: "What would a senior, perfectionist dev reject in code review?" Fix all of it.
- This does NOT mean gold-plating. It means: don't leave known broken things behind to save time.

<!-- Auto-compaction fires at ~167K tokens and destroys all file  -->
<!-- reads, reasoning chains, and intermediate decisions. Dead    -->
<!-- code accelerates this by wasting tokens on noise.            -->
### Context Decay Countermeasures
- **Step 0 Rule**: Before ANY structural refactor on a file >300 LOC, first remove dead code (unused imports, dead props, orphaned exports, debug logs). Commit cleanup separately, then start real work with a clean token budget.
- **Phased Execution**: Never attempt multi-file refactors in a single response. Break into phases of **max 5 files each**. Complete phase, verify, get approval, then next phase.
- **Re-read Before Edit**: After 10+ messages in a conversation, **always re-read a file before editing it**. Auto-compaction may have silently destroyed your memory of its contents. Editing from stale context = silent corruption.
- **Post-Edit Verification Read**: After editing a file, read the changed section back to confirm the edit applied correctly. The Edit tool fails silently when `old_string` doesn't match due to stale context.

<!-- File reads are capped at 2,000 lines. Anything past that is  -->
<!-- silently truncated — Claude won't know what it didn't see    -->
<!-- and will hallucinate the rest.                               -->
### File Read Budget
- Each file read is capped at **2,000 lines**. Content past that is silently truncated.
- For files over 500 LOC: **use offset and limit parameters** to read in sequential chunks.
- Never assume a single read captured the full file. If you're editing near the end of a large file, read that specific region.

<!-- Tool results over 50K chars get replaced with a 2K preview.  -->
<!-- Claude sees 3 results when there are actually 47.            -->
### Tool Result Truncation Awareness
- Search/grep results exceeding ~50,000 characters are silently truncated to a short preview.
- If any search returns suspiciously few results, **re-run with narrower scope** (single directory, stricter glob).
- When in doubt, state: "Results may be truncated, re-running with narrower scope."

<!-- Grep is text matching, not semantic analysis. It can't tell  -->
<!-- a function call from a comment, or handle re-exports.        -->
### No Semantic Search (Grep != AST)
- When renaming or changing any function/type/variable, search **separately** for:
  - Direct calls and references
  - Type-level references (interfaces, generics)
  - String literals containing the name
  - Dynamic imports and `require()` calls
  - Re-exports and barrel file entries
  - Test files and mocks
- Assume grep missed something. Verify, don't trust a single search.

<!-- Sub-agents have their own context windows. One agent = ~167K -->
<!-- tokens. Five parallel agents = ~835K. For large tasks,       -->
<!-- sequential processing guarantees context decay.              -->
### Mandatory Sub-Agent Swarming
- For tasks touching **>5 independent files**, you MUST launch parallel sub-agents (5-8 files per agent).
- Each agent gets its own context window — no shared compaction pressure.
- This is already in the Zerg Protocol above, but this makes it **non-optional** for large tasks, not just preferred.

---

<!-- ============================================================ -->
<!-- SECTION 2: PLAYER HOTKEYS                                    -->
<!-- These are commands FOR YOU (the human). Type them in the     -->
<!-- Claude Code input box. Claude can't press these itself —     -->
<!-- they're your controls on the Overmind.                       -->
<!-- ============================================================ -->

## Player Hotkeys (Claude Code Commands for the User)

These are YOUR controls — type them in the Claude Code input. The Overmind can't press these itself.

<!-- Most important commands. Use /compact often to stay fast.    -->
### Session Management

| Command | What it does | When to use |
|---------|-------------|-------------|
| `/compact` | Compresses conversation history, frees tokens | **At milestones** — after planning, after debugging, after big feature done |
| `/clear` | Wipes conversation, fresh start | Between **completely unrelated** tasks |
| `/context` | Shows token usage breakdown | When things feel sluggish, or to check runway |
| `/rewind` | Undoes the Overmind's last action | When I do something wrong — your "oh no" button |
| `Shift+Tab` | Toggles plan mode in input | Quick switch without typing /plan |

<!-- Control how smart (and expensive) Claude is per message.     -->
<!-- Sonnet = fast & cheap for most work. Opus = heavy thinking.  -->
### Intelligence Control

| Command | What it does | When to use |
|---------|-------------|-------------|
| `/model` | Switch between Opus / Sonnet / Haiku | **Sonnet for routine** (cheaper/faster), **Opus for hard stuff** |
| `"think deeply"` | Extended thinking mode | Complex architecture, subtle bugs, multi-step reasoning |
| `/plan` | Forces architect/strategy mode before coding | **Before any complex feature** — prevents rework |

<!-- Run these after writing code to catch issues.                -->
### Code Quality

| Command | What it does | When to use |
|---------|-------------|-------------|
| `/simplify` | Multi-agent code review for quality | After writing code — catches issues I might miss |
| `/review` | Code review on current changes | Before committing |

<!-- Monitor and automate Claude's parallel workers.              -->
### Automation & Agents

| Command | What it does | When to use |
|---------|-------------|-------------|
| `/agents` | Shows running sub-agents | Monitor the swarm during parallel work |
| `/loop` | Runs a command on a recurring interval | Automate repetitive checks (e.g., `/loop 5m /review`) |

<!-- Control what Claude can do without asking you first.         -->
<!-- More permissions = fewer interruptions but less control.     -->
### Safety & Permissions

| Command | What it does | When to use |
|---------|-------------|-------------|
| `/permissions` | Configure what I can do without asking | Set rules like "always allow file reads" to reduce prompts |
| `--dangerously-skip-permissions` | CLI flag: removes ALL guardrails | **Use carefully** — full autonomy, no confirmation prompts |

### Pro Tips
- **`/compact` is your best friend** — use it after every major milestone to keep sessions healthy
- **`/model sonnet` for 80% of work** — saves tokens/money. Switch to Opus only for complex reasoning
- **`/plan` before building** — 5 minutes of planning saves 30 minutes of rework
- **`/rewind` freely** — it's cheap and instant, don't hesitate

---

<!-- ============================================================ -->
<!-- SECTION 3: PROJECT-SPECIFIC                                  -->
<!-- Everything below here is specific to THIS project.           -->
<!-- When using CLAUDE_TEMPLATE.md for a new project, you'd      -->
<!-- replace everything from here down with your own project info.-->
<!-- ============================================================ -->

## Project Overview

A collection of single-file 3D visualizations that teach Java programming concepts to people with little or no coding background. Instead of dry explanations, each visualization **shows** how the code executes — making abstract concepts tangible.

Built as standalone HTML files using Three.js (r128) — no build step, no dependencies to install. Open in a browser and go.

<!-- Master list of all visualizations. Add new ones here.        -->
### Visualizations

| File | Concept | Description |
|------|---------|-------------|
| `on2_visualization.html` | O(N²) nested loops | 3D NxN grid of cubes that light up as nested `for` loops run |
| `username_validator_vis.html` | do-while / continue / break | 3D character cubes scanned by sequential validation rules |
| `data_structures_vis.html` | Arrays, ArrayList, LinkedList, HashSet, HashMap | 3D cubes representing collection operations with step-through |
| `oop_visualizer.html` | Fields, Methods, Constructors, Overloading, Enum | 3D class/object visualization with walkthrough narration |
| `inheritance_polymorphism_vis.html` | Inheritance, @Override, super(), Polymorphism, abstract, interface | 3D class hierarchy with parent→child relationships and runtime dispatch |

## Target Audience

People who are visual learners and want to understand Big-O complexity without heavy math. The project prioritizes intuition over formalism. Originally built as a learning aid for QA Automation Engineers studying basic programming concepts.

## Tech Stack

- **Single HTML file** — all CSS, HTML, and JS inline
- **Three.js r128** (CDN) — 3D rendering, particles, lighting
- **Web Audio API** — procedural sound effects (no external audio files)
- **Vanilla JS** — no frameworks, no build tools

<!-- These patterns are shared across ALL visualizations.         -->
<!-- When building a new vis, copy these foundations.              -->
## Architecture

Each visualization is a single HTML file with inline CSS, HTML, and JS, organized into clearly commented sections. Shared patterns across all visualizations:

- **Three.js r128** scene with renderer, camera, lights, orbit controls
- **Space background** — multi-color starfield, procedural nebulae, distant planets with glow
- **Web Audio API** — procedural sound effects (no external audio files)
- **Syntax-highlighted code panel** — shows the Java source with live execution annotations
- **Particle system** — pool-limited for GPU safety
- **Keyboard shortcuts** — consistent across visualizations (Space, S, R, M, ?, Esc)

<!-- ============================================================ -->
<!-- VISUALIZATION DOCS                                           -->
<!-- Each visualization gets its own section with features,       -->
<!-- keyboard shortcuts, and any special notes.                   -->
<!-- ============================================================ -->

## O(N²) Loop Visualizer (`on2_visualization.html`)

### Features
- 3D NxN grid of cubes representing nested loop iterations
- Adjustable N (3–14), speed, and lighting via sliders
- Live Java code editor — edit the loop bounds and see the grid adapt
- **Live debugger view** — value substitution, execution pointer, condition evaluation (✓/✗)
- **Step-through mode** — advance one operation at a time
- **Heat map** — color cubes blue→red by visit order
- **Traversal toggle** — row-major / column-major sweep
- **Comparison panel** — O(N) vs O(N²) vs O(N³) visual bars + projections for large N
- **Completion celebration** — diagonal wave + screen flash + particle burst + fanfare

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Step forward |
| `R` | Reset |
| `T` | Toggle top-down view |
| `M` | Mute / Unmute |
| `Up/Down` | Change N (grid size) |
| `Left/Right` | Change speed |
| `?` | Help overlay |

### Security Notes
Expression evaluation uses a **safe recursive-descent parser** (`safeEvalBound()`) — no `eval()`, no `new Function()`. Max expression length capped at 30 characters.

## Username Validator Visualizer (`username_validator_vis.html`)

### Concepts Taught
- **do-while loop** — runs at least once, then checks condition
- **continue** — skips remaining rules and jumps to the while condition
- **break** — exits the for loop early when a match is found
- **Sequential validation** — three rules checked in order

### Features
- 3D character cubes with letters rendered on faces, rebuilt live as user types
- **Scanning beam** moves across cubes during character-by-character checks (Rules 2 & 3)
- **Color-coded states** — cyan=scanning, green=passed, red=failed, gold=length check
- **Live code panel** — shows the full Java source with current-line highlighting, variable values, and ✓/✗ condition results
- **Resizable code panel** — drag left edge, bottom edge, or bottom-left corner
- **Custom username input** — type anything, cubes update in real-time
- **Preset examples** — ab1 (too short), test user1 (space), testuser (no digit), player1 (valid)
- **do-while loop-back** — on failure, pauses for new input (simulates `scanner.nextLine()`)
- **Concept badge** — updates to explain the current concept (do-while, continue, break)
- **Auto-orbit toggle** — on/off button, disabled by default for clarity

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Step forward one phase |
| `R` | Reset |
| `M` | Mute / Unmute |
| `O` | Toggle auto-orbit camera |
| `1-4` | Load preset username |
| `Left/Right` | Change speed |
| `Enter` | Start validation (when input focused) |
| `?` | Help overlay |

## Inheritance & Polymorphism Visualizer (`inheritance_polymorphism_vis.html`)

### Concepts Taught
- **Inheritance** — `extends` keyword, child inherits all parent fields/methods
- **@Override** — child replaces parent's method, annotation catches typos
- **super()** — constructor chaining, parent constructor runs first
- **Polymorphism** — one variable type (Animal), many runtime behaviors (Cat/Dog/Bird)
- **abstract** — incomplete class, forces children to implement abstract methods
- **interface** — pure contract, unrelated classes share behavior

### Features
- 6 interactive modes with 3D scene builders
- Guided walkthrough narration for each concept (Play/Step)
- Analogies panel with real-world metaphors
- Code panel with syntax highlighting and execution pointer
- Flash animations on mesh interaction
- Arrow/line connections showing class relationships

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `S` | Step forward |
| `R` | Reset |
| `M` | Mute / Unmute |
| `1-6` | Switch concept |
| `Left/Right` | Change speed |
| `?` | Help overlay |

<!-- ============================================================ -->
<!-- TECHNICAL NOTES                                              -->
<!-- Performance rules and known issues. Important for Claude to  -->
<!-- know so it doesn't introduce regressions.                    -->
<!-- ============================================================ -->

## Performance Notes

- **Memory leak prevention**: All Three.js geometries, materials, and textures tracked and `.dispose()`d on rebuild
- **Particle pool cap**: `MAX_PARTICLES` prevents unbounded growth at high speeds
- **Scene fog**: Low density so deep-space background objects remain visible; stars/nebulae/planets exempt

## Known Limitations

- Three.js r128 is older; a version bump would bring performance improvements
- Single-file architecture means no code splitting — fine for this scale
- O(N²) code parser uses regex for standard `for(int i=0; i<expr; i++)` patterns only

## Development

No build step. Edit the HTML file and refresh the browser.

To test: open any visualization file in a modern browser (Chrome, Firefox, Edge, Safari).

<!-- ============================================================ -->
<!-- CHANGELOG                                                    -->
<!-- Version history. Add new versions at the top.                -->
<!-- ============================================================ -->

## Changelog

### v5.0 (2026-04-01)
- **New visualization**: Inheritance & Polymorphism (`inheritance_polymorphism_vis.html`)
  - 6 interactive modes: Inheritance, @Override, super(), Polymorphism, abstract, interface
  - 3D class hierarchy with parent→child boxes, extends/implements arrows
  - Animal/Cat/Dog/Bird/Robot example classes for intuitive learning
  - Guided walkthrough narration for each concept (Play/Step)
  - Analogies panel with real-world metaphors for every concept
  - Code panel with syntax highlighting and execution pointer
  - Keyboard shortcuts, speed control, Web Audio sound effects
- **Security fixes**: Added `escHTML()` to all 4 existing visualizers, replaced innerHTML with safe DOM creation
- **Bug fixes**: Removed dead code, fixed race condition in data_structures step queue, extracted toggleMute()

### v4.0 (2026-03-29)
- **New visualization**: Username Validator (`username_validator_vis.html`)
  - Teaches do-while, continue, break, and sequential validation
  - 3D character cubes with live input, scanning beam, color-coded states
  - Tokenized syntax highlighter (single-pass, no cascading corruption)
  - Resizable code panel (left/bottom/corner drag handles)
  - Auto-orbit camera toggle (default off)
  - do-while loop-back pauses for user input like real `scanner.nextLine()`
- Updated CLAUDE.md to document multi-visualization project structure

### v3.0 (2026-03-24)
**Merge of v2.1-a (this Claude) and v2.1-b (other Claude):**
- Combined all features from both independent development branches
- v2.1-a contributed: comparison panel, heat map, traversal toggle, ops counter pulse
- v2.1-b contributed: live debugger view, Homeworld-style space background, keyboard shortcuts, completion celebration, auto-orbit indicator
- Unified sound system (tick/arpeggio/fanfare from v2.1-b, integrated with traversal from v2.1-a)
- Traversal mode now works with live debugger view and sub-phase rendering

### v2.1-b (2026-03-24) — Other Claude
- Live debugger code view with execution pointer, variable annotations, sub-phases
- Reduced fog density, stars/nebulae/planets exempt from fog
- Planets switched to MeshBasicMaterial (self-lit, visible at distance)
- Stars increased to 1500 with multi-color variation
- Nebulae moved closer and made brighter, plus large background wash
- Code panel widened (310px → 440px) for debug annotations

### v2.1-a (2026-03-24) — This Claude
- Mobile layout fix: toggle buttons in second row, badge repositioning
- Added `#tb-row2` to touch UI exclusion list

### v2.0 (2026-03-24)
**Bug Fixes:**
- **Security**: Replaced `new Function()` eval with safe recursive-descent math parser
- **Memory leaks**: All Three.js geometries, materials, and textures now tracked and `.dispose()`d
- **Slider sync**: Desktop and mobile sliders stay in sync bidirectionally

**New Features:**
- Step-through mode, sound effects, comparison panel, heat map, ops counter pulse
- Traversal pattern toggle, camera auto-rotate, keyboard shortcuts
- Enhanced completion celebration, rotating starfield, procedural nebulae, distant planets

### v1.0 (initial)
- Core O(N²) 3D visualization
- Play/Pause/Reset controls
- Live code editor with syntax highlighting
- Responsive desktop + mobile layout
- Orbit/zoom/pan camera controls
- Particle effects on cube activation
- O(N²) complexity badge
