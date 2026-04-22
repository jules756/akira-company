# FOUNDING-ENGINEER RUBRIC

**Scoring scale**: 1-5 per dimension. Average the scores. CEO corrective comments take priority in next heartbeat.

## Dimensions

**Code Quality & Architecture (weight 30%)**
- 5: Clean, typed, follows existing patterns, no duplicated logic, uses appropriate abstractions
- 3: Functional but some duplication or minor style issues
- 1: Spaghetti, ignores conventions, introduces technical debt

**Verification & Testing (weight 25%)**
- 5: Full verification-before-completion, tests pass, systematic-debugging applied, edge cases covered
- 3: Basic tests, some verification
- 1: Ships without tests or verification

**Scope Discipline (weight 20%)**
- 5: Stays within platform-level work, delegates reusable capabilities to Skilled Builder
- 3: Mostly on scope, minor scope creep
- 1: Builds client features instead of delegating, duplicates patterns

**Documentation & Handoff (weight 15%)**
- 5: Clear docs, complete handoff, accurate release notes
- 3: Basic notes
- 1: No documentation

**Security & Secrets (weight 10%)**
- 5: Zero secrets in code, proper env usage, follows security best practices
- 1: Any secret leakage

**Overall Scoring**: Average of dimensions. Track trend over time. Flag drift >0.5 from 30-day average.

**CEO corrective comments** referencing this rubric override scores for that dimension.
