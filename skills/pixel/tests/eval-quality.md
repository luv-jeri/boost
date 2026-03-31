# Pixel — Quality Evaluation Rubric

## Purpose

Grade the quality of `/pixel` output across 5 dimensions. Minimum passing score: 18/25.

## Dimensions

### 1. Design Map Completeness (1-5)

| Score | Criteria |
|---|---|
| 5 | All 7 sections filled, tokens mapped to project system, open questions specific and actionable |
| 4 | All 7 sections filled, minor gaps in token mapping or vague open questions |
| 3 | 5-6 sections filled, some tokens unmapped, open questions present but generic |
| 2 | 3-4 sections filled, significant token gaps, few open questions identified |
| 1 | Incomplete map, missing critical sections, no open questions despite ambiguities |

### 2. Token Accuracy (1-5)

| Score | Criteria |
|---|---|
| 5 | All values mapped to correct project tokens, new tokens properly flagged, stack syntax correct |
| 4 | 90%+ tokens correct, 1-2 close matches used without flagging |
| 3 | 75%+ tokens correct, some hardcoded values, wrong stack syntax in places |
| 2 | 50%+ tokens correct, many hardcoded values, inconsistent syntax |
| 1 | Mostly hardcoded values, tokens ignored or wrong |

### 3. Build Discipline (1-5)

| Score | Criteria |
|---|---|
| 5 | Strict build order followed, every checkpoint passed, no skipped verifications |
| 4 | Build order followed, 1 minor checkpoint deviation, all caught in audit |
| 3 | Build order mostly followed, 2-3 checkpoint skips, some deviations in audit |
| 2 | Build order loosely followed, multiple skipped checkpoints |
| 1 | No build order, built everything at once, no verification |

### 4. Pixel Fidelity (1-5)

| Score | Criteria |
|---|---|
| 5 | Indistinguishable from Figma — spacing, colors, typography, layout all exact |
| 4 | 95%+ match — 1-2 minor spacing or color deviations |
| 3 | 85%+ match — noticeable but not major deviations |
| 2 | 70%+ match — multiple visible deviations, layout roughly correct |
| 1 | Significantly different from Figma — wrong layout, colors, or proportions |

### 5. Completeness (1-5)

| Score | Criteria |
|---|---|
| 5 | All states implemented, responsive works at all breakpoints, micro-interactions smooth, a11y baseline passes |
| 4 | All states implemented, responsive works, 1-2 micro-interactions missing or rough |
| 3 | Most states implemented, responsive works at main breakpoints, some interactions missing |
| 2 | Default state works, responsive partially done, interactions mostly missing |
| 1 | Only default desktop state, no responsive, no interactions |

## Scoring

| Total | Grade | Action |
|---|---|---|
| 23-25 | Excellent | No changes needed |
| 18-22 | Good | Minor refinements to reference files |
| 13-17 | Needs Work | Review and revise process flow |
| Below 13 | Fail | Significant rework needed |

## Test Scenarios

Use these scenarios to evaluate quality:

1. **Simple component** — single card with hover state, 3 text elements, 1 button
2. **Full page** — dashboard with sidebar, header, card grid, table
3. **Form page** — multi-section settings form with validation states
4. **Landing page** — hero section, features grid, CTA, responsive
5. **Complex component** — data table with sorting, filtering, pagination, empty/loading states

Each scenario should score 18+ individually.
