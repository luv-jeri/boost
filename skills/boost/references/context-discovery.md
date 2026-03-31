# Boost — Context Discovery Rules

## Overview

Discover project context to fill template fields. Follow the priority order below. Stay within a ~2000 line total budget to protect the context window.

## Priority 1 — Project Config (always read)

Read these files if they exist. Skip silently if missing.

**Platform instruction files** (read whichever exist):
- `CLAUDE.md` — Claude Code conventions
- `GEMINI.md` — Gemini CLI conventions
- `AGENTS.md` — Codex / Cursor / universal conventions
- `.cursorrules` — Cursor legacy rules
- `.windsurfrules` — Windsurf rules
- `.cursor/rules/*.mdc` — Cursor MDC rules
- `.windsurf/rules/*.md` — Windsurf rules

**Tech stack detection:**
- `package.json` — Node.js/JavaScript: extract `name`, `dependencies`, `devDependencies`
- `pyproject.toml` — Python: extract project name, dependencies
- `Cargo.toml` — Rust: extract package info and dependencies
- `go.mod` — Go: extract module name and dependencies
- `pom.xml` — Java/Kotlin: extract groupId, artifactId, dependencies
- `Gemfile` — Ruby: extract gem dependencies
- `composer.json` — PHP: extract dependencies
- `build.gradle` / `build.gradle.kts` — Gradle projects

**Code style:**
- `tsconfig.json` — TypeScript configuration
- `.eslintrc` / `.eslintrc.json` / `.eslintrc.js` / `eslint.config.js` — Linting rules
- `prettier.config.js` / `.prettierrc` — Formatting rules
- `biome.json` — Biome config
- `ruff.toml` / `pyproject.toml [tool.ruff]` — Python linting

**Extract:**
- Language and framework (e.g., "Next.js 14 with TypeScript")
- Key dependencies (e.g., "Prisma, zod, tailwindcss")
- Test framework (e.g., "vitest", "jest", "pytest")
- Build tool (e.g., "vite", "webpack", "turbopack")

**Budget:** ~500 lines max for Priority 1.

## Priority 2 — Git Context (if available)

Only run if git is initialized. Skip silently if not.

**Commands:**
- `git log --oneline -10` — last 10 commits
- `git diff --stat HEAD~3` — files changed in last 3 commits
- `git branch --show-current` — current branch name

**Extract:**
- Recently changed files that overlap with the mentioned module
- Commit messages hinting at ongoing work
- Branch name if it describes a feature or fix

**Budget:** ~100 lines max for Priority 2.

## Priority 3 — File Structure (conditional)

Only run if the raw prompt mentions specific modules, files, or code areas. Skip for general/docs requests.

**Steps:**
1. Resolve module names using boost-patterns.md aliases (e.g., "auth module" → `src/auth/`)
2. If no alias found, search for directories/files matching the mentioned term
3. List directory structure (1-2 levels deep) around matched paths
4. Read first 50 lines of directly mentioned files
5. Check for related test files (`__tests__/`, `*.test.*`, `*.spec.*` siblings)

**Secrets avoidance — NEVER read:**
- `.env`, `.env.*` files
- `credentials.json`, `secrets.json`, `*.key`, `*.pem`
- Files matching `*secret*`, `*credential*`, `*token*` in the filename

**Extract:**
- Exact file paths for "Related files" field
- Current state of mentioned code (for refactor/debug)
- Test file locations

**Budget:** ~1000 lines max for Priority 3.

## Priority 4 — Team Patterns (always read)

Read `boost-patterns.md` from the project root. If it doesn't exist, skip silently and offer to create it in Step 9.

**Extract:**
- Resolve aliases (e.g., "BE" → "backend, src/api/")
- Pull relevant team conventions for Constraints field
- Check "Do Not Touch" section for warnings
- Match "Common Tasks" entries

**Detecting unresolved terms — track any term that:**
1. Looks like a module/component name (capitalized, hyphenated, or quoted)
2. Is NOT found in boost-patterns.md aliases
3. Is NOT a standard programming term (e.g., "function", "class", "API")
4. Could NOT be resolved to a file path via Priority 3

These become candidates for Step 9 suggestions.

**Budget:** ~400 lines max for Priority 4.

## Edge Cases

| Scenario | Behavior |
|----------|----------|
| Brand new project (no config, no git, no patterns) | Fill Context with "New project — no conventions discovered yet" and proceed |
| Monorepo with multiple package.json | Read the nearest package.json to the mentioned module |
| Git in detached HEAD state | Skip git log, use `git show HEAD --oneline` for current commit only |
| boost-patterns.md is malformed | Skip silently, note: "boost-patterns.md could not be parsed" |
| Config file >500 lines | Read first 100 lines only, note "[truncated]" |
| No files match the mentioned module | Write "Unknown — investigate" in Related files, track as unresolved term |
| Multiple platform instruction files exist | Read all of them, concatenate conventions |

## Total Budget

| Priority | Max Lines |
|----------|-----------|
| 1. Project Config | ~500 |
| 2. Git Context | ~100 |
| 3. File Structure | ~1000 |
| 4. Team Patterns | ~400 |
| **Total** | **~2000** |

If any priority exceeds its budget, truncate and note "[truncated]" in the context output.
