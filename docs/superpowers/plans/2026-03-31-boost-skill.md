# Boost Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a `/boost` skill for Claude Code and Cursor that transforms rough prompts into structured, context-rich prompts before execution.

**Architecture:** A modular skill with a core SKILL.md handling the flow (capture → detect → discover → structure → present → execute → suggest), with reference files for task templates and context discovery rules. A shared `boost-patterns.md` in project root enables adaptive team learning. Cursor gets a thin `.mdc` trigger rule pointing to an equivalent skill.

**Tech Stack:** Markdown (SKILL.md format), YAML frontmatter, MDC (Cursor rules format)

---

## File Structure

```
project-root/
  .claude/skills/boost/
    SKILL.md                          # Core skill — flow, invocation, task detection, confirmation
    references/
      task-templates.md               # 7 structured templates (refactor, debug, feature, review, test, docs, general)
      context-discovery.md            # Discovery priority rules and skip logic
  .cursor/rules/boost.mdc            # Cursor MDC trigger rule (Agent-Requested)
  .cursor/skills/boost/
    SKILL.md                          # Cursor-adapted version of the core skill
    references/
      task-templates.md               # Identical copy of Claude Code version
      context-discovery.md            # Identical copy of Claude Code version
  boost-patterns.md                   # Team-shared adaptive patterns (git-tracked)
```

---

### Task 1: Create the Task Templates Reference File

**Files:**
- Create: `.claude/skills/boost/references/task-templates.md`

This is a pure reference file — no frontmatter, no skill logic. It defines the 7 structured prompt templates that the core SKILL.md loads on demand.

- [ ] **Step 1: Create the task-templates.md file**

```markdown
# Boost Task Templates

## How to Use

Match the detected task category to the corresponding template below. Fill in each field using the raw prompt and discovered context. If a field cannot be filled, write "Unknown — investigate" rather than omitting it.

---

## Refactor Template

```
## Task: [One-line summary: what is being refactored and why]
## Type: Refactor
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Recent changes: [relevant commits, if any]
  - Related files: [file paths for the module being refactored]
  - Team conventions: [relevant entries from boost-patterns.md]
## Current State: [What exists now and what is wrong with it]
## Target State: [What it should look like after refactoring]
## Boundaries: [What must NOT change — APIs, behavior, contracts]
## Constraints:
  - Preserve all existing behavior and API contracts
  - Maintain or improve test coverage
  - [Any project-specific constraints from conventions]
## Success Criteria:
  - Code is reorganized per target state
  - All existing tests pass without modification
  - No behavior changes
```

## Debug Template

```
## Task: [One-line summary of the bug]
## Type: Debug
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Recent changes: [relevant commits — especially recent ones that may have caused the bug]
  - Related files: [file paths related to the bug area]
  - Team conventions: [relevant entries from boost-patterns.md]
## Symptoms: [What the user described as broken — exact words]
## Expected Behavior: [What should happen]
## Actual Behavior: [What happens instead]
## Investigation Starting Points:
  - [Files and functions most likely involved]
  - [Recently changed files in the area]
  - [Related error handlers or middleware]
## Constraints:
  - Do not modify unrelated code
  - Maintain existing test coverage
  - [Any project-specific constraints]
## Success Criteria:
  - The described bug no longer occurs
  - All existing tests pass
  - Root cause is identified and fixed (not just symptoms)
```

## Feature Template

```
## Task: [One-line summary of the feature to build]
## Type: Feature
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Recent changes: [relevant commits, if any]
  - Related files: [existing files this feature integrates with]
  - Team conventions: [relevant entries from boost-patterns.md]
## Requirements: [What the feature should do — extracted from the raw prompt]
## Acceptance Criteria:
  - [Concrete, testable criteria derived from the prompt]
## Edge Cases:
  - [What could go wrong or behave unexpectedly]
## Integration Points: [Existing code this feature touches or extends]
## Constraints:
  - Follow existing project patterns and conventions
  - [Any project-specific constraints]
## Success Criteria:
  - Feature works as described
  - Tests cover happy path and edge cases
  - Integrates cleanly with existing code
```

## Review Template

```
## Task: [One-line summary of what to review]
## Type: Review
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Related files: [file paths to review]
  - Team conventions: [relevant entries from boost-patterns.md]
