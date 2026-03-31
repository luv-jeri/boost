# Boost — Prompt Enhancer

Transform rough prompts into structured, context-rich prompts before your AI coding agent executes them.

Your team writes messy prompts. Boost fixes that. Just add `/boost` to any prompt and it gets restructured with the right template, project context, and clear success criteria — before the agent starts working.

## Installation

### Claude Code
```bash
claude plugins install boost
```

### Cursor
Copy the `skills/boost/` directory to `.cursor/skills/boost/` in your project.

### Gemini CLI
Copy the `skills/boost/` directory to `.gemini/skills/boost/` in your project.

### Codex
Copy the `skills/boost/` directory to `.agents/skills/boost/` in your project.

### Any Agent Skills-compatible tool
This plugin follows the [Agent Skills spec](https://agentskills.io). Copy the `skills/boost/` directory to your tool's skills location.

## Usage

Add `/boost` to any prompt — prefix or suffix:

```
refactor the auth module it's messy /boost
/boost fix the login crash
add dark mode /boost!          ← fast-track (skip confirmation)
```

### What happens

1. **Detects** your task type (debug, feature, refactor, test, review, docs, or general)
2. **Discovers** project context (tech stack, recent changes, related files, team patterns)
3. **Structures** your prompt with the right template
4. **Shows** you the enhanced prompt for review
5. **Executes** once you confirm (or auto-executes with `/boost!`)
6. **Learns** your team's terminology over time via `boost-patterns.md`

### Before & After

**You type:**
```
fix the login page it keeps crashing when I click submit /boost
```

**Boost produces:**
```
## Task: Fix crash on login form submission
## Type: Debug
## Context:
  - Project: Next.js 14 with TypeScript
  - Tech stack: React, Prisma, PostgreSQL
  - Recent changes: auth middleware updated 2 days ago
  - Related files: src/app/login/page.tsx, src/lib/auth.ts
## Symptoms: Login page crashes on submit button click
## Expected Behavior: Form submits and authenticates user
## Actual Behavior: Crash on submit
## Investigation Starting Points:
  - Form submit handler in login page
  - Auth middleware (recently changed)
## Constraints:
  - Maintain existing auth API contracts
  - Maintain test coverage
## Success Criteria:
  - Login form submits without crashing
  - All auth tests pass
```

## Team Patterns

Copy `patterns/boost-patterns.md` to your project root and teach Boost your team's language:

```markdown
## Aliases
- auth module = src/auth/
- FE = frontend, src/app/
- BE = backend, src/api/

## Team Conventions
- Use zod for validation
- Tests in __tests__/ next to source

## Do Not Touch
- src/legacy/ — migration in progress
```

Boost reads this file automatically and uses it to resolve shorthand, apply conventions, and warn about protected areas. It also suggests new entries when it encounters terms it doesn't recognize.

## Task Categories

| Category | Trigger Keywords | What Boost Adds |
|----------|-----------------|-----------------|
| Debug | fix, bug, error, crash | Symptoms, expected/actual, investigation points |
| Feature | add, create, build, implement | Requirements, acceptance criteria, edge cases |
| Refactor | refactor, clean up, simplify | Current state, target state, boundaries |
| Test | test, coverage, spec | Test targets, types, edge cases, mocking |
| Review | review, audit, check | Scope, focus areas, output format |
| Docs | document, explain, readme | Audience, scope, format, examples |
| General | (fallback) | Goal, approach, constraints |

## Smart Features

- **Passthrough detection** — If your prompt is already well-structured, Boost executes it as-is
- **Context budget** — Stays under 2000 lines to protect your context window
- **Secrets avoidance** — Never reads .env files, credentials, or keys
- **Adaptive learning** — Suggests additions to boost-patterns.md when it encounters unknown terms

## License

MIT
