# Superpowers Kiro Power

The Superpowers methodology (v5.1.0) by Jesse Vincent is a structured software development methodology that guides AI-assisted development through distinct workflow phases: brainstorming, planning, implementation, and review. This power packages all 14 skills as Kiro steering files, enabling teams to adopt consistent, high-quality development practices with phase-appropriate guardrails and verification gates.

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

The `using-superpowers` steering file loads automatically when the power is installed (inclusion: auto). It serves as the bootstrap skill that introduces the methodology and guides you to activate phase-specific skills as needed.

To activate a specific skill, use its context key in your conversation. For example:
- `#brainstorming` — activate the brainstorming skill before creative work
- `#writing-plans` — activate the planning skill to create implementation plans
- `#test-driven-development` — activate TDD guidance

## Available Skills

- `brainstorming` — You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation.
- `dispatching-parallel-agents` — Use when facing 2+ independent tasks that can be worked on without shared state or sequential dependencies
- `executing-plans` — Use when you have a written implementation plan to execute in a separate session with review checkpoints
- `finishing-a-development-branch` — Use when implementation is complete, all tests pass, and you need to decide how to integrate the work - guides completion of development work by presenting structured options for merge, PR, or cleanup
- `receiving-code-review` — Use when receiving code review feedback, before implementing suggestions, especially if feedback seems unclear or technically questionable - requires technical rigor and verification, not performative agreement or blind implementation
- `requesting-code-review` — Use when completing tasks, implementing major features, or before merging to verify work meets requirements
- `subagent-driven-development` — Use when executing implementation plans with independent tasks in the current session
- `systematic-debugging` — Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes
- `test-driven-development` — Use when implementing any feature or bugfix, before writing implementation code
- `using-git-worktrees` — Use when starting feature work that needs isolation from current workspace or before executing implementation plans - ensures an isolated workspace exists via native tools or git worktree fallback
- `using-superpowers` — Use when starting any conversation - establishes how to find and use skills, requiring Skill tool invocation before ANY response including clarifying questions
- `verification-before-completion` — Use when about to claim work is complete, fixed, or passing, before committing or creating PRs - requires running verification commands and confirming output before making any success claims; evidence before assertions always
- `writing-plans` — Use when you have a spec or requirements for a multi-step task, before touching code
- `writing-skills` — Use when creating new skills, editing existing skills, or verifying skills work before deployment

## Source

Converted from the [Superpowers methodology](https://github.com/obra/superpowers.git) v5.1.0 by Jesse Vincent.

Source commit: `f2cbfbefebbfef77321e4c9abc9e949826bea9d7`
Conversion date: 2026-05-15

## License

MIT License — see [LICENSE](./LICENSE) for details.
