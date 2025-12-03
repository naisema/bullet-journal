# bj - Bullet Journal CLI

A minimal bullet journal for the command line.

## Installation

```bash
chmod +x bj
# Optional: add to PATH
cp bj $HOME/.local/bin/
```

## Quick Start

```bash
bj Buy groceries              # Add a task
bj                            # List all tasks
bj x 1                        # Mark task 1 as done
bj del 2                      # Delete task 2
bj schedule 3 1225            # Schedule task 3 to Dec 25
bj migrate                    # Move all incomplete tasks to next month
bj date today                 # Go back to today
bj write monthly              # Switch to monthly logs
bj month 12                   # Navigate to December
```

## Commands

### List Tasks
```bash
bj                            # Show today's tasks
```

### Add Task
```bash
bj [text]                     # Add task with default bullet (.)
bj [bullet] [text]            # Add task with specific bullet
```

**Valid bullets for adding tasks:**
- `.` (task), `!` (priority), `o` (event), `-` (note)

**Note:** Status bullets like `x` (done), `a` (abandoned), `w` (waiting) can only be used to change existing tasks, not to add new ones.

**Examples:**
```bash
bj Buy milk                   # Add: .  Buy milk
bj . Write report             # Add: .  Write report
bj o Team meeting 3pm         # Add: o  Team meeting 3pm
bj ! Fix critical bug         # Add: !  Fix critical bug
bj - Remember to call mom     # Add: -  Remember to call mom
```

### Change Task Status
```bash
bj [bullet] [number]          # Change task number to new bullet
```

**Valid bullets for changing status:**
- `!` (priority), `x` (done), `o` (event), `a` (abandoned), `w` (waiting), `-` (note)

**Note:** Bullets `.` (task), `>` (migrated), and `<` (scheduled) cannot be manually set. Migrated and scheduled bullets are system-managed through `bj migrate` and `bj schedule` commands.

**Examples:**
```bash
bj x 1                        # Mark task 1 as done (asks for confirmation)
bj a 2                        # Mark task 2 as abandoned
bj w 3                        # Mark task 3 as waiting
bj ! 4                        # Mark task 4 as priority
bj o 5                        # Mark task 5 as event
bj - 6                        # Mark task 6 as note
```

**Note:** Changing task status requires confirmation to prevent accidental changes. You'll see a preview and must confirm with 'y' to proceed.

### Delete Task
```bash
bj del [number]               # Delete task by number
```

**Example:**
```bash
bj del 1                      # Delete task 1 (asks for confirmation)
```

**Note:** Deletion requires confirmation to prevent accidental data loss.

### Date Navigation
```bash
bj date today                 # Go to today
bj date [date]                # Go to specific date
```

**Examples:**
```bash
bj date today                 # Back to today
bj date 1225                  # Go to December 25 of current year
bj date 20250315              # Go to March 15, 2025
```

### Migrate Tasks
```bash
bj migrate                    # Migrate incomplete tasks to next month
```

**Example:**
```bash
bj migrate                    # Move all incomplete tasks from current month to next month
```

When you migrate tasks, all incomplete tasks (`.`, `!`, `w`) from ALL daily logs in the current month are:
- Changed to `>` (migrated) in their original day files
- Copied as `>` to next month's monthly log file (YYYY-MM.md)

**Migration behavior:**
- Collects incomplete tasks from all days in the current month (e.g., all 2025-12-*.md files)
- Migrates them to next month's monthly log (e.g., 2026-01.md)
- Already migrated tasks (`>`) are NOT migrated again
- Scheduled tasks (`<`) are NOT migrated since they are already scheduled for a specific date

### Schedule Task
```bash
bj schedule [n] [date]        # Schedule task n to specific date
```

**Examples:**
```bash
bj schedule 1 1225            # Schedule task 1 to Dec 25
                              # Today: <  2025-12-25: Buy presents
                              # Dec 25: .  Buy presents
bj schedule 3 20250315        # Schedule task 3 to Mar 15, 2025
bj schedule 1 0101            # If task 1 already scheduled, does nothing
```

When you schedule a task:
- **Current day:** Task is marked as `<` (scheduled) with timestamp prefix
  - Format: `<  YYYY-MM-DD: task text`
- **Target date:** Task becomes regular task `.` without timestamp
  - Format: `.  task text`
- If task is already scheduled (has timestamp), command does nothing

### Monthly Logs
```bash
bj write daily                # Switch to daily log mode
bj write monthly              # Switch to monthly log mode
bj month [month]              # Navigate to specific month
```

