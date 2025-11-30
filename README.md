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
bj migrate                    # Move incomplete tasks to tomorrow
bj date today                 # Go back to today
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

**Examples:**
```bash
bj x 1                        # Mark task 1 as done
bj a 2                        # Mark task 2 as abandoned
bj > 3                        # Mark task 3 as migrated
```

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
bj migrate                    # Migrate incomplete tasks to tomorrow
```

**Example:**
```bash
bj migrate                    # Move incomplete tasks to tomorrow
```

When you migrate tasks, incomplete tasks (`.`, `!`, `w`, `<`) are:
- Changed to `>` (migrated) in the current day
- Copied as `>` to tomorrow

### Schedule Task
```bash
bj schedule [n] [date]        # Schedule task n to specific date
```

**Examples:**
```bash
bj schedule 1 1225            # Schedule task 1 to Dec 25
bj schedule 3 20250315        # Schedule task 3 to Mar 15, 2025
```

When you schedule a task:
- Task is marked as `<` (scheduled) in the current day
- Task is copied as `<` to the target date

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

# Move incomplete to tomorrow
bj > 2
bj > 4
```

### Task Management
```bash
# Add various tasks
bj Write documentation
bj o Lunch with team 12pm
bj ! Review pull request
bj - Check meeting notes from yesterday

# Change priority
bj ! 1                        # Make task 1 priority

# Mark some done
bj x 2                        # Lunch done
bj x 3                        # PR reviewed

# Abandon task
bj a 4                        # Not needed anymore

# Delete task
bj del 4                      # Remove from list
```

## File Storage

- Tasks are stored in: `~/bj/YYYY-MM-DD.md`
- Each day gets its own file
- Files are plain markdown - easy to read/edit manually

**Example file content (`~/bj/2025-11-30.md`):**
```markdown
# 2025-11-30

.  Buy groceries
!  Fix critical bug
o  Team meeting 3pm
-  Remember to call mom
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
| `bj`                   | List tasks                    |
| `bj [text]`            | Add task                      |
| `bj [bullet] [text]`   | Add task with bullet          |
| `bj [bullet] [n]`      | Change task n to bullet       |
| `bj del [n]`           | Delete task n                 |
| `bj date today`        | Go to today                   |
| `bj date [date]`       | Go to specific date           |
| `bj migrate`           | Migrate tasks to tomorrow     |
| `bj schedule [n] [date]` | Schedule task n to date     |

---

**Version**: 1.0.0
**License**: MIT
**Author**: Suwat Saisema
