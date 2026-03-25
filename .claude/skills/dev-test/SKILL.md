---
name: dev-test
description: Use when implementation is complete and you need to validate the changes through automated and manual testing. Requires workflow-state/implementation-results.md from dev-implement.
---

# Step 4: Test & Validation

## Objective
Validate the implementation through testing and verification.

## Context Requirements
- Implementation results: `workflow-state/implementation-results.md`
- Implementation plan: `workflow-state/implementation-plan.md`
- Modified files from Step 3

## Actions
1. Review all files modified in Step 3
2. Run existing test suites
3. Create or update tests for new functionality
4. Perform manual testing checklist
5. Check for linter errors
6. Verify all requirements from Step 1 are met
7. Check for edge cases and error handling

## Testing Checklist
- [ ] All existing tests pass
- [ ] New tests created for new functionality
- [ ] New tests pass
- [ ] No linter errors
- [ ] Edge cases handled
- [ ] Error handling implemented
- [ ] All original requirements satisfied
- [ ] Performance is acceptable

## Outputs
Create `workflow-state/test-results.md` with:
- Test execution summary
- List of tests added or modified
- Any issues found and their status
- Coverage report (if applicable)
- Validation checklist status

## Next Step
- If tests **pass**: proceed to `/dev-document` (Step 5: Documentation)
- If tests **fail**: return to `/dev-implement` with specific failures to fix