**Examples:**
```bash
bj write monthly              # Switch to monthly logs
bj - Completed project X      # Add note to current month
bj month 12                   # Navigate to December (current year)
bj month 202601               # Navigate to January 2026
bj write daily                # Switch back to daily mode
```

**Monthly Log Features:**
- Use monthly logs for high-level reflections, goals, or summaries
- Monthly logs are stored separately from daily entries
- All bullet types work the same way (add notes, events, etc.)
- Navigating to a specific date (`bj date`) automatically switches back to daily mode
- The mode persists between sessions - use `bj write daily/monthly` to switch

## Bullet Types

| Bullet | Meaning    | Description                    |
|--------|-----------|--------------------------------|
| `.`    | Task      | Todo item                      |
| `!`    | Priority  | Important task                 |
| `x`    | Done      | Completed task                 |
| `o`    | Event     | Meeting, appointment           |
| `a`    | Abandoned | Cancelled or dropped           |
| `w`    | Waiting   | Waiting on someone/something   |
| `>`    | Migrated  | Moved to another day           |
| `<`    | Scheduled | Scheduled for future           |
| `-`    | Note      | Just a note                    |

**Bullet Usage:**
- **For adding:** `.` `!` `o` `-`
- **For changing:** `!` `x` `o` `a` `w` `-`
- **System-managed:** `>` (via `bj migrate`), `<` (via `bj schedule`)

## Usage Examples

### Daily Workflow
```bash
# Morning - add tasks
bj Check email
bj ! Finish presentation
bj o Standup meeting 10am

# List tasks
bj
# Output:
#  1122 FRI
#  .  Check email
#  !  Finish presentation
#  o  Standup meeting 10am

# Mark tasks as done
bj x 1                        # Email checked
bj x 3                        # Meeting attended

# Add more tasks
bj Call client

# End of day - check status
bj
# Output:
#  1122 FRI
#  x  Check email
#  !  Finish presentation
#  x  Standup meeting 10am
#  .  Call client

# End of month - migrate incomplete tasks
bj migrate
# Collects all incomplete tasks from current month and moves to next month's log
```

### Task Management
```bash
# Add various tasks
bj Write documentation
bj o Lunch with team 12pm
bj ! Review pull request
bj - Check meeting notes from yesterday

# Change priority (requires confirmation)
bj ! 1                        # Make task 1 priority

# Mark some done (requires confirmation)
bj x 2                        # Lunch done
bj x 3                        # PR reviewed

# Abandon task (requires confirmation)
bj a 4                        # Not needed anymore

# Delete task
bj del 4                      # Remove from list
```

## File Storage

- Daily tasks are stored in: `~/bj/YYYY-MM-DD.md`
- Monthly logs are stored in: `~/bj/YYYY-MM.md`
- Each day/month gets its own file
- Files are plain markdown - easy to read/edit manually

**Example daily file (`~/bj/2025-11-30.md`):**
```markdown
# 2025-11-30

.  Buy groceries
!  Fix critical bug
o  Team meeting 3pm
-  Remember to call mom
```

**Example monthly file (`~/bj/2025-12.md`):**
```markdown
# 2025-12

-  Completed major project refactor
-  Team grew to 5 members
o  Holiday party on Dec 20
```

## Configuration

Set custom directory (optional):
```bash
export BJ_HOME=/path/to/journal
```

## Design Philosophy

**Minimal**: Only essential commands
**Fast**: Quick to type, quick to run
**Portable**: Works on macOS and Linux
**Simple**: Plain text files, no database
**Flexible**: Edit files manually if needed

## Command Summary

| Command                | Action                        |
|------------------------|-------------------------------|
| `bj`                   | List tasks/logs (current mode)|
| `bj [text]`            | Add task/note                 |
| `bj [bullet] [text]`   | Add task/note with bullet     |
| `bj [bullet] [n]`      | Change task n to bullet       |
| `bj del [n]`           | Delete task n                 |
| `bj date today`        | Go to today                   |
| `bj date [date]`       | Go to specific date           |
| `bj migrate`           | Migrate all incomplete tasks to next month |
| `bj schedule [n] [date]` | Schedule task n to date     |
| `bj write daily`       | Switch to daily log mode      |
| `bj write monthly`     | Switch to monthly log mode    |
| `bj month [month]`     | Navigate to specific month    |

---

**Version**: 1.0.0
**License**: MIT
**Author**: Suwat Saisema
