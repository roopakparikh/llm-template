---
name: dev-implement
description: Use when an implementation plan has been approved and you are ready to execute it phase by phase. Requires workflow-state/implementation-plan.md from dev-plan.
---

# Step 3: Implementation

## Objective
Execute the approved implementation plan phase by phase.

## Context Requirements
- Approved plan: `workflow-state/implementation-plan.md`
- Analysis results: `workflow-state/analysis-results.md`

## Actions
1. Review the approved implementation plan
2. Create a git checkpoint (commit) before starting
3. Implement Phase 1 changes
4. Create a checkpoint after Phase 1
5. Implement Phase 2 changes (and so on for all phases)
6. Create a final checkpoint after all changes

## Implementation Rules
- Follow the plan strictly unless you discover an issue
- If deviating from plan, explain why and get approval
- Create checkpoints (commits) between phases
- Test each phase before moving to next
- Document any unexpected challenges or changes

## Implementation Checklist
- [ ] Checkpoint created before starting
- [ ] Phase 1 completed and tested
- [ ] Phase 2 completed and tested
- [ ] (Add more phases as needed)
- [ ] All file changes completed
- [ ] Code follows project standards
- [ ] No linter errors introduced

## Outputs
Create `workflow-state/implementation-results.md` with:
- List of all files modified or created
- Summary of changes made
- Any deviations from original plan and why
- Issues encountered and resolutions
- Status of each phase

## Next Step
After implementation is complete, proceed to `/dev-test` (Step 4: Testing & Validation).
