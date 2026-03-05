# Task Rules

## Source of Truth
- **SQLite** (`~/.claude/tasks.db`) is the primary task store. ALL writes go through SQLite.
- **backlog.md** (`~/.claude/state/backlog.md`) is a read-only export. NEVER edit it directly.
- After any SQLite write, re-export: `python3 ~/.claude/scripts/db.py export`
- Use `/tasks` skill for all task operations.

## Before Substantive Work
When the user requests work that will take more than ~5 minutes:
1. Check active tasks: does this map to an active P1 or P2 task?
   - **YES** → proceed, reference the task ID
   - **NO** → flag: "This doesn't map to an active task. Add it or continue?"
2. If work touches zero problems → flag: "Not connected to any of your problems. Proceed?"

## When NOT to Check
- Quick questions, lookups, debugging
- Tasks under ~5 minutes
- User says "just do it" or explicitly overrides
- System maintenance (git, cleanup, updates)

## After Completing a Task
1. Update in SQLite (status='done', completed_at)
2. Re-export: `python3 ~/.claude/scripts/db.py export`
3. Update MEMORY.md Active Tasks table
4. Suggest the next task based on priority

## Task Conventions
- **IDs:** `T` + 3-digit counter, monotonic, never reused
- **Priority:** P1 (now), P2 (this week), P3 (this month), BL (backlog)
- **Effort:** S (< 1 session), M (2-3 sessions), L (4+ sessions)
- **Status:** pending, in_progress, done, blocked
- **Problems:** comma-separated numbers (1-12) referencing the 12 Favorite Problems
