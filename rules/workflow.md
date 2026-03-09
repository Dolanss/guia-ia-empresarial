# Workflow Rules

## Git Discipline
- Check `git status` before committing — verify no sensitive files are staged
- Commit with descriptive messages in imperative mood
- Commit and push proactively after batching related edits — don't wait to be told
- Never force-push without explicit user approval
- Prefer feature branches over direct commits to main

## Task Completion
- Never claim a task is done without verifying the output. "It's running" is not "it's done."
- If a task fails or blocks, fix it or provide an alternative immediately
- For async/background tasks: wait for completion or explicitly say it's running

## Verify Before Reporting Done
- When you deploy any change (code, config, thresholds), verify it works before reporting done
- Run the program, trigger the failure path, read the output
- For config changes: restart the service and test the affected behavior
- Don't move to the next topic until the current fix is confirmed working

## File Management
- ISO dates in filenames: `YYYY-MM-DD-<kebab-slug>.md`
- Prefer kebab-case for file slugs
- YAML frontmatter on knowledge files (tags, date, type)

## Self-Improvement Loop
When you notice a correction, preference, or pattern:
1. Write it to the appropriate file immediately (don't defer to "end of session")
2. If the same correction happens twice, promote it to a rule in `rules/`
3. If a knowledge file grows past ~80 lines, split it

| What You Noticed | Where It Goes |
|---|---|
| User corrected your approach | `rules/` or knowledge file |
| User stated a preference | CLAUDE.md or rules file |
| Project state changed | MEMORY.md |
| Session ended | `state/sessions/` |

## When Blocked
- Try all available tools first (MCP, CLI, API)
- If truly blocked, ask for the specific thing needed with exact instructions
- Once the user says "done", verify and proceed — never say "let me know when ready"
