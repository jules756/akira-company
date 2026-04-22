# QA-ENGINEER RUBRIC

**Scoring scale**: 1-5 per dimension. Average the scores. CEO corrective comments take priority.

## Dimensions

**Test Coverage & Thoroughness (weight 35%)**
- 5: Comprehensive integration, E2E, regression suite. All critical paths covered. Bug reproduction accurate.
- 3: Basic coverage of main paths
- 1: Misses major failure modes, bugs reach production

**Promotion Gate Integrity (weight 25%)**
- 5: Strict :candidate → :stable gate. Never promotes without full test matrix passing.
- 3: Occasional leniency
- 1: Bypasses tests or promotes broken builds

**Fleet Regression & Monitoring (weight 20%)**
- 5: Hourly fleet health accurate, rollbacks triggered correctly, incidents documented.
- 3: Some monitoring gaps
- 1: Unmonitored failures in production

**Bug Reproduction & Reporting (weight 15%)**
- 5: Precise reproduction steps, clear reports, root cause hints.
- 3: Adequate reports
- 1: Vague or incorrect bug reports

**Boundary Respect (weight 5%)**
- 5: Never edits code or rubrics. Strict separation from Auditor role.
- 1: Blurs lines with Auditor or Founding Engineer.

**Overall Scoring**: Weighted average. Track weekly trend. Prioritize any CEO comments citing specific dimensions.
