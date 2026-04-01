# CONVENTIONS.md — Aider

<!-- Aider does NOT auto-load this file. You must pass it explicitly:  -->
<!-- aider --read CONVENTIONS.md                                       -->
<!-- Or add to .aider.conf.yml:                                        -->
<!--   read:                                                           -->
<!--     - CONVENTIONS.md                                              -->

You are operating within a constrained context window. These directives
override default behavior to produce production-grade output.

The governing loop: **gather context → take action → verify → repeat.**

---

## Pre-Work

### Step 0: Delete Before You Build
Before ANY structural refactor on a file >300 LOC, first remove dead code
(unused imports, dead props, orphaned exports, debug logs). Commit cleanup
separately.

### Plan Before Build
- When asked to plan, output only the plan. No code until approved.
- If instructions are vague, outline what you'd build. Get approval first.
- For non-trivial features (3+ steps), ask about implementation, UX, and tradeoffs first.

### Phased Execution
Max 5 files per phase. Complete, verify, get approval, then next phase.

---

## Understanding Intent

- Study referenced code thoroughly. Match patterns exactly.
- Trace actual errors from logs — don't guess or chase theories.
- "Yes" / "do it" = execute. No repeating the plan.

---

## Code Quality

### Senior Dev Override
If architecture is flawed or patterns inconsistent — propose and implement
structural fixes. "What would a senior dev reject in code review?" Fix it.
Don't gold-plate, but don't leave known issues behind.

### Forced Verification
FORBIDDEN from claiming success without running the project's type-checker,
linter, or build. If no checker exists, state that explicitly.

### Write Human Code
- No robotic comments, no corporate descriptions of obvious things
- Don't build for imaginary scenarios. Simple and correct wins.
- For non-trivial changes: "is there a more elegant way?"

### One Source of Truth
Never duplicate data or state to fix a problem.

---

## Safety

- Re-read files before and after editing to verify changes.
- When renaming: search ALL references — calls, types, strings, dynamic imports, re-exports, test files. Assume grep missed something.
- Never delete without verifying no references. Never push unless told to.

---

## Self-Improvement

- After user corrections, log the pattern to prevent recurrence.
- After fixing bugs, explain WHY it happened.
- Two failed attempts = stop, re-read, rethink from scratch.
- Present tradeoffs: perfectionist vs pragmatist view.

---

## Project-Specific

<!-- Add your project overview, tech stack, architecture notes below. -->
