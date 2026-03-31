# Pixel — Trigger Evaluation Tests

## Purpose

Verify that `/pixel` activates when it should and stays silent when it shouldn't. Target: 90% accuracy (18/20).

## Should Trigger

| # | Prompt | Expected Behavior |
|---|--------|-------------------|
| 1 | `/pixel https://figma.com/design/abc123` | Trigger with Figma MCP input |
| 2 | `/pixel` (with screenshot in context) | Trigger with screenshot input |
| 3 | `build this page /pixel` (with Figma URL) | Trigger, suffix invocation |
| 4 | `/pixel!` (with screenshot) | Trigger, fast-track mode |
| 5 | `/pixel --relaxed https://figma.com/file/xyz` | Trigger, relaxed mode |
| 6 | `/pixel` (with pasted CSS specs) | Trigger with dev mode export input |
| 7 | `implement this design /pixel` | Trigger, natural language with suffix |
| 8 | `/pixel build the hero section from this mockup` | Trigger with description |
| 9 | `/pixel` (with multiple screenshots) | Trigger with multiple image inputs |
| 10 | `/pixel https://figma.com/design/abc?node-id=123` | Trigger with specific Figma node |

## Should NOT Trigger

| # | Prompt | Why Not |
|---|--------|---------|
| 11 | `make it pixel perfect` | No /pixel trigger, just uses the word "pixel" |
| 12 | `fix the pixel ratio on the canvas` | About actual pixels, not the skill |
| 13 | `/boost implement the figma design` | Different skill trigger (/boost) |
| 14 | `what does /pixel do?` | Asking about the skill, not invoking it |
| 15 | `implement the figma design` | No trigger — should work without /pixel |
| 16 | `pixel art generator` | Unrelated use of "pixel" |
| 17 | `/pixelate the image` | Different command |
| 18 | `review the pixel skill code` | Talking about the skill itself |
| 19 | `the pixel density is wrong on retina` | Display/rendering issue |
| 20 | `how do I install /pixel` | Question about the skill |

## Scoring

- **18-20 correct:** Pass
- **15-17 correct:** Marginal — review failing cases
- **Below 15:** Fail — SKILL.md description needs adjustment
