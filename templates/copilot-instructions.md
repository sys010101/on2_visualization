# copilot-instructions.md — GitHub Copilot

<!-- Place this file at: .github/copilot-instructions.md               -->
<!-- Copilot auto-loads it for chat and code generation.               -->
<!-- Note: Copilot has an ~8,000 character instruction limit.          -->
<!-- Keep this file concise — every word counts against that budget.   -->

## Code Quality

- **Senior Dev standard**: If architecture is flawed or patterns inconsistent, propose structural fixes. Don't leave known broken things behind.
- **Verify before done**: Run type-checker/linter/build. Check tests pass. Never claim success without verification.
- **Human code**: No robotic comments, no over-engineering for imaginary scenarios. Simple and correct wins.
- **One source of truth**: Never duplicate data or state to fix a problem.

## Pre-Work

- **Step 0**: Before refactoring a file >300 LOC, remove dead code first. Commit cleanup separately.
- **Plan before build**: When asked to plan, output only the plan. No code until approved. For vague instructions, outline and get approval.
- **Phased execution**: Max 5 files per refactor phase. Verify between phases.

## Understanding Intent

- **Follow references**: When user points to existing code, match its patterns exactly. Working code > English descriptions.
- **Raw data**: When given error logs, trace the actual error. Don't guess.
- **Execute on confirmation**: "Yes" / "do it" = act immediately. No repeating the plan.

## Safety

- Re-read files before and after editing to verify changes applied.
- When renaming: search ALL references — calls, types, strings, imports, re-exports, tests. Assume grep missed something.
- Never delete files without verifying no references exist.
- Never push unless explicitly told to.

## Self-Improvement

- After user corrections, remember the pattern.
- After fixing bugs, explain WHY it happened.
- Two failed fix attempts = stop, re-read context, rethink approach.

## Project-Specific

<!-- Add project overview, tech stack, architecture below. -->
