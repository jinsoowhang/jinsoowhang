# Dofus Touch Current Project Entry Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add `dofus-touch-economy-analytics` as the second Current Projects entry in the GitHub profile README.

**Architecture:** This is a documentation-only change to the existing flat Markdown list. Shell assertions define and verify the exact new line, its adjacency after `skills`, and its uniqueness; project memory and session notes record the decision and verification without introducing new tooling.

**Tech Stack:** Markdown, Bash, ripgrep, Git

---

### Task 1: Add and document the project entry

**Files:**
- Modify: `README.md:6-8`
- Create: `MEMORY.md`
- Create: `notes/Session Notes 2026-08-20.md`
- Modify: `notes/2026-08-20-dofus-touch-current-project-plan.md`

- [ ] **Step 1: Run the failing adjacency assertion**

Run:

```bash
set -euo pipefail
skills_entry='- 🛠️ **[skills](https://github.com/jinsoowhang/skills)** — Custom Claude Code skills (Markdown)'
expected_entry='- 🥚 **[dofus-touch-economy-analytics](https://github.com/jinsoowhang/dofus-touch-economy-analytics)** — Dofus Touch economy analytics (Python, dbt, DuckDB)'
skills_line=$(rg -n -Fx -- "$skills_entry" README.md | cut -d: -f1)
dofus_line=$(rg -n -Fx -- "$expected_entry" README.md | cut -d: -f1)
test -n "$dofus_line" && test "$dofus_line" -eq "$((skills_line + 1))"
```

Expected: FAIL with exit status 1 because the Dofus entry is absent.

- [ ] **Step 2: Add the minimal README entry**

Update the beginning of the Current Projects list in `README.md` to:

```markdown
- 🛠️ **[skills](https://github.com/jinsoowhang/skills)** — Custom Claude Code skills (Markdown)
- 🥚 **[dofus-touch-economy-analytics](https://github.com/jinsoowhang/dofus-touch-economy-analytics)** — Dofus Touch economy analytics (Python, dbt, DuckDB)
- ⚽ **[world-cup-tickets](https://github.com/jinsoowhang/world-cup-tickets)** — FIFA 2026 ticket price tracker (FastAPI, Turso)
```

Do not change the remaining project entries or their relative order.

- [ ] **Step 3: Run the adjacency assertion and exact-count check**

Run:

```bash
set -euo pipefail
skills_entry='- 🛠️ **[skills](https://github.com/jinsoowhang/skills)** — Custom Claude Code skills (Markdown)'
expected_entry='- 🥚 **[dofus-touch-economy-analytics](https://github.com/jinsoowhang/dofus-touch-economy-analytics)** — Dofus Touch economy analytics (Python, dbt, DuckDB)'
skills_line=$(rg -n -Fx -- "$skills_entry" README.md | cut -d: -f1)
dofus_line=$(rg -n -Fx -- "$expected_entry" README.md | cut -d: -f1)
test "$dofus_line" -eq "$((skills_line + 1))"
test "$(rg -Fxc -- "$expected_entry" README.md)" -eq 1
```

Expected: PASS with exit status 0; the Dofus entry is immediately adjacent to `skills` and appears exactly once.

- [ ] **Step 4: Record lasting project context**

Create `MEMORY.md` with:

```markdown
# Project Memory

## Profile project ordering

- The GitHub profile's current projects are maintained as a flat Markdown list in `README.md`.
- `skills` remains first; `dofus-touch-economy-analytics` is second as of 2026-08-20.
- New entries use an emoji, bold repository link, concise description, and parenthesized technology stack.
```

Create `notes/Session Notes 2026-08-20.md` with:

```markdown
# Session Notes 2026-08-20

## Summary

- Added `dofus-touch-economy-analytics` to the GitHub profile's Current Projects list.
- Placed it second, immediately after `skills`.

## Decisions

- Used the concise description "Dofus Touch economy analytics."
- Listed Python, dbt, and DuckDB as the technology stack.
- Used the egg emoji to represent the Dofus-themed project.

## Verification

- Confirmed the entry appears exactly once immediately after `skills`.
- Confirmed the Markdown diff contains no unrelated changes or whitespace errors.
```

- [ ] **Step 5: Run scoped verification**

Run:

```bash
set -euo pipefail
skills_entry='- 🛠️ **[skills](https://github.com/jinsoowhang/skills)** — Custom Claude Code skills (Markdown)'
expected_entry='- 🥚 **[dofus-touch-economy-analytics](https://github.com/jinsoowhang/dofus-touch-economy-analytics)** — Dofus Touch economy analytics (Python, dbt, DuckDB)'
skills_line=$(rg -n -Fx -- "$skills_entry" README.md | cut -d: -f1)
dofus_line=$(rg -n -Fx -- "$expected_entry" README.md | cut -d: -f1)
test "$(rg -Fxc -- "$skills_entry" README.md)" -eq 1
test "$(rg -Fxc -- "$expected_entry" README.md)" -eq 1
test "$dofus_line" -eq "$((skills_line + 1))"

git add README.md MEMORY.md \
  notes/2026-08-20-dofus-touch-current-project-plan.md \
  'notes/Session Notes 2026-08-20.md'
expected_paths=$(printf '%s\n' \
  MEMORY.md \
  README.md \
  notes/2026-08-20-dofus-touch-current-project-plan.md \
  'notes/Session Notes 2026-08-20.md' | sort)
actual_paths=$(git diff --cached --name-only | sort)
test "$actual_paths" = "$expected_paths"
test -z "$(git diff --name-only)"
test -z "$(git ls-files --others --exclude-standard)"
git diff --cached --check
git diff --cached
```

Expected: exit status 0; all four planned files and no others are staged, there are no unstaged or untracked changes, whitespace checks pass, and the full staged diff is reviewed.

- [ ] **Step 6: Commit the implementation atomically**

```bash
git commit -m "Add Dofus Touch to current projects"
```

Expected: one commit containing the README change and its required project notes.
