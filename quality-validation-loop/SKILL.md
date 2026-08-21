---
name: quality-validation-loop
description: Use when a user asks for continuous, codebase-wide feature discovery, quality validation, defect remediation, and regression testing with a canonical feature and test inventory.
---

# Quality Validation Loop

Operate as a continuous software quality and validation agent.

PHASE 1: FEATURE DISCOVERY

1. Analyse the entire codebase.

2. Identify every user-facing feature, workflow, screen, API interaction, configuration option, and business process.

3. For each feature:

   - Create a unique Feature ID.
   - Create a detailed user story.
   - Define expected behaviour based solely on actual code implementation.
   - Document edge cases.
   - Document validation rules.
   - Document dependencies.
   - Document known assumptions.

4. Maintain a single canonical spreadsheet (the source of truth) containing:

   Feature ID
   Feature Name
   User Story
   Expected Behaviour
   Edge Cases
   Test Cases
   Current Status
   Defect Count
   Severity
   Notes
   Last Tested Date

5. Continue discovery until no undocumented feature remains.

Exit Criteria:

- Every identifiable feature in the codebase exists in the spreadsheet.
- No screens, routes, workflows, or APIs remain undocumented.

---

PHASE 2: TEST GENERATION

For every feature:

1. Generate comprehensive test scenarios:

   - Happy path
   - Error path
   - Boundary conditions
   - Invalid input
   - Permission/security cases
   - Performance considerations
   - Mobile/responsive behaviour (if applicable)

2. Add all test cases to the spreadsheet.

Exit Criteria:

- Every feature has at least one complete test suite.
- All major user journeys are covered end-to-end.

---

PHASE 3: EXECUTION

Execute every test case.

For every failure:

1. Record:

   - Defect ID
   - Feature ID
   - Reproduction steps
   - Expected result
   - Actual result
   - Severity
   - Root cause hypothesis

2. Update spreadsheet immediately.

Exit Criteria:

- Every test case executed.
- Every defect documented.

---

PHASE 4: REMEDIATION

For each defect:

1. Investigate root cause.
2. Implement the smallest safe fix.
3. Verify fix locally.
4. Update defect status.

Focus on:

- Functional defects
- UX friction
- Workflow inconsistencies
- Navigation issues
- Validation errors
- Accessibility issues
- Error messaging
- Data integrity issues
- Performance bottlenecks

Exit Criteria:

- All defects resolved or explicitly waived.

---

PHASE 5: REGRESSION TESTING

1. Re-run every test case.
2. Re-run all major end-to-end user journeys.
3. Verify no regressions were introduced.
4. Update spreadsheet.

Exit Criteria:

- All tests pass.
- No open critical defects.
- No open high-severity defects.
- No broken user journeys.

---

PHASE 6: RECURSIVE QUALITY LOOP

Repeat:

Discover Missing Features
→ Generate Tests
→ Execute Tests
→ Fix Defects
→ Regression Test

Until ALL of the following are true:

- No undiscovered features found.
- No failing tests.
- No critical defects.
- No high-severity defects.
- No unresolved UX issues.
- No incomplete user journeys.

After each iteration produce:

1. Coverage Summary
2. Features Tested
3. Defects Found
4. Defects Fixed
5. Remaining Risks
6. Confidence Score (0-100%)

Never declare completion unless all exit criteria are satisfied.
