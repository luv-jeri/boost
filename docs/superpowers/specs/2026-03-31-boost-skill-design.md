# Boost Skill — Design Spec

**Date:** 2026-03-31
**Status:** Draft
**Platforms:** Claude Code (primary), Cursor (primary), Gemini/Codex/Windsurf (future)

## Problem

Team members struggle with writing effective prompts for AI coding agents. They use generic online prompt generators which produce low-quality, context-free prompts. This leads to poor agent output, wasted iterations, and frustration.

## Solution

A skill called `/boost` that takes a rough, unstructured prompt and transforms it into a clear, structured, context-rich prompt before the agent executes it. The skill auto-discovers project context, detects the task type, applies the right template, and optionally learns team-specific patterns over time.

## Invocation

**Prefix:** `/boost refactor the auth module it's messy`
**Suffix:** `refactor the auth module it's messy /boost`
**Fast-track (skip confirmation):** `refactor the auth module /boost!`

Both prefix and suffix are supported. The skill strips `/boost` or `/boost!` from the input and processes the remaining text as the raw prompt.

## Core Flow

```
1. CAPTURE — Strip /boost trigger, extract raw prompt
2. DETECT — Classify task type from keywords (7 categories)
3. DISCOVER — Read project config, git history, file structure, team patterns
4. STRUCTURE — Apply matching task template with discovered context
5. PRESENT — Show enhanced prompt to user
6. DECIDE:
   - /boost  → show enhanced prompt, wait for confirmation
   - /boost! → auto-execute without confirmation
   - User says "yes" / "y" → execute
   - User says "skip" / "s" → execute immediately
   - User edits → incorporate changes, then execute
7. EXECUTE — Run the structured prompt
8. SUGGEST — If unknown terms found, offer to add to boost-patterns.md
```

## Task Detection

The skill classifies prompts into 7 categories based on keyword matching:

| Category | Trigger Keywords | Template Focus |
|----------|-----------------|----------------|
| **Refactor** | refactor, clean up, reorganize, simplify, messy | Current state, target state, constraints, preserve behavior |
| **Debug** | fix, bug, broken, error, not working, crash | Symptoms, reproduction, expected vs actual, error messages |
| **Feature** | add, create, build, implement, new | Requirements, acceptance criteria, edge cases, integration |
| **Review** | review, check, audit, look at | Scope, what to look for, severity levels, output format |
| **Test** | test, coverage, spec, assert | What to test, test types, edge cases, mocking strategy |
| **Docs** | document, readme, explain, comment | Audience, scope, format, examples needed |
| **General** | (fallback when no keywords match) | Goal, context, constraints, success criteria |

**Detection rules:**
- Scan the raw prompt for keywords (case-insensitive)
- If multiple categories match, pick the one with the most keyword hits
- If tied, prefer the first match in priority order: Debug > Feature > Refactor > Test > Review > Docs > General
- Debug is highest priority because "fix" and "error" often co-occur with other categories but debugging is almost always the primary intent

## Structured Prompt Template

Every enhanced prompt follows this format:

```
## Task: [Clear one-line summary of what needs to be done]
## Type: [Category name]
## Context:
  - Project: [Framework, language, key dependencies]
  - Tech stack: [Discovered from config files]
  - Recent changes: [Relevant commits if applicable]
  - Related files: [File paths related to mentioned modules]
  - Team conventions: [Relevant conventions from patterns file]
## [Category-specific sections — see task-templates.md]
## Constraints:
  - [Auto-discovered from project conventions]
  - [Inferred from task type — e.g., "preserve behavior" for refactors]
## Success Criteria:
  - [Measurable outcomes for this task]
```

### Category-Specific Sections

**Refactor:**
- Current State: What exists now and what's wrong with it
- Target State: What it should look like after
- Boundaries: What must not change

**Debug:**
- Symptoms: What the user described as broken
- Expected Behavior: What should happen
- Actual Behavior: What happens instead
- Investigation Starting Points: Files and areas to check first

**Feature:**
- Requirements: What the feature should do
- Acceptance Criteria: How to know it's done
- Edge Cases: What could go wrong
- Integration Points: What existing code it touches

**Review:**
- Scope: What files/modules to review
- Focus Areas: What to look for (security, performance, correctness)
- Output Format: How to report findings

**Test:**
- Test Targets: What code to test
- Test Types: Unit, integration, e2e
- Edge Cases: Boundary conditions to cover
- Mocking Strategy: What to mock vs use real implementations

**Docs:**
- Audience: Who will read this
- Scope: What to document
- Format: README, inline comments, API docs, etc.
- Examples: What examples to include

**General:**
- Goal: What the user wants to achieve
- Approach: Suggested strategy

## Context Discovery

The skill auto-discovers project context from multiple sources, with a budget of ~2000 lines maximum.

### Priority 1 — Project Config (always read)
- `CLAUDE.md` / `.cursorrules` / `.cursor/rules/` — project conventions
- `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` — tech stack, dependencies
- `tsconfig.json` / `.eslintrc` / `prettier.config` — code style settings

### Priority 2 — Git Context (if available)
- `git log --oneline -10` — recent changes
- `git diff --stat HEAD~3` — recently changed files
- `git branch --show-current` — current branch name

### Priority 3 — File Structure (when prompt mentions specific modules)
- Scan directory structure around mentioned files/modules
- Read first 50 lines of mentioned files to understand current state
- Check for related test files

### Priority 4 — Team Patterns (always read)
- `boost-patterns.md` — team acronyms, module aliases, conventions