## Scope: [Which files, modules, or changes to review]
## Focus Areas:
  - Security (injection, auth, data exposure)
  - Performance (N+1 queries, unnecessary re-renders, memory leaks)
  - Correctness (logic errors, edge cases, error handling)
  - Code quality (readability, naming, duplication)
## Output Format:
  - List findings by severity: critical, warning, suggestion
  - Include file path and line number for each finding
  - Suggest fixes for critical and warning items
## Constraints:
  - Review only the specified scope
  - [Any project-specific review standards]
## Success Criteria:
  - All critical issues identified and flagged
  - Actionable suggestions provided
```

## Test Template

```
## Task: [One-line summary of what to test]
## Type: Test
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files, including test framework]
  - Related files: [source files to test + existing test files]
  - Team conventions: [testing conventions from boost-patterns.md]
## Test Targets: [Specific functions, modules, or behaviors to test]
## Test Types:
  - [Unit / Integration / E2E — as appropriate]
## Edge Cases:
  - [Boundary conditions, empty inputs, error states, concurrent access]
## Mocking Strategy: [What to mock vs use real implementations]
## Constraints:
  - Follow existing test patterns in the project
  - [Test file location conventions from boost-patterns.md]
## Success Criteria:
  - Tests cover happy path and specified edge cases
  - Tests are deterministic and fast
  - All tests pass
```

## Docs Template

```
## Task: [One-line summary of what to document]
## Type: Documentation
## Context:
  - Project: [framework, language, key dependencies]
  - Related files: [code files being documented]
  - Team conventions: [documentation conventions from boost-patterns.md]
## Audience: [Who will read this — developers, end users, new team members]
## Scope: [What to document — API, architecture, usage, setup]
## Format: [README, inline comments, API docs, runbook, etc.]
## Examples Needed: [What usage examples to include]
## Constraints:
  - Match existing documentation style in the project
  - [Any project-specific docs conventions]
## Success Criteria:
  - Documentation is accurate and complete for the specified scope
  - Examples are runnable
  - Audience can understand without prior context
```

## General Template

```
## Task: [One-line summary of the goal]
## Type: General
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Recent changes: [relevant commits, if any]
  - Related files: [any files mentioned or implied]
  - Team conventions: [relevant entries from boost-patterns.md]
## Goal: [What the user wants to achieve — clarified and expanded]
## Approach: [Suggested strategy based on the prompt and context]
## Constraints:
  - [Any discovered project constraints]
## Success Criteria:
  - [Measurable outcomes derived from the goal]
```
```

- [ ] **Step 2: Review the file for completeness**

Verify all 7 templates are present: Refactor, Debug, Feature, Review, Test, Docs, General. Each must have: Task, Type, Context block, category-specific sections, Constraints, Success Criteria.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills/boost/references/task-templates.md
git commit -m "feat(boost): add 7 structured task templates for prompt enhancement"
```

---

### Task 2: Create the Context Discovery Reference File

**Files:**
- Create: `.claude/skills/boost/references/context-discovery.md`

- [ ] **Step 1: Create the context-discovery.md file**

```markdown
# Boost Context Discovery Rules

## Overview

When enhancing a prompt, discover project context to fill template fields. Follow the priority order below. Stay within a ~2000 line budget to avoid consuming excessive context window.

## Priority 1 — Project Config (always read)

Read these files if they exist. Skip silently if missing.

**Project conventions:**
- `CLAUDE.md` — Claude Code project conventions
- `.cursorrules` — Cursor project rules (legacy format)
- `.cursor/rules/*.mdc` — Cursor MDC rules

**Tech stack detection:**
- `package.json` — Node.js/JavaScript: extract `name`, `dependencies`, `devDependencies`
- `pyproject.toml` — Python: extract project name, dependencies
- `Cargo.toml` — Rust: extract package info and dependencies
- `go.mod` — Go: extract module name and dependencies
- `pom.xml` — Java: extract groupId, artifactId, dependencies
- `Gemfile` — Ruby: extract gem dependencies

**Code style:**
- `tsconfig.json` — TypeScript configuration
- `.eslintrc` / `.eslintrc.json` / `.eslintrc.js` — Linting rules
- `prettier.config.js` / `.prettierrc` — Formatting rules
- `biome.json` — Biome config

