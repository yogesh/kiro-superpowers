# Kiro IDE Tool Reference

This is the native platform. Steering files use Kiro IDE tool names directly.

| Concept | Kiro IDE Tool |
|---------|---------------|
| Activate steering file | `discloseContext` tool or `#context-key` in chat |
| Task tracking | Track progress in conversation |
| Sub-agent dispatch | `invoke_sub_agent` tool |
| Read files | `read_file` / `read_files` / `readCode` tools |
| Write/Edit files | `fs_write` / `str_replace` tools |
| Run commands | `execute_bash` tool |
| Web search | `remote_web_search` / `web_fetch` tools |
| Hooks | Kiro hooks (fileEdited, promptSubmit, preToolUse, etc.) |
| Git worktrees | `git worktree` via `execute_bash` |
| Diagnostics | `getDiagnostics` tool |
| User input | `user_input` tool |
| Create hooks | `createHook` tool |
| Powers/Plugins | Kiro Powers via `kiroPowers` tool |

## Sub-agent Support

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
