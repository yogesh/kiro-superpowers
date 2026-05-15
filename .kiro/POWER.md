# Superpowers for Kiro

> Superpowers v5.1.0 — Converted on 2026-05-15

## Overview

**Superpowers** is a structured software development methodology (v5.1.0) by Jesse Vincent that provides a comprehensive set of skills for AI-assisted development. It enforces a disciplined workflow — brainstorm → plan → implement → review — ensuring that design precedes code, plans are actionable, tests drive development, and verification is evidence-based.

This power packages all 14 Superpowers skills as Kiro steering files, preserving the methodology's structured workflow while adapting platform-specific concepts to Kiro's agent model.

## Skill Registry

| Skill | Description | Inclusion | Steering File |
|---|---|---|---|
| brainstorming | You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation. | manual | `.kiro/steering/brainstorming.md` |
| dispatching-parallel-agents | Use when facing 2+ independent tasks that can be worked on without shared state or sequential dependencies | manual | `.kiro/steering/dispatching-parallel-agents.md` |
| executing-plans | Use when you have a written implementation plan to execute in a separate session with review checkpoints | manual | `.kiro/steering/executing-plans.md` |
| finishing-a-development-branch | Use when implementation is complete, all tests pass, and you need to decide how to integrate the work - guides completion of development work by presenting structured options for merge, PR, or cleanup | manual | `.kiro/steering/finishing-a-development-branch.md` |
| receiving-code-review | Use when receiving code review feedback, before implementing suggestions, especially if feedback seems unclear or technically questionable - requires technical rigor and verification, not performative agreement or blind implementation | manual | `.kiro/steering/receiving-code-review.md` |
| requesting-code-review | Use when completing tasks, implementing major features, or before merging to verify work meets requirements | manual | `.kiro/steering/requesting-code-review.md` |
| subagent-driven-development | Use when executing implementation plans with independent tasks in the current session | manual | `.kiro/steering/subagent-driven-development.md` |
| systematic-debugging | Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes | manual | `.kiro/steering/systematic-debugging.md` |
| test-driven-development | Use when implementing any feature or bugfix, before writing implementation code | manual | `.kiro/steering/test-driven-development.md` |
| using-git-worktrees | Use when starting feature work that needs isolation from current workspace or before executing implementation plans - ensures an isolated workspace exists via native tools or git worktree fallback | manual | `.kiro/steering/using-git-worktrees.md` |
| using-superpowers | Use when starting any conversation - establishes how to find and use skills, requiring Skill tool invocation before ANY response including clarifying questions | auto | `.kiro/steering/using-superpowers.md` |
| verification-before-completion | Use when about to claim work is complete, fixed, or passing, before committing or creating PRs - requires running verification commands and confirming output before making any success claims; evidence before assertions always | manual | `.kiro/steering/verification-before-completion.md` |
| writing-plans | Use when you have a spec or requirements for a multi-step task, before touching code | manual | `.kiro/steering/writing-plans.md` |
| writing-skills | Use when creating new skills, editing existing skills, or verifying skills work before deployment | manual | `.kiro/steering/writing-skills.md` |

## Usage

Skills are loaded into the Kiro agent's context using one of three inclusion methods:

### Auto Inclusion

Steering files with `inclusion: auto` are always loaded into the agent's context at the start of every conversation. The bootstrap skill (`using-superpowers`) uses this mode to establish the methodology framework.

### File Match Inclusion

Steering files with `inclusion: fileMatch` are conditionally loaded when the user has files open that match the specified glob pattern. This allows skills to activate contextually based on the type of work being done.

### Manual Inclusion

Steering files with `inclusion: manual` are activated by the user via a context key (e.g., `#brainstorming`, `#writing-plans`). This gives the user explicit control over which skills are active during a session.

To activate a manual skill, reference its context key in your conversation:
```
#brainstorming
```

## Workflow

The Superpowers methodology follows a structured phase ordering. Each phase must be completed before moving to the next:

```
brainstorm → plan → implement → review
```

### Brainstorm

Explore ideas and design before any implementation.

**Skills:**
- ✓ `brainstorming`

### Plan

Create detailed, bite-sized implementation plans.

**Skills:**
- ✓ `writing-plans`

### Implement

Execute plans with structured development practices.

**Skills:**
- ✓ `executing-plans`
- ✓ `subagent-driven-development`
- ✓ `test-driven-development`
- ✓ `dispatching-parallel-agents`
- ✓ `using-git-worktrees`

### Review

Verify, debug, and review completed work.

**Skills:**
- ✓ `verification-before-completion`
- ✓ `systematic-debugging`
- ✓ `requesting-code-review`
- ✓ `receiving-code-review`
- ✓ `finishing-a-development-branch`

### Cross-Cutting

Skills that apply across all phases.

**Skills:**
- ✓ `using-superpowers` (bootstrap — always active)
- ✓ `writing-skills` (meta — creating new skills)

## Dependency Graph

Skills reference and depend on each other. The following graph documents all interdependencies:

**brainstorming**
- transitions-to: `writing-plans`

**executing-plans**
- uses: `finishing-a-development-branch`
- uses: `subagent-driven-development`
- uses: `using-git-worktrees`
- uses: `writing-plans`

**requesting-code-review**
- transitions-to: `executing-plans`
- transitions-to: `subagent-driven-development`

**subagent-driven-development**
- uses: `executing-plans`
- uses: `finishing-a-development-branch`
- uses: `requesting-code-review`
- uses: `test-driven-development`
- uses: `using-git-worktrees`
- uses: `writing-plans`

**systematic-debugging**
- requires: `test-driven-development`
- uses: `verification-before-completion`

**using-superpowers**
- transitions-to: `brainstorming`

**writing-plans**
- uses: `brainstorming`
- uses: `executing-plans`
- uses: `subagent-driven-development`
- uses: `using-git-worktrees`

**writing-skills**
- uses: `executing-plans`
- uses: `systematic-debugging`
- requires: `test-driven-development`
- uses: `verification-before-completion`


## License

This power is derived from the [Superpowers](https://github.com/obra/superpowers) repository by Jesse Vincent, licensed under the MIT License.

See the LICENSE file in the package root for the full license text.

## Version

- **Source**: Superpowers v5.1.0
- **Repository**: https://github.com/obra/superpowers
- **Author**: Jesse Vincent
- **Conversion Date**: 2026-05-15
- **Source Commit**: `f2cbfbefebbfef77321e4c9abc9e949826bea9d7`