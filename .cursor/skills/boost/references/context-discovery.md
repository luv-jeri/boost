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