**Extract from config files:**
- Language and framework (e.g., "Next.js 14 with TypeScript")
- Key dependencies (e.g., "Prisma, zod, tailwindcss")
- Test framework (e.g., "vitest", "jest", "pytest")
- Build tool (e.g., "vite", "webpack", "turbopack")

## Priority 2 — Git Context (if available)

Only run if git is initialized in the project. Skip silently if not.

**Commands to run:**
- `git log --oneline -10` — last 10 commits for recent activity
- `git diff --stat HEAD~3` — files changed in last 3 commits
- `git branch --show-current` — current branch name (hints at current work)

**Extract from git:**
- Recently changed files that overlap with the mentioned module
- Commit messages that hint at ongoing work
- Branch name if it describes a feature or fix

## Priority 3 — File Structure (conditional)

Only run if the raw prompt mentions specific modules, files, or areas of code. Skip if the prompt is a general documentation or broad request.

**Steps:**
1. Resolve module names using `boost-patterns.md` aliases (e.g., "auth module" → `src/auth/`)
2. If no alias found, search for directories/files matching the mentioned term
3. List the directory structure (1-2 levels deep) around matched paths
4. Read the first 50 lines of directly mentioned files
5. Check for related test files (look for `__tests__/`, `*.test.*`, `*.spec.*` siblings)

**Extract from file structure:**
- Exact file paths for the "Related files" template field
- Current state of mentioned code (for refactor/debug tasks)
- Test file locations (for test/refactor tasks)

## Priority 4 — Team Patterns (always read)

Read `boost-patterns.md` from the project root. If the file does not exist, skip silently and offer to create it after the first execution.

**Extract from patterns:**
- Resolve aliases mentioned in the raw prompt (e.g., "BE" → "backend, src/api/")
- Pull relevant team conventions for the Constraints field
- Check "Do Not Touch" section to add warnings to Constraints
- Note any "Common Tasks" that match the prompt

**Detecting unresolved terms:**
Track any term in the raw prompt that:
1. Looks like a module/component name (capitalized, hyphenated, or quoted)
2. Is not found in boost-patterns.md aliases
3. Is not a standard programming term
4. Could not be resolved to a file path via Priority 3

These become candidates for the adaptive pattern suggestion after execution.

## Budget Enforcement

- Priority 1: ~500 lines max (config files are typically short)
- Priority 2: ~100 lines max (git output is compact)
- Priority 3: ~1000 lines max (directory listing + file previews)
- Priority 4: ~400 lines max (patterns file)
- Total: ~2000 lines

If any priority exceeds its budget, truncate and note "[truncated]" in the context.
```

- [ ] **Step 2: Commit**

```bash
git add .claude/skills/boost/references/context-discovery.md
git commit -m "feat(boost): add context discovery rules with priority-based resolution"
```

---

### Task 3: Create the Claude Code SKILL.md

**Files:**
- Create: `.claude/skills/boost/SKILL.md`

This is the core skill file — the main logic that orchestrates the entire boost flow.

- [ ] **Step 1: Create the SKILL.md file**

```markdown
---
name: boost
description: Use when user appends or prefixes /boost to their prompt. Restructures rough prompts into clear, structured, context-rich prompts before execution. Supports /boost! for auto-execute without confirmation.
user-invokable: true
---

# Boost — Prompt Enhancer

Transform rough, unstructured prompts into clear, structured, context-rich prompts before execution.

## Invocation

The user triggers this skill by including `/boost` or `/boost!` in their prompt:
- **Prefix:** `/boost refactor the auth module`
- **Suffix:** `refactor the auth module /boost`
- **Fast-track:** `refactor the auth module /boost!` (skip confirmation, auto-execute)

## Core Flow

```
1. CAPTURE raw prompt (strip /boost or /boost! trigger)
2. DETECT task type from keywords
3. DISCOVER project context (read references/context-discovery.md)
4. STRUCTURE prompt using matching template (read references/task-templates.md)
5. PRESENT enhanced prompt to user
6. DECIDE: confirm, edit, or auto-execute
7. EXECUTE the structured prompt
8. SUGGEST pattern additions if unknown terms found
```

## Step 1: Capture

