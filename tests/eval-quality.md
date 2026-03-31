# Boost — Quality Evaluation Rubric

Rubric for grading the quality of enhanced prompts produced by the boost skill.

## How to Run

1. Feed a raw prompt through /boost
2. Capture the enhanced prompt output
3. Grade each dimension on a 1-5 scale
4. Calculate total score
5. Compare against quality threshold

---

## Grading Dimensions

### Task Clarity (1-5)

| Score | Criteria |
|-------|----------|
| 1 | Vague, could mean multiple things ("do something with auth") |
| 2 | Slightly better but still ambiguous ("fix auth issues") |
| 3 | Clear but missing nuance ("Fix authentication login crash") |
| 4 | Clear and specific ("Fix crash on login form submission when clicking submit") |
| 5 | Unambiguous, specific, actionable ("Fix crash on login form submission — form throws TypeError on submit handler when auth middleware returns 401") |

### Context Completeness (1-5)

| Score | Criteria |
|-------|----------|
| 1 | No context discovered — all fields empty or "Unknown" |
| 2 | Only tech stack identified (e.g., "Next.js") |
| 3 | Tech stack + related files identified |
| 4 | Tech stack + related files + recent changes |
| 5 | Full context: stack, recent changes, related files, team conventions, patterns |

### Constraints Relevance (1-5)

| Score | Criteria |
|-------|----------|
| 1 | No constraints or entirely generic ("don't break things") |
| 2 | Task-type boilerplate only ("maintain test coverage") |
| 3 | Task-appropriate defaults ("preserve API contracts" for refactor) |
| 4 | Task defaults + one project-specific constraint |
| 5 | Task defaults + multiple project-specific constraints from conventions |

### Success Criteria Measurability (1-5)

| Score | Criteria |
|-------|----------|
| 1 | Unmeasurable ("it should work better") |
| 2 | Slightly measurable ("tests pass") |
| 3 | Measurable but basic ("all existing tests pass") |
| 4 | Measurable and specific ("login form submits without crashing, auth tests pass") |
| 5 | Measurable, specific, and scoped ("login form submits, auth tests pass, no regressions in session management") |

### Category Accuracy (pass/fail)

Did the skill detect the correct task category based on the raw prompt's intent?

- **Pass:** Correct category
- **Fail:** Wrong category (e.g., classified a debug task as feature)

---

## Quality Threshold

| Metric | Minimum for Pass |
|--------|-----------------|
| Task Clarity | >= 3 |
| Context Completeness | >= 3 |
| Constraints Relevance | >= 2 |
| Success Criteria Measurability | >= 2 |
| Category Accuracy | Pass |
| **Total Score** | **>= 14/20** |

## Test Prompts for Quality Evaluation

Use these prompts to test boost quality:

1. `fix the login page it keeps crashing /boost` (Debug — should score 16+)
2. `add search to the app /boost` (Feature — vague, test how well boost enriches it)
3. `clean up this mess /boost` (Refactor — extremely vague, test context discovery)
4. `write some tests /boost` (Test — vague, test how boost adds specificity)
5. `check the code /boost` (Review — ambiguous, test category detection and focus areas)

## Recording Results

| Prompt | Category | Task Clarity | Context | Constraints | Success | Total | Pass? |
|--------|----------|-------------|---------|-------------|---------|-------|-------|
| ... | ... | /5 | /5 | /5 | /5 | /20 | Y/N |