**Skip rules:**
- Priority 2 and 3 are skipped if the prompt is a simple documentation or general request with no specific module mentioned
- If git is not available, skip Priority 2 silently
- If `boost-patterns.md` does not exist, skip Priority 4 silently and offer to create it after the first execution with a starter template

## Adaptive Patterns System

### File: `boost-patterns.md` (project root, git-tracked)

```markdown
# Boost Patterns

## Aliases
- auth module = src/auth/ (authentication & session management)
- DB = PostgreSQL with Prisma ORM
- FE = frontend, src/app/ (Next.js app router)
- BE = backend, src/api/ (API routes)

## Team Conventions
- All API responses use { data, error, status } shape
- Use zod for validation, never manual checks
- Tests go in __tests__/ next to source, not a separate test/ tree

## Common Tasks
- "deploy" = build > test > push to main > Vercel auto-deploys
- "lint fix" = run eslint --fix then prettier --write

## Do Not Touch
- src/legacy/ — migration in progress, do not refactor
- .env.production — never modify via AI agent
```

### Adaptive Learning Rules

After successful task execution, if the skill encountered unresolved terms:

1. Collect up to 3 unrecognized terms from the raw prompt
2. Present suggestions to the user:
   ```
   Boost noticed some terms it didn't recognize:
     - "the gateway" — what does this refer to? (file path, module, service?)
     - "K8s config" — should I add: K8s config = kubernetes/, Helm charts?
   Want me to add these to boost-patterns.md?
   ```
3. Never auto-write — always ask first
4. If user confirms, append to the appropriate section (Aliases, Conventions, etc.)
5. Never interrupt the main task flow — suggestions come after execution only

## Platform Implementation

### Claude Code

```
project-root/
  .claude/skills/boost/
    SKILL.md                      # Core flow, invocation, task detection
    references/
      task-templates.md           # 7 task category templates
      context-discovery.md        # Discovery rules and priority logic
  boost-patterns.md               # Git-tracked, team-shared
```

**SKILL.md frontmatter:**
```yaml
---
name: boost
description: Use when user appends or prefixes /boost to their prompt. Restructures rough prompts into clear, structured, context-rich prompts before execution. Supports /boost! for auto-execute without confirmation.
---
```

- Invoked via the Skill tool
- Reads references on demand (progressive disclosure)
- Reads `boost-patterns.md` from project root

### Cursor

```
project-root/
  .cursor/rules/boost.mdc        # MDC trigger rule (Agent-Requested type)
  .cursor/skills/boost/
    SKILL.md                      # Same core logic, minor Cursor adaptations
    references/
      task-templates.md           # Identical to Claude Code version
      context-discovery.md        # Identical to Claude Code version
  boost-patterns.md               # Same file, already in project root
```

**boost.mdc frontmatter:**
```yaml
---
description: Invoke when user includes /boost or /boost! in their prompt to restructure and enhance it before execution
alwaysApply: false
globs:
---
```

- `alwaysApply: false` makes this Agent-Requested — Cursor activates it only when relevant
- SKILL.md body is ~90% identical to Claude Code version
- References are identical copies

### Shared vs Platform-Specific

| File | Shared? | Notes |
|------|---------|-------|
| `task-templates.md` | Identical across platforms | Copy or symlink |
| `context-discovery.md` | Identical across platforms | Copy or symlink |
| `boost-patterns.md` | Single file in project root | Both platforms read same file |
| `SKILL.md` | ~90% shared | Minor platform-specific wording |
| `boost.mdc` | Cursor only | Thin trigger rule |

## Example End-to-End

**User types:** `fix the login page it keeps crashing when I click submit /boost`

**Skill detects:** Debug (keywords: "fix", "crashing")

**Context discovered:**
- Project: Next.js 14 with TypeScript
- Recent: auth middleware updated 2 days ago (commit abc123)
- Related: `src/app/login/page.tsx`, `src/lib/auth.ts`
- Patterns: "login page" not in aliases — candidate for suggestion

**Enhanced prompt presented:**
```
## Task: Fix crash on login form submission
## Type: Debug
## Context:
  - Project: Next.js 14 app with TypeScript
  - Tech stack: React, Prisma, PostgreSQL
  - Recent changes: auth middleware updated 2 days ago (commit abc123)
  - Related files: src/app/login/page.tsx, src/lib/auth.ts
## Symptoms: Login page crashes on submit button click
## Expected Behavior: Form submits and authenticates user
## Actual Behavior: Crash/error on submit
## Investigation Starting Points:
  - Form submit handler in login page component
  - Auth middleware (recently changed — likely culprit)
  - Network requests triggered on form submit
## Constraints:
  - Do not modify existing auth API contracts
  - Maintain existing test coverage
## Success Criteria:
  - Login form submits without crashing
  - All existing auth tests still pass
```

**User says:** `yes`

**Skill executes** the enhanced prompt. Agent debugs the issue with full context.

**After execution, skill suggests:**
```
Boost noticed a term it could add to patterns:
  - "login page" = src/app/login/page.tsx
Want me to add this to boost-patterns.md?
```

## Future Expansion

### Phase 2 — Additional Platforms
- **Gemini CLI:** `SKILL.md` in `.gemini/skills/boost/` with same structure
- **Codex:** `SKILL.md` in `.agents/skills/boost/` with same structure
- **Windsurf:** `.windsurf/rules/boost.md` rule file (no skills system — embed logic in rule)

### Phase 3 — Enhancements
- Prompt quality scoring (rate the raw prompt 1-5 and show improvement)
- Team analytics (most common task types, most used patterns)
- Custom template overrides per team member
