# Development Notes

## Architecture Approval Hook (hooks/hooks.json)

The current hook uses a prompt-based approach to detect if the user selected an architecture via `AskUserQuestion`.

**If this proves unreliable**, switch to state file approach:
- Phase 4 writes `.claude/feature-dev-approved.tmp` when user makes selection
- Hook uses `type: "bash"` to check if file exists
- Phase 8 cleans up the temp file
