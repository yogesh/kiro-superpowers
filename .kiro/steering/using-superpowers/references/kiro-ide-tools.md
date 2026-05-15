# Kiro Kiro IDE Tool Mapping

Skills use Claude Code tool names. Here are the Kiro IDE equivalents:

| Skill references | Kiro IDE equivalent |
|-----------------|--------------------------|
| `Skill tool / invoke skill` | #context-key manual steering reference |
| `TodoWrite / todo items` | Kiro task tracking via conversation |
| `EnterPlanMode` | Steering file workflow guidance |
| `Subagent dispatch` | invoke_sub_agent tool |
| `Claude Code / Codex / Gemini / Cursor` | "Kiro" or "the agent" |
| `Read tool` | read_file / read_files / readCode tools |
| `Write / Edit tool` | fs_write / str_replace tools |
| `Bash tool` | execute_bash tool |
| `WebSearch / WebFetch` | remote_web_search / web_fetch tools |
| `Hooks / session start` | Kiro hooks (fileEdited, promptSubmit, etc.) |
| `Git worktrees (platform commands)` | Standard git worktree via execute_bash |
| `"your human partner"` | "the user" |
| `Plugin marketplace` | Kiro Powers |
| `getDiagnostics tool` | getDiagnostics tool |
| `user_input tool` | user_input tool |
| `createHook tool` | createHook tool |

## Subagent Support

Sub-agent dispatch is supported via the `invoke_sub_agent` tool. Use it to delegate tasks to specialized sub-agents for parallel or focused execution.

## Hooks

Kiro hooks allow you to automate agent actions based on IDE events. Available hook events:

- `fileEdited`
- `fileCreated`
- `fileDeleted`
- `userTriggered`
- `promptSubmit`
- `agentStop`
- `preToolUse`
- `postToolUse`
- `preTaskExecution`
- `postTaskExecution`

Hook actions: `askAgent`, `runCommand`
