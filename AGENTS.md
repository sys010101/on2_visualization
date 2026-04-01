# AGENTS.md

## Purpose

- This is the agent-facing field manual for this repo.
- `CLAUDE.md` is the fuller upstream reference. Mirror its useful rules here; do not edit `CLAUDE.md` unless the user explicitly asks.
- If `AGENTS.md` and `CLAUDE.md` differ, prefer the stricter rule unless the user gives a direct override.

## Operational Directives (Zerg Overmind Protocol)

### Reference Material

- Read `CLAUDE.md` before large refactors, repo-wide consistency passes, or architecture decisions.
- `README.md` is not the full project inventory. Do not treat it as authoritative by itself.
- The user's working files and current repo state outrank stale documentation.

### Agent Spawning (Zerg Protocol)

- Always parallelize independent work. Do not do sequentially what can be split safely.
- Keep each spawned task independently verifiable in 15 minutes or less.
- Give each agent one clear job, one scope, and one expected output.
- For work touching more than 5 independent files, swarm by default.
- Use scout agents first for discovery, references, grep passes, and inventory building.
- Use isolated worktrees for parallel implementation when agents need to edit in parallel.
- Do not poll background agents mid-run. Wait for their completion notification.
- Escalate agent tier only when a lower tier failed or the task is obviously high-risk.

### Zerg Unit Naming and Model Routing

- Name spawned agents by Zerg unit plus ordinal, for example: `Zergling-01`, `Hydralisk-02`, `Ultralisk-01`.
- Reserve `Overmind` for the parent coordinator.
- Reserve `Overlord` for scout or explorer roles.
- Use this routing table:
  - `Zergling` -> `gpt-5.4-mini`: quick lookups, grep, file inventory, doc comparison, note taking.
  - `Roach` -> `gpt-5.1-codex-mini`: narrow deterministic edits, repetitive cleanup, targeted validation, bounded transforms.
  - `Hydralisk` -> `gpt-5.3-codex`: implementation, testing, reviews, and medium-scope fixes.
  - `Mutalisk` -> `gpt-5.2`: cross-file consistency sweeps, UI checks, smoke-test coordination, and broader general tasks.
  - `Lurker` -> `gpt-5.2-codex`: hidden regression hunting, diff review, and edge-case analysis.
  - `Infestor` -> `gpt-5.1-codex-max`: architecture support, deep debugging, and rescue tasks after two failed attempts.
  - `Ultralisk` -> `gpt-5.4`: the hardest blockers, repo-wide design work, and high-risk refactors.
  - `Overlord` -> explorer/scout subagent type rather than a power tier; use for discovery and reference gathering.
- If tooling constraints force a fallback, keep the same unit label and note the actual model used in commentary.

### Code Quality Standards

- Security first. No `eval()`, no `new Function()`, no hardcoded secrets. Parse safely and validate inputs at boundaries.
- Prefer immutability when practical, but do not contort simple code just to avoid mutation.
- Keep changes small and pattern-matching. Read existing code before introducing a new convention.
- Keep functions focused. Avoid sprawling utility blobs inside single-file HTML pages.
- One source of truth. Do not duplicate state to patch over rendering bugs.
- Write human code. No robotic comments, no filler abstractions, no fake future-proofing.

### Forced Verification Loop

- Do not report success until the result has been checked.
- For single-file HTML work: re-read the changed section after the edit and check for syntax issues, broken references, and mismatched tags.
- Run the smallest real verification available: browser smoke test, DOM capture, screenshot, `node` parse, or project checker.
- If no automated checker exists, say that explicitly.

### Context Decay Countermeasures

- Re-read before editing after a long conversation or after switching files for a while.
- For files above 300 LOC, remove dead code first if a structural refactor is coming.
- After editing, read the changed section back to confirm the patch really landed.
- Do not trust stale memory of a large file.

### File Read and Search Discipline

- Large reads can truncate silently. Read in chunks when files are big.
- If grep results look suspiciously small, narrow the scope and rerun.
- Grep is not semantic search. When renaming or refactoring, search separate patterns for definitions, calls, strings, comments, and config references.

### Plan Before Build

- If the user says to plan first, produce only the plan.
- For non-trivial work, outline the approach before major edits if the tradeoffs are not obvious.
- When the user provides a concrete plan, follow it unless a real issue must be escalated.

### Follow References, Not Descriptions

- Existing working code is a better spec than a loose English description.
- When the user points at a file, component, or visualizer as the reference, study that first and match its patterns.
- When the user pastes logs, trace the real error instead of guessing.

### One-Word Mode

- If the user says "yes", "do it", or "push", execute immediately.
- Do not restate the plan unless something risky changed.

### Destructive Action Safety

- Never delete a file without checking whether something still references it.
- Never revert or overwrite user changes unless explicitly asked.
- Never push to shared remotes unless explicitly told to.

## Communication Style

- Be direct, concise, and factual.
- Explain tradeoffs clearly when they matter.
- Use visual and interactive metaphors when teaching concepts. The user learns best through shapes, colors, motion, and real-world analogies.
- Prefer milestone updates over chatter.
- If a fix fails twice, stop and explain what was wrong with the current mental model.

