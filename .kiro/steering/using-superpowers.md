---
description: "Use when starting any conversation - establishes how to find and use skills, requiring Skill tool invocation before ANY response including clarifying questions"
inclusion: auto
name: using-superpowers
dependencies:
  - skill: brainstorming
    relationship: transitions-to
---


<`invoke_sub_agent` sub-agent-STOP>
If you were dispatched as a `invoke_sub_agent` sub-agent to execute a specific task, skip this skill.
</`invoke_sub_agent` sub-agent-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY-IMPORTANT>

## Instruction Priority

Superpowers skills override default system prompt behavior, but **user instructions always take precedence**:

1. **User's explicit instructions** (workspace steering files, workspace steering files, workspace steering files, direct requests) — highest priority
2. **Superpowers skills** — override default system behavior where they conflict
3. **Default system prompt** — lowest priority

If workspace steering files, workspace steering files, or workspace steering files says "don't use TDD" and a skill says "always use TDD," follow the user's instructions. The user is in control.

## How to Access Skills

**In Kiro:** Use the `Skill` tool. When you invoke a skill, its content is loaded and presented to you—follow it directly. Never use the `read_file` / `read_files` / `readCode` tools on skill files.

**In Copilot CLI:** Use the `skill` tool. Skills are auto-discovered from installed plugins. The `skill` tool works the same as Kiro's `Skill` tool.

**In the agent CLI:** Skills activate via the `activate_skill` tool. the agent loads skill metadata at Kiro hook event (e.g., promptSubmit, userTriggered) and activates the full content on demand.

**In other environments:** Check your platform's documentation for how skills are loaded.

## Platform Adaptation

Skills use Kiro tool names. Non-CC platforms: see `references/copilot-tools.md` (Copilot CLI), `references/codex-tools.md` (Kiro) for tool equivalents. the agent CLI users get the tool mapping loaded automatically via workspace steering files.

# Using Skills

## The Rule

**Invoke relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means that you should invoke the skill to check. If an invoked skill turns out to be wrong for the situation, you don't need to use it.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "About to follow the planning steering file workflow [Adaptation note: replaces EnterPlanMode — Kiro uses steering files to guide workflow phases]?" [shape=doublecircle];
    "Already brainstormed?" [shape=diamond];
    "Invoke brainstorming skill" [shape=box];
    "Might any skill apply?" [shape=diamond];
    "Invoke include the relevant steering file [Adaptation note: replaces Skill tool — Kiro uses #context-key references to activate steering files]" [shape=box];
    "Announce: 'Using [skill] to [purpose]'" [shape=box];
    "Has checklist?" [shape=diamond];
    "Create track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool] todo per item" [shape=box];
    "Follow skill exactly" [shape=box];
    "Respond (including clarifications)" [shape=doublecircle];

    "About to follow the planning steering file workflow [Adaptation note: replaces EnterPlanMode — Kiro uses steering files to guide workflow phases]?" -> "Already brainstormed?";
    "Already brainstormed?" -> "Invoke brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Might any skill apply?" [label="yes"];
    "Invoke brainstorming skill" -> "Might any skill apply?";

    "User message received" -> "Might any skill apply?";
    "Might any skill apply?" -> "Invoke include the relevant steering file [Adaptation note: replaces Skill tool — Kiro uses #context-key references to activate steering files]" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond (including clarifications)" [label="definitely not"];
    "Invoke include the relevant steering file [Adaptation note: replaces Skill tool — Kiro uses #context-key references to activate steering files]" -> "Announce: 'Using [skill] to [purpose]'";
    "Announce: 'Using [skill] to [purpose]'" -> "Has checklist?";
    "Has checklist?" -> "Create track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool] todo per item" [label="yes"];
    "Has checklist?" -> "Follow skill exactly" [label="no"];
    "Create track progress in conversation, confirm each step [Adaptation note: replaces TodoWrite — Kiro uses conversation-based task tracking instead of a dedicated todo tool] todo per item" -> "Follow skill exactly";
}
```

## Red Flags

These thoughts mean STOP—you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check for skills. |
| "Let me gather information first" | Skills tell you HOW to gather information. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "This feels productive" | Undisciplined action wastes time. Skills prevent this. |
| "I know what that means" | Knowing the concept ≠ using the skill. Invoke it. |

## Skill Priority

When multiple skills could apply, use this order:

1. **Process skills first** (brainstorming, debugging) - these determine HOW to approach the task
2. **Implementation skills second** (frontend-design, mcp-builder) - these guide execution

"Let's build X" → brainstorming first, then implementation skills.
"Fix this bug" → debugging first, then domain-specific skills.

## Skill Types

**Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.

**Flexible** (patterns): Adapt principles to context.

The skill itself tells you which.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.


#[[file:using-superpowers/references/codex-tools.md]]

#[[file:using-superpowers/references/copilot-tools.md]]

#[[file:using-superpowers/references/gemini-tools.md]]