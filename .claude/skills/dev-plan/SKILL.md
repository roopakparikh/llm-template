---
name: dev-plan
description: Use when analysis is complete and you need to create a detailed implementation plan before writing any code. Requires workflow-state/analysis-results.md from dev-requirements.
---

# Step 2: Create Implementation Plan

## Objective
Based on the analysis, create a detailed implementation plan with specific steps and file changes.

## Context Requirements
- Results from Step 1: `workflow-state/analysis-results.md`
- Current codebase state

## Actions
1. Review analysis results from previous step
2. Break down implementation into logical phases
3. Define specific file changes for each phase
4. Identify test requirements
5. Estimate complexity and potential issues
6. Create checkpoint plan (where to save state)

## Planning Checklist
- [ ] Implementation broken into clear phases
- [ ] Each phase has specific file changes defined
- [ ] Test strategy defined
- [ ] Rollback strategy considered
- [ ] Dependencies ordered correctly

## Outputs
Create `workflow-state/implementation-plan.md` with:
- Phase-by-phase implementation plan
- Specific files to modify in each phase
- Code structure and architecture decisions
- Test cases to implement
- Checkpoint strategy

## Approval Gate
**STOP HERE** — present the plan to the user for approval before proceeding to Step 3.

## Next Step
After plan approval, proceed to `/dev-implement` (Step 3: Implementation).