Strip `/boost` or `/boost!` from the user's input. The remaining text is the raw prompt.

- If `/boost!` was used, set fast-track mode = ON (skip confirmation in Step 6)
- If `/boost` was used, set fast-track mode = OFF (show confirmation)
- The trigger can appear at the start or end of the prompt — handle both

## Step 2: Detect Task Type

Scan the raw prompt for keywords (case-insensitive) to classify into one of 7 categories:

| Category | Keywords |
|----------|----------|
| Debug | fix, bug, broken, error, not working, crash, failing, issue |
| Feature | add, create, build, implement, new, make, setup, introduce |
| Refactor | refactor, clean up, reorganize, simplify, messy, restructure, improve, optimize |
| Test | test, coverage, spec, assert, unit test, integration test |
| Review | review, check, audit, look at, examine, inspect |
| Docs | document, readme, explain, comment, describe, write docs |
| General | (fallback — no keywords matched) |

**Tie-breaking priority:** Debug > Feature > Refactor > Test > Review > Docs > General

Count keyword matches per category. Highest count wins. If tied, use the priority order above. Debug is highest because "fix" and "error" co-occur with other categories but debugging is almost always the primary intent.

## Step 3: Discover Context

Read `references/context-discovery.md` for the full discovery rules.

**Quick summary of priority order:**
1. **Project config** (always) — CLAUDE.md, package.json, tsconfig, etc.
2. **Git context** (if available) — recent commits, changed files, branch name
3. **File structure** (if prompt mentions modules) — directory listing, file previews
4. **Team patterns** (always) — boost-patterns.md from project root

Stay within ~2000 line budget. If `boost-patterns.md` does not exist, skip silently — you will offer to create it in Step 8.

**Track unresolved terms** during discovery: any module/component name in the raw prompt that could not be resolved to a file path or alias.

## Step 4: Structure the Prompt

Read `references/task-templates.md` and select the template matching the detected task type.

Fill in every field:
- **Task:** Rewrite the raw prompt as a clear one-line summary
- **Type:** The detected category name
- **Context:** Populate from discovered project context
- **Category-specific sections:** Fill using the raw prompt content and discovered context
- **Constraints:** Combine project conventions + task-type defaults (e.g., "preserve behavior" for refactors)
- **Success Criteria:** Derive measurable outcomes from the task description

If a field cannot be filled from available information, write "Unknown — investigate" rather than omitting it.

## Step 5: Present

Display the enhanced prompt to the user in a clear, readable format.

Prefix it with:
```
**Boost enhanced your prompt:**
```

## Step 6: Decide

**If fast-track mode is ON** (`/boost!` was used):
- Skip confirmation entirely
- Proceed directly to Step 7

**If fast-track mode is OFF** (`/boost` was used):
- Show the enhanced prompt (Step 5)
- Ask: "Execute this enhanced prompt? (yes / edit / skip)"
  - **"yes" or "y"** — proceed to Step 7 with the enhanced prompt
  - **"edit"** — user provides modifications, incorporate them, then proceed to Step 7
  - **"skip" or "s"** — proceed to Step 7 immediately (same as fast-track, for users who want to skip mid-flow)

## Step 7: Execute

Execute the structured prompt as if the user had typed it directly. The agent now works on the task with full context and clear structure.

IMPORTANT: Do not re-announce the prompt. Just begin working on the task described in the structured prompt.

## Step 8: Suggest Pattern Additions

After the task execution completes, if there were unresolved terms from Step 3:

1. Collect up to 3 unresolved terms
2. Present them to the user:
   ```
   Boost noticed terms it could add to your team patterns:
     - "[term]" — what does this refer to? (file path, module, service?)
   Want me to add these to boost-patterns.md?
   ```
3. **Never auto-write** — always ask first
4. If user confirms, append to the appropriate section in `boost-patterns.md`
5. If `boost-patterns.md` does not exist, offer to create it with a starter template:

```markdown
# Boost Patterns

## Aliases

## Team Conventions

## Common Tasks

## Do Not Touch
```

