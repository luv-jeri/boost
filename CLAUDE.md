# Agent Skills — Contributor Conventions

## Project Structure

This is a Claude Code plugin / Agent Skills package. No application code — only markdown skill files, JSON metadata, and documentation.

## Key Rules

- Each skill lives in `skills/<name>/`
- `SKILL.md` contains ALL essential process logic (target: 200-500 lines, self-contained)
- Reference files are for supplementary content only (detailed templates, examples, checklists)
- Description field: ONLY trigger conditions, NEVER process summary (Claude Search Optimization)
- Iron laws and red flags MUST be inline in SKILL.md, never in separate files
- Each skill MUST create a TodoWrite checklist at start for step tracking
- Each skill has its own `tests/eval-triggers.md` and `tests/eval-quality.md`

## Skills

### Boost (`skills/boost/`)
Prompt enhancer — transforms rough prompts into structured, context-rich prompts.

- `SKILL.md` — Complete self-contained skill (process, iron laws, detection logic)
- `references/task-templates.md` — 7 category templates with examples (loaded per-category on demand)
- `examples/before-after.md` — Complete transformation examples
- `tests/` — Evaluation framework
- `patterns/boost-patterns.md` — Starter template for users

### Pixel (`skills/pixel/`)
Figma-to-pixel-perfect UI — design map first, then incremental build-verify cycles.

- `SKILL.md` — Complete self-contained skill (process, iron laws, detection, flow)
- `references/design-map.md` — Detailed 7-section design map structure with table formats
- `references/verification.md` — Detailed checkpoint and final audit checklists
- `examples/design-map-example.md` — Complete design map example
- `tests/` — Evaluation framework

### QA Shield (`skills/qa-shield/`)
Post-build QA verification — 9-category scan for attention-to-detail issues.

- `SKILL.md` — Complete self-contained skill (process, iron laws, scoping, severity framework)
- `references/checklist-categories.md` — Detailed checks per category (9 categories)
- `references/report-format.md` — Report template with severity levels
- `examples/sample-report.md` — Complete example QA Shield report
- `tests/` — Evaluation framework

### QA Watch (`skills/qa-watch/`)
Lightweight QA companion — 5-category lite checks during development.

- `SKILL.md` — Complete self-contained skill (process, iron laws, lite checklist, session mode)
- `references/lite-checklist.md` — Detailed checks for the 5 lite categories
- `examples/sample-watch-output.md` — Example mid-build watch output
- `tests/` — Evaluation framework

## Making Changes

1. Edit SKILL.md directly for process changes — it IS the skill
2. Edit reference files only for supplementary detail changes
3. Run eval-triggers.md mentally — would any trigger behavior change?
4. Update eval-quality.md if grading criteria changed
5. Verify SKILL.md is self-contained: could Claude execute the full process without reading ANY reference file?
