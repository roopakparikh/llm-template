---
name: dev-document
description: Use when implementation and testing are complete and you need to document all changes, decisions, and produce a final workflow summary. Requires all prior workflow-state files.
---

# Step 5: Documentation

## Objective
Document all changes, decisions, and usage for future reference.

## Context Requirements
- Analysis results: `workflow-state/analysis-results.md`
- Implementation plan: `workflow-state/implementation-plan.md`
- Implementation results: `workflow-state/implementation-results.md`
- Test results: `workflow-state/test-results.md`
- All modified files

## Actions
1. Update inline code documentation (comments, docstrings, etc.)
2. Update README if necessary
3. Update API documentation
4. Create or update user guides if needed
5. Document architectural decisions
6. Update changelog
7. Create summary of workflow execution

## Documentation Checklist
- [ ] Code comments updated
- [ ] Function and class documentation complete
- [ ] README updated (if needed)
- [ ] API documentation current
- [ ] Changelog updated
- [ ] Architectural decisions documented

## Outputs
1. Updated documentation files
2. `workflow-state/workflow-summary.md` with:
   - Overview of what was accomplished
   - All files changed
   - Key decisions made
   - Tests added
   - How to use new functionality
   - Future considerations

## Workflow Complete
This is the final step. Review `workflow-state/workflow-summary.md` and ensure all objectives are met.