**Rules for suggestions:**
- Maximum 3 suggestions per invocation
- Never interrupt the main task — suggestions come AFTER execution only
- If the user declines, do not ask again for the same terms in the same session
```

- [ ] **Step 2: Review the SKILL.md for completeness**

Verify: frontmatter has `name`, `description`, `user-invokable`. Body covers all 8 steps. References are pointed to correctly. Fast-track and normal flow are both handled. Adaptive patterns logic is complete.

- [ ] **Step 3: Commit**

```bash
git add .claude/skills/boost/SKILL.md
git commit -m "feat(boost): add core SKILL.md with full prompt enhancement flow"
```

---

### Task 4: Create the Cursor MDC Rule

**Files:**
- Create: `.cursor/rules/boost.mdc`

This is a thin trigger rule that tells Cursor when to activate the boost skill.

- [ ] **Step 1: Create the boost.mdc file**

```markdown
---
description: Invoke when user includes /boost or /boost! in their prompt to restructure and enhance it before execution
alwaysApply: false
globs:
---

# Boost — Prompt Enhancer

When the user includes `/boost` or `/boost!` in their prompt, activate the boost skill at `.cursor/skills/boost/SKILL.md` to restructure their rough prompt into a clear, structured, context-rich prompt before execution.

See the full skill instructions in `.cursor/skills/boost/SKILL.md`.
```

- [ ] **Step 2: Commit**

```bash
git add .cursor/rules/boost.mdc
git commit -m "feat(boost): add Cursor MDC trigger rule for /boost invocation"
```

---

### Task 5: Create the Cursor SKILL.md

**Files:**
- Create: `.cursor/skills/boost/SKILL.md`
- Create: `.cursor/skills/boost/references/task-templates.md` (copy from Claude Code)
- Create: `.cursor/skills/boost/references/context-discovery.md` (copy from Claude Code)

The Cursor version is ~90% identical to the Claude Code version, with minor adaptations for Cursor's agent behavior.

- [ ] **Step 1: Create the Cursor SKILL.md**

```markdown
---
name: boost
description: Use when user appends or prefixes /boost to their prompt. Restructures rough prompts into clear, structured, context-rich prompts before execution. Supports /boost! for auto-execute without confirmation.
---

# Boost — Prompt Enhancer

Transform rough, unstructured prompts into clear, structured, context-rich prompts before execution.

## Invocation

The user triggers this skill by including `/boost` or `/boost!` in their prompt:
- **Prefix:** `/boost refactor the auth module`
- **Suffix:** `refactor the auth module /boost`
- **Fast-track:** `refactor the auth module /boost!` (skip confirmation, auto-execute)

## Core Flow

```
1. CAPTURE raw prompt (strip /boost or /boost! trigger)
2. DETECT task type from keywords
3. DISCOVER project context (read references/context-discovery.md)
4. STRUCTURE prompt using matching template (read references/task-templates.md)
5. PRESENT enhanced prompt to user
6. DECIDE: confirm, edit, or auto-execute
7. EXECUTE the structured prompt
8. SUGGEST pattern additions if unknown terms found
```

## Step 1: Capture

Strip `/boost` or `/boost!` from the user's input. The remaining text is the raw prompt.

- If `/boost!` was used, set fast-track mode = ON (skip confirmation in Step 6)
- If `/boost` was used, set fast-track mode = OFF (show confirmation)
- The trigger can appear at the start or end of the prompt — handle both

## Step 2: Detect Task Type

Scan the raw prompt for keywords (case-insensitive) to classify into one of 7 categories:

| Category | Keywords |
|----------|----------|
| Debug | fix, bug, broken, error, not working, crash, failing, issue |
| Feature | add, create, build, implement, new, make, setup, introduce |
| Refactor | refactor, clean up, reorganize, simplify, messy, restructure, improve, optimize |
| Test | test, coverage, spec, assert, unit test, integration test |
| Review | review, check, audit, look at, examine, inspect |
| Docs | document, readme, explain, comment, describe, write docs |
| General | (fallback — no keywords matched) |

**Tie-breaking priority:** Debug > Feature > Refactor > Test > Review > Docs > General

Count keyword matches per category. Highest count wins. If tied, use the priority order above.

## Step 3: Discover Context

Read `references/context-discovery.md` for the full discovery rules.

**Quick summary of priority order:**
1. **Project config** (always) — `.cursorrules`, `.cursor/rules/`, `package.json`, `tsconfig`, etc.
2. **Git context** (if available) — recent commits, changed files, branch name
3. **File structure** (if prompt mentions modules) — directory listing, file previews
4. **Team patterns** (always) — `boost-patterns.md` from project root

