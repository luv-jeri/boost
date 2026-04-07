# QA Watch — Trigger Evaluation Tests

## Purpose

Verify that `/qa-watch` activates when it should and stays silent when it shouldn't. Target: 90% accuracy (18/20).

## Should Trigger

| # | Prompt | Expected Behavior |
|---|--------|-------------------|
| 1 | `/qa-watch` | Trigger, manual mode |
| 2 | `/qa-watch --session` | Trigger, session mode |
| 3 | `/qa-watch stop` | Trigger, end session mode |
| 4 | `/qa-watch --focus=overflow` | Trigger, focused manual scan |
| 5 | `/qa-watch --focus=overflow,states` | Trigger, multi-focus scan |
| 6 | `check this component /qa-watch` | Trigger, suffix invocation |
| 7 | `/QA-WATCH` | Trigger, case-insensitive |
| 8 | `/qa-watch src/components/Card/` | Trigger, specific path |
| 9 | `/qa-watch --focus=detail` | Trigger, single focus |
| 10 | `/qa-watch --session` (during active build) | Trigger, session mode mid-conversation |

## Should NOT Trigger

| # | Prompt | Why Not |
|---|--------|---------|
| 11 | `watch for QA issues` | No /qa-watch trigger |
| 12 | `what does /qa-watch do?` | Asking about the skill, not invoking |
| 13 | `/qa-shield` | Different skill trigger |
| 14 | `keep an eye on code quality` | Natural language, no trigger |
| 15 | `/boost watch for bugs` | Different skill trigger (/boost) |
| 16 | `qa-watch is useful` | No leading slash |
| 17 | `how do I use /qa-watch` | Question about the skill |
| 18 | `/qa-watching the build` | Part of another word |
| 19 | `watch the qa-watch demo` | Referring to the skill itself |
| 20 | `/pixel then /qa-watch` | Multiple skills — should handle /pixel first |

## Scoring

- **18-20 correct:** Pass
- **15-17 correct:** Marginal — review failing cases
- **Below 15:** Fail — SKILL.md description needs adjustment
