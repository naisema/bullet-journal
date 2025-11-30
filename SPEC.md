Bullet Journal Specification

Here is a clean, minimal **User Story + Acceptance Criteria** for a Bullet-Journal system using your *Rapid Key* spec.

---

# **User Story: Bullet Journal with Rapid Logging Keys**

**As a user**,
I want to create and manage daily bullet-journal entries
so that I can quickly record tasks, events, and priorities using Rapid Logging symbols.

---

# **Feature Description**

The system must support the following **Rapid Keys**:

| Key | Meaning                         |
| --- | ------------------------------- |
| `.` | Task                            |
| `x` | Task done                       |
| `>` | Task migrated                   |
| `<` | Task scheduled                  |
| `o` | Event                           |
| `a` | Abandoned task                  |
| `w` | Waiting for someone / something |
| `!` | Priority                        |

---

# **Acceptance Criteria**

### **AC1 — User can create bullet-journal entries**

* User can enter a new line starting with any supported Rapid Key.
* The line format must be:
  `<symbol>  <text>`
  Example: `.  Check yesterday's list`
* System must validate that symbol is one of the Rapid Keys.

---

### **AC2 — User can mark tasks as completed**

* When a user changes a task to `x`,
  the system must update its state from “task” to “done”.
* Example:
  `.  Update README.md` → `x  Update README.md`

---

### **AC3 — User can migrate or schedule tasks**

* User can convert a task using:

  * `>` for migrated tasks
  * `<` for scheduled tasks
* System must preserve the original text when updating the symbol.

---

### **AC4 — Events can be logged**

* User can create an entry starting with `o`.
* Date formats must be allowed inside text.
  Example:
  `o  2024-08-10: Peter's 23rd birthday`

---

### **AC5 — User can log priority items**

* User can mark important items using `!` at the beginning.
* Priority items must sort to the top when the list is displayed (optional configuration).

---

### **AC6 — User can mark abandoned tasks**

* User can log a task as abandoned using `a`.
* Abandoned tasks remain in the daily log for history.

---

### **AC7 — Waiting-for items**

* User can log dependencies using `w`.
* Format example:
  `w  Mary: call about party`
* System must display these in a separate “Waiting For” section (optional).

---

### **AC8 — Daily List Rendering**

Given a daily log like:

```
1130 SUN
.  Check yesterday's list
x  [1] Update README.md for bulletjournal.md
!  Ask Uncle Ben about present for Peter
o  2024-08-10: Peter's 23rd birthday
a  Wash car
w  Mary: call about party
```

The system must render each line according to symbol meaning.

---

# **Output Format (for your app in Beepy)**

Every entry must contain:

* `symbol` (1 character)
* `body text` (string)
* `timestamp` (optional)
* `status` (derived from symbol)

---
