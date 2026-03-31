# Agent Skills — Contributor Conventions

## Project Structure

This is a Claude Code plugin / Agent Skills package. No application code — only markdown skill files, JSON metadata, and documentation.

## Key Rules

- Each skill lives in `skills/<name>/`
- `SKILL.md` must stay under 150 words (trigger-only, no inline logic)
- All process logic lives in `references/` (loaded on demand)
- Progressive disclosure: metadata → SKILL.md → references → examples
- Each skill has its own `tests/eval-triggers.md` and `tests/eval-quality.md`

## Skills

### Boost (`skills/boost/`)
Prompt enhancer — transforms rough prompts into structured, context-rich prompts.

- `SKILL.md` — When to trigger and what to read
- `references/flow.md` — Complete 9-step process
- `references/task-templates.md` — 7 category templates with examples
- `references/context-discovery.md` — How to find project context
- `references/red-flags.md` — Iron laws, rationalization prevention
- `references/prompt-passthrough.md` — Detecting already-structured prompts
- `examples/before-after.md` — Complete transformation examples
- `tests/` — Evaluation framework
- `patterns/boost-patterns.md` — Starter template for users

### Pixel (`skills/pixel/`)
Figma-to-pixel-perfect UI — design map first, then incremental build-verify cycles.

- `SKILL.md` — When to trigger and what to read
- `references/flow.md` — Complete 2-phase process (Design Map → Build)
- `references/design-map.md` — 7-section design map structure
- `references/input-detection.md` — Figma MCP, screenshot, dev mode export handling
- `references/token-extraction.md` — CSS variable and design token matching
- `references/verification.md` — Checkpoint and final audit checklists
- `references/red-flags.md` — Iron laws, rationalization prevention
- `examples/design-map-example.md` — Complete design map example
- `tests/` — Evaluation framework

## Making Changes

1. Edit the relevant reference file in the skill's directory
2. Update examples if behavior changed
3. Run eval-triggers.md mentally — would any trigger behavior change?
4. Update eval-quality.md if grading criteria changed
5. Keep SKILL.md under 150 words
