---
description: Use when executing implementation plans with independent tasks in the current session
inclusion: manual
dependencies:
  - skill: executing-plans
    relationship: uses
  - skill: finishing-a-development-branch
    relationship: uses
  - skill: requesting-code-review
    relationship: uses
  - skill: test-driven-development
    relationship: uses
  - skill: using-git-worktrees
    relationship: uses
  - skill: writing-plans
    relationship: uses
---


# `invoke_sub_agent` sub-agent-Driven Development

Execute plan by dispatching fresh `invoke_sub_agent` sub-agent per task, with two-stage review after each: spec compliance review first, then code quality review.

**Why subagents:** You delegate tasks to specialized agents with isolated context. By precisely crafting their instructions and context, you ensure they stay focused and succeed at their task. They should never inherit your session's context or history — you construct exactly what they need. This also preserves your own context for coordination work.

**Core principle:** Fresh `invoke_sub_agent` sub-agent per task + two-stage review (spec then quality) = high quality, fast iteration

**Continuous execution:** Do not pause to check in with the user between tasks. Execute all tasks from the plan without stopping. The only reasons to stop are: BLOCKED status you cannot resolve, ambiguity that genuinely prevents progress, or all tasks complete. "Should I continue?" prompts and progress summaries waste their time — they asked you to execute the plan, so execute it.

## When to Use

```dot
digraph when_to_use {
    "Have implementation plan?" [shape=diamond];
    "Tasks mostly independent?" [shape=diamond];
    "Stay in this session?" [shape=diamond];
    "`invoke_sub_agent` sub-agent-driven-development" [shape=box];
    "executing-plans" [shape=box];
    "Manual execution or brainstorm first" [shape=box];

    "Have implementation plan?" -> "Tasks mostly independent?" [label="yes"];
    "Have implementation plan?" -> "Manual execution or brainstorm first" [label="no"];
    "Tasks mostly independent?" -> "Stay in this session?" [label="yes"];
    "Tasks mostly independent?" -> "Manual execution or brainstorm first" [label="no - tightly coupled"];
    "Stay in this session?" -> "`invoke_sub_agent` sub-agent-driven-development" [label="yes"];
    "Stay in this session?" -> "executing-plans" [label="no - parallel session"];
}
```

**vs. Executing Plans (parallel session):**
- Same session (no context switch)
- Fresh `invoke_sub_agent` sub-agent per task (no context pollution)
- Two-stage review after each task: spec compliance first, then code quality
- Faster iteration (no human-in-loop between tasks)

## The Process

