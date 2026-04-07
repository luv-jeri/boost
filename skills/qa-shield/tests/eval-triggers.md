# QA Shield — Trigger Evaluation Tests

## Purpose

Verify that `/qa-shield` activates when it should and stays silent when it shouldn't. Target: 90% accuracy (18/20).

## Should Trigger

| # | Prompt | Expected Behavior |
|---|--------|-------------------|
| 1 | `/qa-shield` | Trigger, scan changed files |
| 2 | `/qa-shield src/components/Card/` | Trigger, scan specific path |
| 3 | `/qa-shield!` | Trigger, fast-track mode |
| 4 | `/qa-shield https://figma.com/design/abc123` | Trigger, with Figma fidelity |
| 5 | `/qa-shield --focus=overflow,scroll` | Trigger, focus mode |
| 6 | `/qa-shield --all` | Trigger, full project scan |
| 7 | `check my code for QA issues /qa-shield` | Trigger, suffix invocation |
| 8 | `/QA-SHIELD` | Trigger, case-insensitive |
| 9 | `/qa-shield src/Button.tsx src/Card.tsx` | Trigger, specific files |
| 10 | `/qa-shield --focus=detail` | Trigger, single focus category |

## Should NOT Trigger

| # | Prompt | Why Not |
|---|--------|---------|
| 11 | `run QA on this` | No /qa-shield trigger |
| 12 | `what does /qa-shield do?` | Asking about the skill, not invoking |
| 13 | `/qa-watch` | Different skill trigger |
| 14 | `shield the code from QA bugs` | Natural language, no trigger |
| 15 | `/boost check for QA issues` | Different skill trigger (/boost) |
| 16 | `check for overflow issues` | No trigger — valid request but not /qa-shield |
| 17 | `how do I install /qa-shield` | Question about the skill |
| 18 | `qa-shield is a great tool` | No leading slash |
| 19 | `/qa-shielded deployment` | Part of another word |
| 20 | `review the qa-shield skill code` | Talking about the skill itself |

## Scoring

- **18-20 correct:** Pass
- **15-17 correct:** Marginal — review failing cases
- **Below 15:** Fail — SKILL.md description needs adjustment