Stay within ~2000 line budget. If `boost-patterns.md` does not exist, skip silently — you will offer to create it in Step 8.

**Track unresolved terms** during discovery: any module/component name in the raw prompt that could not be resolved to a file path or alias.

## Step 4: Structure the Prompt

Read `references/task-templates.md` and select the template matching the detected task type.

Fill in every field:
- **Task:** Rewrite the raw prompt as a clear one-line summary
- **Type:** The detected category name
- **Context:** Populate from discovered project context
- **Category-specific sections:** Fill using the raw prompt content and discovered context
- **Constraints:** Combine project conventions + task-type defaults
- **Success Criteria:** Derive measurable outcomes from the task description

If a field cannot be filled, write "Unknown — investigate" rather than omitting it.

## Step 5: Present

Display the enhanced prompt to the user in a clear, readable format.

Prefix it with:
```
**Boost enhanced your prompt:**
```

## Step 6: Decide

**If fast-track mode is ON** (`/boost!` was used):
- Skip confirmation entirely
- Proceed directly to Step 7

**If fast-track mode is OFF** (`/boost` was used):
- Show the enhanced prompt (Step 5)
- Ask: "Execute this enhanced prompt? (yes / edit / skip)"
  - **"yes" or "y"** — proceed to Step 7 with the enhanced prompt
  - **"edit"** — user provides modifications, incorporate them, then proceed to Step 7
  - **"skip" or "s"** — proceed to Step 7 immediately

## Step 7: Execute

Execute the structured prompt as if the user had typed it directly. The agent now works on the task with full context and clear structure.

IMPORTANT: Do not re-announce the prompt. Just begin working on the task described in the structured prompt.

## Step 8: Suggest Pattern Additions

After the task execution completes, if there were unresolved terms from Step 3:

1. Collect up to 3 unresolved terms
2. Present them to the user:
   ```
   Boost noticed terms it could add to your team patterns:
     - "[term]" — what does this refer to? (file path, module, service?)
   Want me to add these to boost-patterns.md?
   ```
3. **Never auto-write** — always ask first
4. If user confirms, append to the appropriate section in `boost-patterns.md`
5. If `boost-patterns.md` does not exist, offer to create it with a starter template

**Rules for suggestions:**
- Maximum 3 suggestions per invocation
- Never interrupt the main task — suggestions come AFTER execution only
- If the user declines, do not ask again for the same terms in the same session
```

- [ ] **Step 2: Copy reference files from Claude Code to Cursor**

```bash
mkdir -p .cursor/skills/boost/references
cp .claude/skills/boost/references/task-templates.md .cursor/skills/boost/references/task-templates.md
cp .claude/skills/boost/references/context-discovery.md .cursor/skills/boost/references/context-discovery.md
```

- [ ] **Step 3: Commit**

```bash
git add .cursor/skills/boost/
git commit -m "feat(boost): add Cursor skill with SKILL.md and shared reference files"
```

---

### Task 6: Create the Starter boost-patterns.md

**Files:**
- Create: `boost-patterns.md` (project root)

This is the team-shared patterns file with example entries that teams can customize.

- [ ] **Step 1: Create boost-patterns.md with starter template**

```markdown
# Boost Patterns

Team-shared knowledge base for the /boost skill. Edit this file to teach Boost your team's terminology, conventions, and project structure. This file is git-tracked so the whole team benefits.

## Aliases
<!-- Map team shorthand to file paths or descriptions -->
<!-- Example: auth module = src/auth/ (authentication & session management) -->
<!-- Example: DB = PostgreSQL with Prisma ORM -->
<!-- Example: FE = frontend, src/app/ (Next.js app router) -->

## Team Conventions
<!-- Patterns and rules your team follows -->
<!-- Example: All API responses use { data, error, status } shape -->
<!-- Example: Use zod for validation, never manual checks -->
<!-- Example: Tests go in __tests__/ next to source, not a separate test/ tree -->

## Common Tasks
<!-- Shortcuts for multi-step workflows -->
<!-- Example: "deploy" = build > test > push to main > Vercel auto-deploys -->
<!-- Example: "lint fix" = run eslint --fix then prettier --write -->