```dot
digraph process {
    rankdir=TB;

    subgraph cluster_per_task {
        label="Per Task";
        "Dispatch implementer `invoke_sub_agent` sub-agent (./implementer-prompt.md)" [shape=box];
        "Implementer `invoke_sub_agent` sub-agent asks questions?" [shape=diamond];
        "Answer questions, provide context" [shape=box];
        "Implementer `invoke_sub_agent` sub-agent implements, tests, commits, self-reviews" [shape=box];
        "Dispatch spec reviewer `invoke_sub_agent` sub-agent (./spec-reviewer-prompt.md)" [shape=box];
        "Spec reviewer `invoke_sub_agent` sub-agent confirms code matches spec?" [shape=diamond];
        "Implementer `invoke_sub_agent` sub-agent fixes spec gaps" [shape=box];
        "Dispatch code quality reviewer `invoke_sub_agent` sub-agent (./code-quality-reviewer-prompt.md)" [shape=box];
        "Code quality reviewer `invoke_sub_agent` sub-agent approves?" [shape=diamond];
        "Implementer `invoke_sub_agent` sub-agent fixes quality issues" [shape=box];
        "Mark task complete in track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool]" [shape=box];
    }

    "Read plan, extract all tasks with full text, note context, create track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool]" [shape=box];
    "More tasks remain?" [shape=diamond];
    "Dispatch final code reviewer `invoke_sub_agent` sub-agent for entire implementation" [shape=box];
    "Use superpowers:finishing-a-development-branch" [shape=box style=filled fillcolor=lightgreen];

    "Read plan, extract all tasks with full text, note context, create track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool]" -> "Dispatch implementer `invoke_sub_agent` sub-agent (./implementer-prompt.md)";
    "Dispatch implementer `invoke_sub_agent` sub-agent (./implementer-prompt.md)" -> "Implementer `invoke_sub_agent` sub-agent asks questions?";
    "Implementer `invoke_sub_agent` sub-agent asks questions?" -> "Answer questions, provide context" [label="yes"];
    "Answer questions, provide context" -> "Dispatch implementer `invoke_sub_agent` sub-agent (./implementer-prompt.md)";
    "Implementer `invoke_sub_agent` sub-agent asks questions?" -> "Implementer `invoke_sub_agent` sub-agent implements, tests, commits, self-reviews" [label="no"];
    "Implementer `invoke_sub_agent` sub-agent implements, tests, commits, self-reviews" -> "Dispatch spec reviewer `invoke_sub_agent` sub-agent (./spec-reviewer-prompt.md)";
    "Dispatch spec reviewer `invoke_sub_agent` sub-agent (./spec-reviewer-prompt.md)" -> "Spec reviewer `invoke_sub_agent` sub-agent confirms code matches spec?";
    "Spec reviewer `invoke_sub_agent` sub-agent confirms code matches spec?" -> "Implementer `invoke_sub_agent` sub-agent fixes spec gaps" [label="no"];
    "Implementer `invoke_sub_agent` sub-agent fixes spec gaps" -> "Dispatch spec reviewer `invoke_sub_agent` sub-agent (./spec-reviewer-prompt.md)" [label="re-review"];
    "Spec reviewer `invoke_sub_agent` sub-agent confirms code matches spec?" -> "Dispatch code quality reviewer `invoke_sub_agent` sub-agent (./code-quality-reviewer-prompt.md)" [label="yes"];
    "Dispatch code quality reviewer `invoke_sub_agent` sub-agent (./code-quality-reviewer-prompt.md)" -> "Code quality reviewer `invoke_sub_agent` sub-agent approves?";
    "Code quality reviewer `invoke_sub_agent` sub-agent approves?" -> "Implementer `invoke_sub_agent` sub-agent fixes quality issues" [label="no"];
    "Implementer `invoke_sub_agent` sub-agent fixes quality issues" -> "Dispatch code quality reviewer `invoke_sub_agent` sub-agent (./code-quality-reviewer-prompt.md)" [label="re-review"];
    "Code quality reviewer `invoke_sub_agent` sub-agent approves?" -> "Mark task complete in track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool]" [label="yes"];
    "Mark task complete in track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool]" -> "More tasks remain?";
    "More tasks remain?" -> "Dispatch implementer `invoke_sub_agent` sub-agent (./implementer-prompt.md)" [label="yes"];
    "More tasks remain?" -> "Dispatch final code reviewer `invoke_sub_agent` sub-agent for entire implementation" [label="no"];
    "Dispatch final code reviewer `invoke_sub_agent` sub-agent for entire implementation" -> "Use superpowers:finishing-a-development-branch";
}
```

## Model Selection

Use the least powerful model that can handle each role to conserve cost and increase speed.

**Mechanical implementation tasks** (isolated functions, clear specs, 1-2 files): use a fast, cheap model. Most implementation tasks are mechanical when the plan is well-specified.

**Integration and judgment tasks** (multi-file coordination, pattern matching, debugging): use a standard model.

**Architecture, design, and review tasks**: use the most capable available model.

**Task complexity signals:**
- Touches 1-2 files with a complete spec → cheap model
- Touches multiple files with integration concerns → standard model
- Requires design judgment or broad codebase understanding → most capable model

## Handling Implementer Status

Implementer subagents report one of four statuses. Handle each appropriately:

**DONE:** Proceed to spec compliance review.

**DONE_WITH_CONCERNS:** The implementer completed the work but flagged doubts. Read the concerns before proceeding. If the concerns are about correctness or scope, address them before review. If they're observations (e.g., "this file is getting large"), note them and proceed to review.

**NEEDS_CONTEXT:** The implementer needs information that wasn't provided. Provide the missing context and re-dispatch.

**BLOCKED:** The implementer cannot complete the task. Assess the blocker:
1. If it's a context problem, provide more context and re-dispatch with the same model
2. If the task requires more reasoning, re-dispatch with a more capable model
3. If the task is too large, break it into smaller pieces
4. If the plan itself is wrong, escalate to the human

**Never** ignore an escalation or force the same model to retry without changes. If the implementer said it's stuck, something needs to change.

## Prompt Templates

- `./implementer-prompt.md` - Dispatch implementer `invoke_sub_agent` sub-agent
- `./spec-reviewer-prompt.md` - Dispatch spec compliance reviewer `invoke_sub_agent` sub-agent
- `./code-quality-reviewer-prompt.md` - Dispatch code quality reviewer `invoke_sub_agent` sub-agent

## Example Workflow

