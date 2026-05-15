# Superpowers for Kiro IDE

A complete port of the [Superpowers methodology](https://github.com/obra/superpowers) (v5.1.0) by Jesse Vincent — rebuilt from the ground up as **native Kiro IDE steering files**.

This isn't a thin wrapper or compatibility shim. Every steering file has been rewritten to use Kiro's native tools (`invoke_sub_agent`, `execute_bash`, `discloseContext`, `control_bash_process`, etc.), native terminology, and native activation patterns. Zero references to Claude Code, Gemini, Copilot, or Codex remain.

## What Is Superpowers?

Superpowers is a structured software development methodology that guides AI-assisted development through distinct workflow phases:

```
brainstorm → plan → implement → review
```

Each phase has dedicated steering files with guardrails, verification gates, and anti-rationalization patterns that keep the agent disciplined even under pressure. The methodology enforces:

- **Design before code** — brainstorming steering file blocks implementation until design is approved
- **Plans before action** — bite-sized, TDD-driven task plans with exact file paths and code
- **Tests before implementation** — strict RED-GREEN-REFACTOR with no exceptions
- **Evidence before claims** — verification steering file requires running commands before claiming success
- **Sub-agent isolation** — fresh context per task prevents pollution and drift

## What Was Done (Kiro-Native Overhaul)

The original Superpowers was built for Claude Code with multi-platform adaptation layers for Gemini CLI, Copilot CLI, and Codex. This port strips all of that away and rebuilds for Kiro IDE as the single target:

| Before | After |
|--------|-------|
| "Skill tool" / "invoke skill" | `discloseContext` / steering file activation |
| "TodoWrite" with verbose adaptation notes | "Track progress in conversation" (clean) |
| "EnterPlanMode" with adaptation notes | Removed — Kiro uses steering files natively |
| "`invoke_sub_agent` sub-agent" verbose phrasing | Clean "sub-agent" terminology |
| "your human partner" | "the user" |
| `git worktree (via \`execute_bash\`)` | `git worktree` (clean commands) |
| Multi-platform launch instructions | Kiro-only (`control_bash_process`) |
| 4 platform reference files (Codex, Copilot, Gemini, Claude Code) | Single `kiro-ide-tools.md` reference |
| Claude-specific test documentation | Removed |
| "project docs directory" placeholder | Actual paths (`docs/designs/`, `docs/superpowers/plans/`) |

**Net result:** 713 lines removed, 289 lines rewritten. Cleaner, faster to load, zero confusion about which platform you're on.

## Prerequisites

- Kiro IDE with steering file support
- A workspace where you want to apply the Superpowers methodology

## Installation

### Copy-based installation

Copy the `.kiro/steering/` directory into your project:

```bash
cp -r .kiro/steering/ /path/to/your/project/.kiro/steering/
```

### Git submodule installation

```bash
git submodule add <repository-url> .kiro/steering/superpowers
```

Then symlink or copy the steering files into your project's `.kiro/steering/` directory.

## Quick Start

The `using-superpowers` steering file loads automatically (`inclusion: auto`). It bootstraps the methodology and guides you to activate phase-specific steering files as needed.

To activate a specific steering file, use its context key in your conversation:
- `#brainstorming` — before any creative work
- `#writing-plans` — to create implementation plans
- `#test-driven-development` — TDD guidance
- `#systematic-debugging` — before proposing fixes

## Available Steering Files

### Workflow Phase: Brainstorm
- **`brainstorming`** — Explores user intent, requirements and design before implementation. MUST be used before any creative work.

### Workflow Phase: Plan
- **`writing-plans`** — Creates detailed, bite-sized implementation plans with exact file paths, code, and TDD steps.

### Workflow Phase: Implement
- **`executing-plans`** — Executes plans in session with review checkpoints
- **`subagent-driven-development`** — Dispatches fresh sub-agent per task with two-stage review (spec compliance + code quality)
- **`test-driven-development`** — Strict RED-GREEN-REFACTOR. No production code without a failing test first.
- **`dispatching-parallel-agents`** — Parallel sub-agent dispatch for independent problem domains
- **`using-git-worktrees`** — Isolated workspace setup via git worktree

### Workflow Phase: Review
- **`verification-before-completion`** — Evidence before claims. Run the command, read the output, THEN claim the result.
- **`systematic-debugging`** — Four-phase root cause investigation. No fixes without understanding.
- **`requesting-code-review`** — Dispatch reviewer sub-agents with structured templates
- **`receiving-code-review`** — Technical evaluation of feedback, not performative agreement
- **`finishing-a-development-branch`** — Structured merge/PR/discard options after implementation

### Cross-Cutting
- **`using-superpowers`** — Bootstrap (auto-loaded). Establishes steering file activation rules.
- **`writing-skills`** — Meta: creating and testing new steering files using TDD methodology.

## Source

Ported from the [Superpowers methodology](https://github.com/obra/superpowers) v5.1.0 by Jesse Vincent.

- Source commit: `f2cbfbefebbfef77321e4c9abc9e949826bea9d7`
- Original conversion: 2026-05-15
- Kiro-native overhaul: 2025-07-15

## License

MIT License — see [LICENSE](./LICENSE) for details.
