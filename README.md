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
bj schedule 3 12              # Schedule task 3 to December (future log)
bj migrate                    # Move incomplete tasks to next working day
bj migrate 1 jan              # Move task 1 to January monthly log
bj goto today                 # Go back to today
bj goto dec 25                # Go to December 25
bj write monthly              # Switch to monthly logs
bj write future               # Switch to future log
```

## Commands

### List Tasks
```bash
bj                            # List tasks in current write mode
bj list DD                    # List daily log for day DD of current month
bj list MONTH                 # List monthly log for MONTH of current year
bj list MONTH YEAR            # List monthly log for MONTH of YEAR
bj list future                # List future log for current year
bj list future YEAR           # List future log for YEAR
```

**Examples:**
```bash
bj                            # List tasks in current mode
bj list 01                    # List daily log for Dec 1st
bj list 15                    # List daily log for Dec 15th
bj list dec                   # List December 2025 monthly log
bj list jan 2026              # List January 2026 monthly log
bj list future                # List 2025 future log
bj list future 2026           # List 2026 future log
```

**List Command Features:**
- View any log without switching write modes
- Month names are case-insensitive (dec, Dec, DEC all work)
- Supports both short (jan, feb, dec) and full month names (january, february, december)
- Day numbers are automatically padded (1 becomes 01)

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
bj goto today                 # Go to today
bj goto DD                    # Go to day DD of current month
bj goto MONTH                 # Go to MONTH of current year (monthly log)
bj goto MONTH DD              # Go to MONTH DD of current year
bj goto MONTH DD YEAR         # Go to MONTH DD of YEAR
```

**Examples:**
```bash
bj goto today                 # Go to today
bj goto 01                    # Go to day 1 of current month
bj goto dec                   # Go to December (monthly log)
bj goto dec 25                # Go to December 25 of current year
bj goto dec 25 2026           # Go to December 25, 2026
```

**Date Navigation Features:**
- Month names are case-insensitive (dec, Dec, DEC all work)
- Supports both short (jan, feb, dec) and full month names (january, february, december)
- Day numbers are automatically padded (1 becomes 01)
- Navigating to a specific date automatically switches to daily mode
- Navigating to a month only (e.g., `bj goto dec`) switches to monthly log mode

### Migrate Tasks
```bash
bj migrate                    # Migrate all incomplete tasks to next working day
bj migrate *                  # Migrate all incomplete tasks to next working day
bj migrate N                  # Migrate task N to next working day
bj migrate N MONTH            # Migrate task N to MONTH monthly log
bj migrate * MONTH YEAR       # Migrate all tasks from current month to MONTH YEAR monthly log
```

**Examples:**
```bash
bj migrate                    # Move all incomplete tasks to next working day
bj migrate '*'                # Move all incomplete tasks to next working day
bj migrate 1                  # Move task 1 to next working day
bj migrate 1 dec              # Move task 1 to December monthly log
bj migrate '*' jan 2026       # Move all December tasks to January 2026 monthly log
```

**Migration behavior:**
- **Working days only**: Automatically skips weekends (Sat & Sun) and holidays
- **Holidays**: Define holidays in `~/bj/.holidays` file (one date per line in YYYY-MM-DD format)
- **Incomplete tasks**: Only migrates tasks with bullets `.` (task), `!` (priority), or `w` (waiting)
- **Original bullets preserved**: Tasks keep their original bullet type when migrated
- **Source marked as migrated**: Original tasks are marked with `>` bullet
- **Already migrated tasks (`>`)**: NOT migrated again
- **Scheduled tasks (`<`)**: NOT migrated since they are already scheduled

**Working with holidays:**
Create a `.holidays` file in your bj directory:
```bash
echo "2025-12-25" >> ~/bj/.holidays  # Christmas
echo "2026-01-01" >> ~/bj/.holidays  # New Year
```

When migrating, the system will skip these dates and find the next working day.

### Schedule Task
```bash
bj schedule [n] [MM|YYYYMM]   # Schedule task n to future log month
```

