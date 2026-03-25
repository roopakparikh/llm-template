---
name: dev-orchestrate
description: Use when given a development task that needs the full 5-step workflow: analyze, plan, implement, test, and document. Orchestrates all steps sequentially with approval gates.
---

# Dev Workflow Orchestrator

## Overview
Orchestrates a complete 5-step development workflow from analysis to documentation.

## Usage
```
/dev-orchestrate <task description>
```
Example: `/dev-orchestrate Add user authentication with Google OAuth`

## Workflow Steps

Run each step fully before proceeding to the next.

### Step 1: Analysis (`/dev-requirements`)
- Analyze requirements and current codebase
- Identify affected files, dependencies, and risks
- Save results to `workflow-state/analysis-results.md`

### Step 2: Planning (`/dev-plan`)
- Create phase-by-phase implementation plan from analysis
- Present plan to the user
- **APPROVAL GATE:** Wait for user approval before proceeding

### Step 3: Implementation (`/dev-implement`)
- Create a git checkpoint before starting
- Execute plan phase by phase, checkpointing between phases
- Save results to `workflow-state/implementation-results.md`

### Step 4: Testing (`/dev-test`)
- Run existing and new tests
- Verify all requirements are met
- Save results to `workflow-state/test-results.md`
- If tests fail: ask user whether to fix or roll back

### Step 5: Documentation (`/dev-document`)
- Update docs, README, and changelog as needed
- Create `workflow-state/workflow-summary.md`
- Present final summary to user

## Execution Rules
1. **Sequential:** Complete each step fully before proceeding
2. **State:** Save intermediate results to `workflow-state/`
3. **Checkpoints:** Create git commits before and after major changes
4. **Approval gates:** Stop and wait at critical points
5. **Error handling:** If a step fails, stop and ask the user how to proceed

## Workflow State Files
| File | Created By |
|------|-----------|
| `workflow-state/analysis-results.md` | Step 1 |
| `workflow-state/implementation-plan.md` | Step 2 |
| `workflow-state/implementation-results.md` | Step 3 |
| `workflow-state/test-results.md` | Step 4 |
| `workflow-state/workflow-summary.md` | Step 5 |
