# LLM Template

A collection of Claude Code skills for a structured, multi-step development workflow. Clone this repo and point Claude Code at it to get a consistent, repeatable process for going from requirements to shipped code.

## What Are Skills?

Skills are prompt files that Claude Code loads on demand via the `Skill` tool. They live in `.claude/skills/` and define structured workflows — checklists, approval gates, state files — that guide Claude through complex tasks without needing to re-explain your process every session.

## Setup

1. Clone this repo
2. Open it in Claude Code (`claude` in the project directory)
3. Invoke skills using the `/skill-name` syntax in your prompt

Skills are auto-discovered from `.claude/skills/`. No additional configuration needed.

## Skills

### `/dev-orchestrate` — Full workflow, one command

The entry point for any non-trivial development task. Runs all five steps below in sequence with approval gates between them.

```
/dev-orchestrate Add user authentication with Google OAuth
```

**Steps it runs:**
1. Analyze requirements (`/dev-requirements`)
2. Create implementation plan (`/dev-plan`) — pauses for your approval
3. Implement phase by phase (`/dev-implement`)
4. Test and validate (`/dev-test`)
5. Document changes (`/dev-document`)

**State files** are saved to `workflow-state/` as each step completes, so you can resume or inspect mid-workflow.

---

### `/dev-requirements` — Step 1: Analyze

Explores the codebase, maps affected files and dependencies, identifies risks, and flags any ambiguous requirements before any planning begins.

**Output:** `workflow-state/analysis-results.md`

---

### `/dev-plan` — Step 2: Plan

Reads the analysis results and produces a phase-by-phase implementation plan with specific file changes, test strategy, and rollback considerations. Presents the plan and waits for your approval before anything is written.

**Requires:** `workflow-state/analysis-results.md`
**Output:** `workflow-state/implementation-plan.md`

---

### `/dev-implement` — Step 3: Implement

Executes the approved plan one phase at a time. Creates a git checkpoint before starting, commits between phases, and documents any deviations.

**Requires:** `workflow-state/implementation-plan.md`
**Output:** `workflow-state/implementation-results.md`

---

### `/dev-test` — Step 4: Test

Runs existing test suites, writes new tests for new functionality, checks linting, and verifies all original requirements are met. If tests fail, asks whether to fix or roll back.

**Requires:** `workflow-state/implementation-results.md`
**Output:** `workflow-state/test-results.md`

---

### `/dev-document` — Step 5: Document

Updates inline docs, README, API docs, and changelog. Produces a final `workflow-summary.md` covering what changed, why, and how to use the new functionality.

**Requires:** All prior `workflow-state/` files
**Output:** `workflow-state/workflow-summary.md` + updated project docs

---

### `/fix-github-issue` — Autonomous issue fix

End-to-end workflow for fixing a GitHub issue: fetch → analyze → plan → implement autonomously → open PR.

```
/fix-github-issue 142
/fix-github-issue https://github.com/org/repo/issues/142
```

**How it works:**
1. Fetches the issue with `gh issue view`
2. Summarizes root cause, acceptance criteria, affected files, and risks
3. Creates an isolated git branch via a worktree
4. Writes a TDD-based fix plan to `docs/superpowers/plans/issue-N-fix.md`
5. Runs an autonomous implementation loop until tests pass
6. Opens a pull request with a structured description

**Prerequisites:**
- `gh` CLI authenticated to the repo
- `/ralph-loop` command available (from the ralph-loop plugin)
- A working test suite

**Stops and asks** if the issue has no clear acceptance criteria before proceeding.

---

## Workflow State Files

| File | Created by |
|------|-----------|
| `workflow-state/analysis-results.md` | `/dev-requirements` |
| `workflow-state/implementation-plan.md` | `/dev-plan` |
| `workflow-state/implementation-results.md` | `/dev-implement` |
| `workflow-state/test-results.md` | `/dev-test` |
| `workflow-state/workflow-summary.md` | `/dev-document` |

These files are intentionally ephemeral — they capture the state of one workflow run. You can delete them between tasks or keep them for reference.

## When to Use Each Skill

| Situation | Skill |
|-----------|-------|
| New feature or bug fix end-to-end | `/dev-orchestrate` |
| Just need to understand scope first | `/dev-requirements` |
| Analysis done, need a plan | `/dev-plan` |
| Plan approved, ready to code | `/dev-implement` |
| Code done, need to verify | `/dev-test` |
| Everything works, need to document | `/dev-document` |
| GitHub issue to close | `/fix-github-issue` |