**Examples:**
```bash
bj schedule 1 12              # Schedule task 1 to December (current year)
                              # Today: <  2025-12: Buy presents
                              # 2025.md [DECEMBER]: .  Buy presents
bj schedule 3 202601          # Schedule task 3 to January 2026
                              # Today: <  2026-01: Project deadline
                              # 2026.md [JANUARY]: .  Project deadline
```

When you schedule a task:
- **Current day:** Task is marked as `<` (scheduled) with YYYY-MM timestamp
  - Format: `<  YYYY-MM: task text`
- **Future log:** Task is added to the month section with original bullet type preserved
  - Format: `[MONTH]` header followed by task with original bullet (`.`, `!`, `w`)
- Original bullet type is preserved (task `.` stays `.`, priority `!` stays `!`, etc.)
- If task is already scheduled (has timestamp), command does nothing

### Monthly Logs
```bash
bj write daily                # Switch to daily log mode
bj write monthly              # Switch to monthly log mode
```

**Examples:**
```bash
bj write monthly              # Switch to monthly logs
bj - Completed project X      # Add note to current month
bj goto dec                   # Navigate to December monthly log
bj write daily                # Switch back to daily mode
```

**Monthly Log Features:**
- Use monthly logs for high-level reflections, goals, or summaries
- Monthly logs are stored separately from daily entries
- All bullet types work the same way (add notes, events, etc.)
- Navigating to a specific date (`bj goto MONTH DD`) automatically switches back to daily mode
- Navigating to a month only (`bj goto MONTH`) switches to monthly log mode
- The mode persists between sessions - use `bj write daily/monthly` to switch

### Future Log
```bash
bj write future               # Switch to future log mode
bj schedule [n] [MM|YYYYMM]   # Schedule tasks to future log
```

**Examples:**
```bash
bj write future               # View current year's future log
bj schedule 2 12              # Schedule task 2 to December
bj schedule 3 202601          # Schedule task 3 to January 2026
```

**Future Log Features:**
- One file per year (YYYY.md) for long-term planning
- Months organized with headers like [JANUARY], [FEBRUARY], etc.
- Schedule tasks to specific months using `bj schedule`
- Tasks preserve their original bullet type when scheduled
- Perfect for planning future events, deadlines, and goals

**Example Future Log Structure (2025.md):**
```markdown
# 2025

[JANUARY]
!  Q1 planning meeting

[FEBRUARY]

[MARCH]
.  File taxes

[DECEMBER]
o  Year-end review
.  Holiday shopping
```

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

# Incomplete task - migrate to tomorrow
bj migrate 2
# Moves "Finish presentation" to next working day
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
- Future logs are stored in: `~/bj/YYYY.md`
- Each day/month/year gets its own file
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

**Example future log file (`~/bj/2025.md`):**
```markdown
# 2025

[JANUARY]
!  Q1 planning

[MARCH]
.  File taxes

[DECEMBER]
o  Year-end review
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
| `bj list DD`           | List daily log for day DD     |
| `bj list MONTH [YEAR]` | List monthly log              |
| `bj list future [YEAR]` | List future log              |
| `bj [text]`            | Add task/note                 |
| `bj [bullet] [text]`   | Add task/note with bullet     |
| `bj [bullet] [n]`      | Change task n to bullet       |
| `bj del [n]`           | Delete task n                 |
| `bj goto today`        | Go to today                   |
| `bj goto DD`           | Go to day DD of current month |
| `bj goto MONTH`        | Go to MONTH (monthly log)     |
| `bj goto MONTH DD [YEAR]` | Go to specific date        |
| `bj migrate [*\|N] [MONTH] [YEAR]` | Migrate tasks to next working day or monthly log |
| `bj schedule [n] [MM\|YYYYMM]` | Schedule task to future log month |
| `bj write daily`       | Switch to daily log mode      |
| `bj write monthly`     | Switch to monthly log mode    |
| `bj write future`      | Switch to future log mode     |

---

**Version**: 1.0.0
**License**: MIT
**Author**: Suwat Saisema