```
You: I'm using `invoke_sub_agent` sub-agent-Driven Development to execute this plan.

[Read plan file once: docs/superpowers/plans/feature-plan.md]
[Extract all 5 tasks with full text and context]
[Create track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool] with all tasks]

Task 1: Hook installation script

[Get Task 1 text and context (already extracted)]
[Dispatch implementation `invoke_sub_agent` sub-agent with full task text + context]

Implementer: "Before I begin - should the hook be installed at user or system level?"

You: "User level (~/.config/superpowers/Kiro hooks (e.g., fileEdited, promptSubmit, preToolUse)/)"

Implementer: "Got it. Implementing now..."
[Later] Implementer:
  - Implemented install-hook command
  - Added tests, 5/5 passing
  - Self-review: Found I missed --force flag, added it
  - Committed

[Dispatch spec compliance reviewer]
Spec reviewer: ✅ Spec compliant - all requirements met, nothing extra

[Get git SHAs, dispatch code quality reviewer]
Code reviewer: Strengths: Good test coverage, clean. Issues: None. Approved.

[Mark Task 1 complete]

Task 2: Recovery modes

[Get Task 2 text and context (already extracted)]
[Dispatch implementation `invoke_sub_agent` sub-agent with full task text + context]

Implementer: [No questions, proceeds]
Implementer:
  - Added verify/repair modes
  - 8/8 tests passing
  - Self-review: All good
  - Committed

[Dispatch spec compliance reviewer]
Spec reviewer: ❌ Issues:
  - Missing: Progress reporting (spec says "report every 100 items")
  - Extra: Added --json flag (not requested)

[Implementer fixes issues]
Implementer: Removed --json flag, added progress reporting

[Spec reviewer reviews again]
Spec reviewer: ✅ Spec compliant now

[Dispatch code quality reviewer]
Code reviewer: Strengths: Solid. Issues (Important): Magic number (100)

[Implementer fixes]
Implementer: Extracted PROGRESS_INTERVAL constant

[Code reviewer reviews again]
Code reviewer: ✅ Approved

[Mark Task 2 complete]

...

[After all tasks]
[Dispatch final code-reviewer]
Final reviewer: All requirements met, ready to merge

Done!
```

## Advantages

**vs. Manual execution:**
- Subagents follow TDD naturally
- Fresh context per task (no confusion)
- Parallel-safe (subagents don't interfere)
- `invoke_sub_agent` sub-agent can ask questions (before AND during work)

**vs. Executing Plans:**
- Same session (no handoff)
- Continuous progress (no waiting)
- Review checkpoints automatic

**Efficiency gains:**
- No file reading overhead (controller provides full text)
- Controller curates exactly what context is needed
- `invoke_sub_agent` sub-agent gets complete information upfront
- Questions surfaced before work begins (not after)

**Quality gates:**
- Self-review catches issues before handoff
- Two-stage review: spec compliance, then code quality
- Review loops ensure fixes actually work
- Spec compliance prevents over/under-building
- Code quality ensures implementation is well-built

**Cost:**
- More `invoke_sub_agent` sub-agent invocations (implementer + 2 reviewers per task)
- Controller does more prep work (extracting all tasks upfront)
- Review loops add iterations
- But catches issues early (cheaper than debugging later)

## Red Flags

**Never:**
- Start implementation on main/master branch without explicit user consent
- Skip reviews (spec compliance OR code quality)
- Proceed with unfixed issues
- Dispatch multiple implementation subagents in parallel (conflicts)
- Make `invoke_sub_agent` sub-agent read plan file (provide full text instead)
- Skip scene-setting context (`invoke_sub_agent` sub-agent needs to understand where task fits)
- Ignore `invoke_sub_agent` sub-agent questions (answer before letting them proceed)
- Accept "close enough" on spec compliance (spec reviewer found issues = not done)
- Skip review loops (reviewer found issues = implementer fixes = review again)
- Let implementer self-review replace actual review (both are needed)
- **Start code quality review before spec compliance is ✅** (wrong order)
- Move to next task while either review has open issues

**If `invoke_sub_agent` sub-agent asks questions:**
- Answer clearly and completely
- Provide additional context if needed
- Don't rush them into implementation

**If reviewer finds issues:**
- Implementer (same `invoke_sub_agent` sub-agent) fixes them
- Reviewer reviews again
- Repeat until approved
- Don't skip the re-review

**If `invoke_sub_agent` sub-agent fails task:**
- Dispatch fix `invoke_sub_agent` sub-agent with specific instructions
- Don't try to fix manually (context pollution)

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** - Ensures isolated workspace (creates one or verifies existing)
- **superpowers:writing-plans** - Creates the plan this skill executes
- **superpowers:requesting-code-review** - Code review template for reviewer subagents
- **superpowers:finishing-a-development-branch** - Complete development after all tasks

**Subagents should use:**
- **superpowers:test-driven-development** - Subagents follow TDD for each task

**Alternative workflow:**
- **superpowers:executing-plans** - Use for parallel session instead of same-session execution


#[[file:subagent-driven-development/code-quality-reviewer-prompt.md]]

#[[file:subagent-driven-development/implementer-prompt.md]]

#[[file:subagent-driven-development/spec-reviewer-prompt.md]]