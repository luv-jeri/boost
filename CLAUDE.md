# Boost Plugin — Contributor Conventions

## Project Structure

This is a Claude Code plugin / Agent Skills package. No application code — only markdown skill files, JSON metadata, and documentation.

## Key Rules

- `skills/boost/SKILL.md` must stay under 150 words (trigger-only, no inline logic)
- All process logic lives in `skills/boost/references/` (loaded on demand)
- Progressive disclosure: metadata → SKILL.md → references → examples
- Test changes with `tests/eval-triggers.md` (trigger accuracy) and `tests/eval-quality.md` (output quality)

## File Responsibilities

- `SKILL.md` — When to trigger (description) and what to read (references)
- `references/flow.md` — The complete 9-step process
- `references/task-templates.md` — 7 category templates with examples
- `references/context-discovery.md` — How to find project context
- `references/red-flags.md` — What never to do (iron laws, rationalization prevention)
- `references/prompt-passthrough.md` — Detecting already-structured prompts
- `examples/before-after.md` — Complete transformation examples
- `tests/` — Evaluation framework
- `patterns/boost-patterns.md` — Starter template for users

## Making Changes

1. Edit the relevant reference file
2. Update before-after examples if behavior changed
3. Run eval-triggers.md mentally — would any trigger behavior change?
4. Update eval-quality.md if grading criteria changed
5. Keep SKILL.md under 150 words
