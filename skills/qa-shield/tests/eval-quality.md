# QA Shield — Quality Evaluation Rubric

## Purpose

Grade the quality of `/qa-shield` output across 5 dimensions. Minimum passing score: 18/25.

## Dimensions

### 1. Scoping Accuracy (1-5)

| Score | Criteria |
|---|---|
| 5 | All categories correctly scoped (Active or N/A with accurate reasons), no false N/As, no missed N/As |
| 4 | 8/9 categories correctly scoped, 1 minor mis-scope |
| 3 | 7/9 correct, 2 mis-scopes (e.g., marked active when clearly N/A) |
| 2 | 5-6/9 correct, multiple incorrect scopes |
| 1 | Scoping skipped or mostly wrong |

### 2. Finding Accuracy (1-5)

| Score | Criteria |
|---|---|
| 5 | All findings are real issues with correct severity, no false positives, no missed obvious issues |
| 4 | 90%+ findings accurate, 1 false positive or 1 missed obvious issue |
| 3 | 75%+ accurate, 2-3 false positives or missed issues |
| 2 | 50%+ accurate, multiple false positives or significant misses |
| 1 | Mostly false positives or critical issues missed |

### 3. Location Specificity (1-5)

| Score | Criteria |
|---|---|
| 5 | Every finding has exact file:line or DOM element, all locations are correct |
| 4 | 90%+ findings have specific locations, all correct |
| 3 | 75%+ have locations, some are vague ("somewhere in Card component") |
| 2 | 50%+ have locations, some incorrect |
| 1 | Most findings lack specific locations |

### 4. Fix Quality (1-5)

| Score | Criteria |
|---|---|
| 5 | All suggested fixes are correct, minimal, and follow project conventions |
| 4 | 90%+ fixes correct, 1 fix slightly off or overly complex |
| 3 | 75%+ fixes correct, some don't follow project conventions |
| 2 | 50%+ fixes correct, some would introduce new issues |
| 1 | Most fixes are wrong or would break things |

### 5. Report Clarity (1-5)

| Score | Criteria |
|---|---|
| 5 | Summary table is clear, findings are well-organized, severity is consistent, easy to act on |
| 4 | Report is clear, 1 minor formatting or organization issue |
| 3 | Report is readable but findings are inconsistently formatted |
| 2 | Report is disorganized, hard to find specific issues |
| 1 | Report is confusing or missing the summary table |

## Scoring

| Total | Grade | Action |
|---|---|---|
| 23-25 | Excellent | No changes needed |
| 18-22 | Good | Minor refinements to reference files |
| 13-17 | Needs Work | Review and revise process flow |
| Below 13 | Fail | Significant rework needed |

## Test Scenarios

Use these scenarios to evaluate quality:

1. **Simple component** — static card with no API calls, no interactivity (most categories should be N/A)
2. **Data-heavy component** — table with API data, pagination, sorting, filters
3. **Interactive form** — multi-step form with validation, submission, error handling
4. **Full page with Figma** — dashboard page with Figma URL provided
5. **Component with known overflow** — card with long dynamic text, nested scrollable areas

Each scenario should score 18+ individually.
