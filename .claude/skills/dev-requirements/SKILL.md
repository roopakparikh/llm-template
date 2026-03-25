---
name: dev-requirements
description: Use when starting any development task and needing to analyze requirements and the current codebase before planning or implementing anything.
---

# Step 1: Analyze Requirements

## Objective
Analyze the project requirements and current codebase state to understand what needs to be done.

## Context Requirements
- Access to project documentation
- Current codebase structure
- User requirements or feature requests

## Actions
1. Review relevant documentation (or specified files)
2. Analyze existing codebase structure
3. Identify files and components that will be affected
4. Pay special attention to test, CLI & UI in addition to the actual feature api/backend planning
5. List dependencies and constraints
6. Identify potential risks or challenges

## Analysis Checklist
- [ ] Requirements clearly understood
- [ ] Affected files identified
- [ ] Dependencies mapped
- [ ] Risks documented
- [ ] Questions for clarification noted

## Outputs
Create `workflow-state/analysis-results.md` with:
- Summary of requirements
- List of files to modify or create
- Dependency analysis
- Risk assessment
- Clarifying questions (if any)

## Next Step
After analysis is complete, proceed to `/dev-plan` (Step 2: Planning).
