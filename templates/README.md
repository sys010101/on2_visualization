# AI Coding Agent — Directive Templates

Production-grade directive files for every major AI coding tool. Each file
contains the same core directives adapted to the specific tool's format and
conventions.

Drop the right file into your project root (or the tool's expected path),
rename if needed, and fill in the Project-Specific section at the bottom.

## Files

| File | Tool | Where to place it | Auto-loaded? |
|------|------|--------------------|--------------|
| `CLAUDE_TEMPLATE.md` | Claude Code | Project root → rename to `CLAUDE.md` | Yes |
| `AGENTS.md` | OpenAI Codex CLI | Project root (also supports subdirs) | Yes |
| `.cursorrules` | Cursor | Project root | Yes (legacy; new: `.cursor/rules/*.mdc`) |
| `GEMINI.md` | Gemini CLI | Project root | Yes |
| `.windsurfrules` | Windsurf / Codeium | Project root | Yes |
| `copilot-instructions.md` | GitHub Copilot | `.github/copilot-instructions.md` | Yes (~8K char limit) |
| `.clinerules` | Cline (VS Code) | Project root (or `.clinerules/` directory) | Yes |
| `CONVENTIONS.md` | Aider | Project root → `aider --read CONVENTIONS.md` | **No** — needs CLI flag |

## Not Covered

| Tool | Why |
|------|-----|
| **Grok (xAI)** | No coding agent or CLI as of 2025. Chat-only. |
| **Amazon Q Developer** | No project directive file convention. Uses code indexing. |

## What's Inside

Every template contains the same battle-tested directives:

- **Step 0 / Dead Code Cleanup** — prevent context bloat before refactoring
- **Plan Before Build** — separate planning from coding
- **Phased Execution** — max 5 files per phase, verify between phases
- **Senior Dev Override** — fix architecture, don't band-aid
- **Forced Verification** — must run build/test before claiming done
- **Write Human Code** — no robotic output, no over-engineering
- **One Source of Truth** — never duplicate state
- **Edit Safety** — re-read before/after edits, thorough rename search
- **Self-Improvement** — log mistakes, explain bugs, rethink after 2 failures

## Claude-Specific Extras

The `CLAUDE_TEMPLATE.md` (parent of this project) has additional sections
that only apply to Claude Code:

- **Zerg Overmind Protocol** — sub-agent spawning with model routing
- **Context Decay Countermeasures** — file read budget, tool truncation awareness
- **Prompt Cache Awareness** — don't break the prefix cache
- **Session Continuity** — `--continue` and `--fork-session`
- **Player Hotkeys** — `/compact`, `/model`, `/plan`, etc.

## Source

Distilled from:
- [Anthropic's Claude Code source](https://github.com/anthropics/claude-code)
- [everything-claude-code patterns](https://github.com/anthropics/everything-claude-code)
- [@fakeguru's production CLAUDE.md](https://x.com/iamfakeguru/status/2038965567269249484)
- Real-world agent sessions across thousands of tokens of debugging
