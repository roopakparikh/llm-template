---
name: fix-github-issue
description: Use when given a GitHub issue URL or number to fetch, analyze, plan, and autonomously implement a fix using ralph loop, then create a PR
---

# Fix GitHub Issue

## Overview

End-to-end autonomous workflow: fetch issue → analyze → plan → ralph loop → PR.

**Announce at start:** "I'm using the fix-github-issue skill."

**Core principle:** The issue defines the requirements. Skip full brainstorming — analyze inline, write a bite-sized TDD plan, then let the ralph loop iterate until the fix is complete and verified.

## Prerequisites

- `gh` CLI authenticated to the repo
- Ralph loop plugin installed (`/ralph-loop` command available)
- Working git repository with tests

## The Process

### Step 1: Fetch and Analyze Issue

```bash
gh issue view <number-or-url> --json title,body,labels,comments,assignees
```

Extract and summarize in 3-5 bullets:
- **Root cause** — what is broken or missing?
- **Acceptance criteria** — how do we know it's fixed?
- **Affected files/components** — scope the work
- **Edge cases or risks** — what could go wrong?

**If the issue has no clear acceptance criteria, STOP and ask before continuing.**

### Step 2: Create Isolated Workspace

**REQUIRED SUB-SKILL:** Use `superpowers:using-git-worktrees`

Branch name: `fix/issue-<number>-<short-slug>`

### Step 3: Write Inline Plan

Save to `docs/superpowers/plans/issue-<number>-fix.md`:

```markdown
# Fix: <issue title>

**Issue:** #<number> | **Branch:** fix/issue-<number>-<slug>
**Goal:** <one sentence>
**Test command:** <exact command to run the test suite>

### Task 1: <component>
**Files:** `path/to/file`, `path/to/file_test`

- [ ] Write failing test for <specific behavior>
- [ ] Run: `<test command>` — expect FAIL
- [ ] Implement minimal fix
- [ ] Run: `<test command>` — expect PASS
- [ ] `git commit -m "fix: <description>"`

### Task 2: ...
```

Rules: test first, 2-5 min steps, match existing code patterns.

### Step 4: Launch Ralph Loop

Construct the prompt by substituting actual values, then run:

```
/ralph-loop "Fix GitHub issue NUMBER: TITLE

## What needs fixing
PASTE 2-3 SENTENCE ISSUE ANALYSIS HERE

## Acceptance criteria
- BULLET 1
- BULLET 2

## Implementation plan
PASTE FULL PLAN CONTENT HERE (all tasks with steps)

## How to verify
Run: TEST_COMMAND
All tests must pass with zero failures.

## If blocked after many iterations
Create file issue-NUMBER-blockers.md documenting:
- What was attempted
- What failed and why
- Suggested next steps

## Done when ALL of these are true
1. All plan task checkboxes are checked off
2. TEST_COMMAND exits with code 0
3. Changes are committed with descriptive messages

When all of the above are verified true, output exactly:
<promise>ISSUE NUMBER FIXED AND TESTS PASSING</promise>" \
  --completion-promise "ISSUE NUMBER FIXED AND TESTS PASSING" \
  --max-iterations 20
```

**Note:** Replace `NUMBER` with the actual issue number (no `#` symbol — shells interpret `#` as a comment).

**If the ralph loop exits without completing:**
- Check `issue-NUMBER-blockers.md` if it was created
- Run `git log --oneline` to see what was committed
- Fix blockers manually or switch to `superpowers:subagent-driven-development` for the remaining tasks

### Step 5: Create Pull Request

```bash
git push -u origin fix/issue-<number>-<slug>

gh pr create \
  --title "fix: <issue title> (#<number>)" \
  --body "$(cat <<'EOF'
## Summary
<2-3 bullets of what changed and why>

## Changes
- `path/to/file`: <what changed>

## Test Plan
- [ ] Existing tests pass
- [ ] New tests cover the fix

Closes #<number>
EOF
)"
```

## Quick Reference

| Step | Action | Tool |
|------|--------|------|
| 1 | Fetch + analyze | `gh issue view --json` |
| 2 | Isolated branch | `superpowers:using-git-worktrees` |
| 3 | Write TDD plan | `docs/superpowers/plans/issue-N-fix.md` |
| 4 | Autonomous fix | `/ralph-loop` |
| 5 | PR | `gh pr create` |

## Ralph Loop Prompt — Required Elements

Every ralph loop prompt for this workflow must include all six:

1. **Issue context** — title + what's broken + acceptance criteria
2. **Full plan** — all tasks with TDD steps embedded
3. **Test command** — exact command to verify correctness
4. **Escape hatch** — what to write if stuck, so loop exits cleanly
5. **Completion promise** — exact text to output only when verified done
6. **`--max-iterations 20`** — always set a limit

## Common Mistakes

**No plan in the prompt**
- Problem: Ralph iterates aimlessly, implements wrong things
- Fix: Embed the full task list with TDD steps in the prompt

**`#` in the completion promise**
- Problem: Shell treats `#` as comment start, truncating the promise text
- Fix: Write promise as `ISSUE 123 FIXED AND TESTS PASSING` (no `#`)

**Vague completion criteria**
- Problem: Loop exits too early or never exits
- Fix: Require explicit test command success in the done criteria

**No `--max-iterations`**
- Problem: Loop runs indefinitely on ambiguous or impossible tasks
- Fix: Always set `--max-iterations 20` or lower

**Ambiguous issue skipped**
- Problem: Ralph implements the wrong thing
- Fix: STOP at Step 1 if acceptance criteria are unclear. Ask before planning.

## Integration

**Required:**
- `superpowers:using-git-worktrees` — branch isolation
- `/ralph-loop` command — autonomous iterative implementation

**Fallback (if ralph loop gets blocked):**
- `superpowers:subagent-driven-development` — structured per-task execution with spec + quality reviews

**Not used (intentionally skipped):**
- `superpowers:brainstorming` — the issue already defines requirements
- `superpowers:writing-plans` — plan is written inline, not as a full design cycle