## Do Not Touch
<!-- Files or areas the AI agent should never modify -->
<!-- Example: src/legacy/ — migration in progress, do not refactor -->
<!-- Example: .env.production — never modify via AI agent -->
```

- [ ] **Step 2: Commit**

```bash
git add boost-patterns.md
git commit -m "feat(boost): add starter boost-patterns.md for team-shared conventions"
```

---

### Task 7: Initialize Git Repository and Final Commit

**Files:**
- All files created in Tasks 1-6

- [ ] **Step 1: Initialize git repo (if not already initialized)**

```bash
cd /Users/sanjaykumar/Documents/p1/pt-skill
git init
```

- [ ] **Step 2: Create .gitignore**

```
.DS_Store
node_modules/
*.log
```

- [ ] **Step 3: Stage and commit all files**

```bash
git add .
git commit -m "feat: initial boost skill — prompt enhancer for Claude Code and Cursor

Includes:
- Claude Code skill (.claude/skills/boost/)
- Cursor skill (.cursor/skills/boost/) with MDC trigger rule
- 7 structured task templates (refactor, debug, feature, review, test, docs, general)
- Context discovery rules with priority-based resolution
- Starter boost-patterns.md for team-shared adaptive patterns"
```

- [ ] **Step 4: Verify final file structure**

```bash
find . -type f -not -path './.git/*' -not -name '.DS_Store' | sort
```

Expected output:
```
./.claude/skills/boost/SKILL.md
./.claude/skills/boost/references/context-discovery.md
./.claude/skills/boost/references/task-templates.md
./.cursor/rules/boost.mdc
./.cursor/skills/boost/SKILL.md
./.cursor/skills/boost/references/context-discovery.md
./.cursor/skills/boost/references/task-templates.md
./.gitignore
./boost-patterns.md
./docs/superpowers/plans/2026-03-31-boost-skill.md
./docs/superpowers/specs/2026-03-31-boost-skill-design.md
```

---

### Task 8: Manual Testing — Claude Code

**Files:** None (testing only)

- [ ] **Step 1: Test skill discovery**

Open Claude Code in the project directory. Verify that `/boost` appears in the available skills list. If not, check that `.claude/skills/boost/SKILL.md` exists and has valid frontmatter.

- [ ] **Step 2: Test prefix invocation**

Type: `/boost fix the login page it crashes on submit`

Expected:
1. Skill activates
2. Detects "Debug" category (keywords: "fix", "crashes")
3. Discovers project context
4. Presents structured prompt with Debug template
5. Asks for confirmation

- [ ] **Step 3: Test suffix invocation**

Type: `add dark mode toggle to settings /boost`

Expected:
1. Skill activates
2. Detects "Feature" category (keywords: "add")
3. Presents structured prompt with Feature template
4. Asks for confirmation

- [ ] **Step 4: Test fast-track invocation**

Type: `clean up the utils folder /boost!`

Expected:
1. Skill activates
2. Detects "Refactor" category (keywords: "clean up")
3. Presents structured prompt briefly
4. Auto-executes WITHOUT asking for confirmation

- [ ] **Step 5: Test adaptive patterns suggestion**

Type: `/boost fix the gateway timeout issue`

Expected (if "gateway" is not in boost-patterns.md):
1. After execution, skill suggests adding "gateway" to patterns
2. If confirmed, appends to boost-patterns.md Aliases section

---

### Task 9: Manual Testing — Cursor

**Files:** None (testing only)

- [ ] **Step 1: Verify MDC rule loads**

Open Cursor in the project directory. Open a file and invoke the agent. The `boost.mdc` rule should appear as an available agent-requested rule.

- [ ] **Step 2: Test invocation in Cursor**

Type in Cursor chat: `refactor the user service it's too big /boost`

Expected:
1. Cursor activates the boost rule
2. Reads SKILL.md from `.cursor/skills/boost/`
3. Detects "Refactor" category
4. Presents structured prompt
5. Asks for confirmation

- [ ] **Step 3: Test that boost-patterns.md is shared**

Add an alias to `boost-patterns.md` via Claude Code. Verify that Cursor's boost skill reads the same alias in its next invocation.
