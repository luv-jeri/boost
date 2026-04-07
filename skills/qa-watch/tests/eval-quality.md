# QA Watch — Quality Evaluation Rubric

## Purpose

Grade the quality of `/qa-watch` output across 5 dimensions. Minimum passing score: 14/20.

## Dimensions

### 1. Scoping Speed (1-4)

| Score | Criteria |
|---|---|
| 4 | Scoping is fast — categories marked N/A or active within seconds, no unnecessary file reading |
| 3 | Scoping is mostly fast, 1 unnecessary file read or slow step |
| 2 | Scoping reads too many files or takes too long for a "lightweight" check |
| 1 | Scoping is as slow as qa-shield — defeats the purpose of qa-watch |

### 2. Finding Accuracy (1-4)

| Score | Criteria |
|---|---|
| 4 | All findings are real issues, no false positives, no missed obvious issues within the 5 categories |
| 3 | 90%+ accurate, 1 false positive or 1 missed obvious issue |
| 2 | 75%+ accurate, multiple false positives or misses |
| 1 | Mostly false positives or critical issues missed |

### 3. Report Compactness (1-4)

| Score | Criteria |
|---|---|
| 4 | Findings are 1 line each, no unnecessary detail, easy to scan at a glance |
| 3 | Mostly compact, 1-2 findings are too verbose |
| 2 | Report looks like a mini qa-shield report — too detailed for inline format |
| 1 | Full report table produced — wrong format entirely |

### 4. Build Flow Respect (1-4)

| Score | Criteria |
|---|---|
| 4 | Findings don't interrupt or slow the build. Session mode runs seamlessly between build steps. |
| 3 | Minor interruption — 1 unnecessary confirmation or pause |
| 2 | Noticeable interruption — asked to stop and address findings before continuing |
| 1 | Blocked the build — treated findings as gates instead of informational |

### 5. Session Mode Behavior (1-4)

| Score | Criteria |
|---|---|
| 4 | Session mode activates/deactivates cleanly, runs after each build step, provides session summary on stop |
| 3 | Session mode works but misses 1 build step or summary is incomplete |
| 2 | Session mode is inconsistent — runs after some steps but not others |
| 1 | Session mode doesn't work or produces full reports instead of inline |

## Scoring

| Total | Grade | Action |
|---|---|---|
| 18-20 | Excellent | No changes needed |
| 14-17 | Good | Minor refinements |
| 10-13 | Needs Work | Review process flow |
| Below 10 | Fail | Significant rework needed |

## Test Scenarios

1. **Simple static component** — most categories should be N/A, fast scoping
2. **Interactive form component** — all 5 categories active
3. **Session mode during 3-component build** — checks run after each, session summary on stop
4. **Focus mode on overflow only** — single category, very compact output
5. **Component with no issues** — clean pass, minimal output

Each scenario should score 14+ individually.