## Context and Session Management

### File System as State

- Use the file system as working memory for large investigations, smoke-test artifacts, and reproducible outputs.
- Save intermediate findings when they help verification or later sessions.
- Prefer selective reads and searches over dumping giant files into context.

### Compaction and Continuity

- Treat compaction like a save point. Preserve important decisions in markdown if the session is getting long.
- Resume or fork existing work when it preserves useful context better than starting from scratch.
- Delegate to sub-agents instead of switching the parent model mid-session when a task needs a different reasoning tier.

### Self-Improvement

- After the user corrects a recurring mistake, update the operating rules instead of repeating the mistake.
- After fixing a bug, explain why it happened and what prevents the category from recurring.
- For self-review, balance perfectionist criticism against pragmatic acceptance and let the user choose the tradeoff when needed.

## Project Overview

### Audience

- The repo is for educational, concept-first visualizations aimed at a strong visual learner.
- Favor obvious state changes, concrete examples, progressive walkthroughs, and clear mappings between code and on-screen behavior.

### Tech Stack

- Root visualizers are single HTML files with inline HTML, CSS, and JavaScript.
- Root visualizers use vanilla JS and Three.js from CDN. No build step.
- Some root pages also use Web Audio API for procedural sound.
- `QA automation courses/qa-html-export-v6.html` is a separate React/Babel course dashboard artifact and should not be treated as the architecture template for the root visualizers.

### Architecture Expectations

- Preserve the no-build workflow unless the user explicitly asks for a tooling change.
- Keep the root visualizers as single-file HTML unless the user explicitly asks for a refactor.
- Preserve keyboard-driven interaction where it already exists: `Space`, `S`, `R`, `M`, `?`, arrows, and related visualizer-specific controls.
- Maintain mobile and desktop usability. Do not break touch targets, panel layout, or code-panel readability.
- Dispose Three.js geometries, materials, textures, and temporary scene objects on rebuilds.
- Cap particle-style effects and other repeating scene allocations so they cannot grow without bound.

## Repo Shape

- Main standalone visualizers at the repo root:
  - `on2_visualization.html`
  - `username_validator_vis.html`
  - `data_structures_vis.html`
  - `oop_visualizer.html`
  - `inheritance_polymorphism_vis.html`
  - `oop_mental_map_vis.html`
- Course-source assets and exported dashboards live under `QA automation courses/`.
- If the user wants the latest course export, prefer `QA automation courses/qa-html-export-v6.html` unless told otherwise.
- Ignore `.claude/`, `everything-claude-code-main/`, and `src/` when the user says they are out of scope.

## Visualizer Notes

### `on2_visualization.html`

- Focus: nested loops, complexity growth, debugger-style execution tracing, traversal patterns, and comparison views.
- Preserve the code-to-visual mapping and the step-through clarity.

### `username_validator_vis.html`

- Focus: `do-while`, `for`, `continue`, `break`, sequential validation, and clear rule feedback.
- Keep the scanning and validation flow visually obvious.

### `data_structures_vis.html`

- Focus: arrays, lists, sets, maps, and practical complexity intuition.
- Keep examples grounded in QA or test-data use cases.

### `oop_visualizer.html`

- Focus: OOP basics, fields, methods, access control, constructors, static, final, and enums.
- Keep terminology and labels in English unless the user explicitly asks otherwise.

### `inheritance_polymorphism_vis.html`

- Focus: inheritance, `super()`, `@Override`, abstract classes, interfaces, and polymorphism.
- Preserve relationship lines, visual class hierarchy cues, and guided explanation flow.

### `oop_mental_map_vis.html`

- Focus: lesson 10 mental-model teaching using shape-first analogies for encapsulation, abstraction, abstract classes, interfaces, inheritance, and polymorphism.
- Keep the experience concept-first and low on syntax overload.

### `QA automation courses/qa-html-export-v6.html`

- This is the current React dashboard export for sessions and homework.
- Homework content should track the course source materials closely.
- If checklists belong inside homework, keep them inside homework instead of splitting them into fake standalone tabs.

## Frontend Expectations

- Avoid generic UI drift. Preserve the repo's bold educational feel.
- Favor visible cause-and-effect over decorative animation.
- When adding motion, make it explanatory rather than ornamental.
- Panels, badges, code views, and concept labels should reinforce the lesson, not compete with it.
- If a new page teaches a hard concept, use shapes, color groupings, and real-world analogies before dense prose.

## Editing Guidance

- Match the repo's existing educational tone: concrete, visual, narrated, and concept-first.
- Prefer the smallest viable change that keeps the file internally coherent.
- If a file has inconsistent naming, fix the inconsistency at the source rather than layering aliases on top.
- For course exports, keep the dashboard in English if that is the current direction, but do not silently rewrite source lesson files unless the user asks.

## Known Practical Constraints

- CDN dependencies mean some local HTML pages need browser network access to render fully.
- Browser smoke tests may need an unsandboxed run if the local sandbox blocks Chrome or Edge startup.
- A DOM dump proves React/Babel pages actually rendered. A non-empty screenshot is a reasonable smoke signal for the Three.js pages when full interaction automation is not available.
