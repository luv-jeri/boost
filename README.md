<p align="center">
  <img src="https://img.shields.io/badge/%F0%9F%9A%80_Boost-Prompt_Enhancer-blueviolet?style=for-the-badge" alt="Boost — Prompt Enhancer" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="MIT License" />
  <img src="https://img.shields.io/badge/platforms-16%2B_tools-blue?style=flat-square" alt="16+ Platforms" />
  <img src="https://img.shields.io/badge/task_categories-7-orange?style=flat-square" alt="7 Task Categories" />
  <img src="https://img.shields.io/badge/iron_laws-7-red?style=flat-square" alt="7 Iron Laws" />
  <img src="https://img.shields.io/badge/Agent_Skills_Spec-compliant-brightgreen?style=flat-square" alt="Agent Skills Spec Compliant" />
</p>

<p align="center">
  <strong>Transform rough prompts into structured, context-rich prompts before your AI agent executes them.</strong>
</p>

<p align="center">
  Your team writes messy prompts. Boost fixes that.<br/>
  Just add <code>/boost</code> to any prompt.
</p>

---

## The Problem

> *"fix the login thing it's broken"*

Your AI agent gets this and starts guessing. No context. No constraints. No success criteria. It reads random files, makes assumptions, and delivers something you didn't ask for.

## The Solution

> *"fix the login thing it's broken /boost"*

Boost transforms that into a structured prompt with the right template, your project's tech stack, recent git changes, related files, and team conventions — **before** the agent starts working.

---

## Before & After

<table>
<tr>
<td width="50%">

### Without Boost

```
fix the login page it keeps
crashing when I click submit
```

The agent guesses what's wrong, reads random files, and maybe fixes it after 3 back-and-forth attempts.

</td>
<td width="50%">

### With Boost

```
## Task: Fix crash on login form submission
## Type: Debug
## Context:
  - Project: Next.js 14 with TypeScript
  - Recent: auth middleware updated 2 days ago
  - Files: src/app/login/page.tsx, src/lib/auth.ts
## Symptoms: Crash on submit button click
## Investigation Starting Points:
  - Form submit handler
  - Auth middleware (recently changed)
## Constraints:
  - Maintain auth API contracts
## Success Criteria:
  - Login submits without crashing
  - All auth tests pass
```

The agent knows exactly what to do, where to look, and how to verify the fix.

</td>
</tr>
</table>

---

## Quick Start

### Installation

<details>
<summary><strong>Claude Code</strong> (recommended)</summary>

```bash
claude plugins install boost
```
</details>

<details>
<summary><strong>Cursor</strong></summary>

Copy the `skills/boost/` directory to `.cursor/skills/boost/` in your project.
</details>

<details>
<summary><strong>Gemini CLI</strong></summary>

Copy the `skills/boost/` directory to `.gemini/skills/boost/` in your project.
</details>

<details>
<summary><strong>OpenAI Codex</strong></summary>

Copy the `skills/boost/` directory to `.agents/skills/boost/` in your project.
</details>

<details>
<summary><strong>Any Agent Skills-compatible tool</strong></summary>

This plugin follows the [Agent Skills spec](https://agentskills.io). Copy `skills/boost/` to your tool's skills directory.

Supported: Claude Code, Cursor, Gemini CLI, Codex, Copilot, Windsurf, and 10+ more.
</details>

### Usage

```bash
# Suffix (most natural)
refactor the auth module it's messy /boost

# Prefix
/boost fix the login crash

# Fast-track (skip confirmation, auto-execute)
add dark mode /boost!

# Bootstrap team patterns from codebase
/boost --init
```

---

## How It Works

```
You type:  "fix the login crash /boost"
                    |
            1. Capture prompt
                    |
            2. Already structured? ──yes──> Execute as-is
                    |no
            3. Detect task type (Debug)
                    |
            4. Discover context
               ├─ Tech stack (Next.js, TypeScript)
               ├─ Recent commits (auth middleware changed)
               ├─ Related files (login/page.tsx)
               └─ Team patterns (boost-patterns.md)
                    |
            5. Apply Debug template
                    |
            6. Show enhanced prompt
                    |
            7. You confirm? ──yes──> Execute
                    |
            8. Agent works with full context
                    |
            9. Suggest new patterns
```

---

## Task Categories

Boost auto-detects what you're trying to do and applies the right template:

| | Category | Trigger Keywords | What Boost Adds |
|---|----------|-----------------|-----------------|
| :bug: | **Debug** | fix, bug, error, crash | Symptoms, expected/actual, investigation points |
| :sparkles: | **Feature** | add, create, build, implement | Requirements, acceptance criteria, edge cases |
| :recycle: | **Refactor** | refactor, clean up, simplify | Current state, target state, boundaries |
| :test_tube: | **Test** | test, coverage, spec | Test targets, types, edge cases, mocking strategy |
| :mag: | **Review** | review, audit, check | Scope, focus areas, severity-based output |
| :book: | **Docs** | document, explain, readme | Audience, scope, format, examples |
| :gear: | **General** | *(fallback)* | Goal, approach, constraints |

---

## Team Collaboration

Boost is built for teams. The shared `boost-patterns.md` file is your team's prompt knowledge base.

### Getting Started (30 seconds)

```bash
# 1. Bootstrap patterns from your codebase
/boost --init

# 2. Review and commit
git add boost-patterns.md
git commit -m "feat: add boost team patterns"

# 3. Everyone on the team now gets shared context
```

### How It Works for Teams

```
boost-patterns.md (git-tracked, team-shared)
├── Aliases:        "auth module" = src/auth/
├── Conventions:    "Use zod for validation"
├── Common Tasks:   "deploy" = build > test > push
└── Do Not Touch:   src/legacy/ — migration in progress
```

**Every team member benefits:**
- New devs get the team's vocabulary instantly
- Prompts are consistently structured across the team
- AI agents respect team conventions and protected areas
- Terminology is resolved automatically (no more "what's the auth module?")

### Adaptive Learning

Boost learns your team's terminology over time:

```
You: "fix the gateway timeout /boost"

Boost: ✅ Enhanced and executed.

  Boost noticed a term it could add to patterns:
    - "gateway" — what does this refer to? (file path, module, service?)

  Want me to add this to boost-patterns.md?
```

Every team member's usage contributes suggestions. Over time, your patterns file becomes a comprehensive project glossary.

### Example: boost-patterns.md

```markdown
## Aliases
- auth module = src/auth/ (authentication & session management)
- DB = PostgreSQL with Prisma ORM
- FE = frontend, src/app/ (Next.js app router)
- BE = backend, src/api/ (API routes)
- gateway = src/gateway/ (API gateway, rate limiting)

## Team Conventions
- All API responses use { data, error, status } shape
- Use zod for validation, never manual checks
- Tests go in __tests__/ next to source
- Feature branches: feat/TICKET-description

## Common Tasks
- "deploy" = build > test > push to main > Vercel auto-deploys
- "lint fix" = run eslint --fix then prettier --write
- "db reset" = prisma migrate reset && prisma db seed

## Do Not Touch
- src/legacy/ — migration in progress, do not refactor
- .env.production — never modify via AI agent
- migrations/ — never edit existing migration files
```

---

## Smart Features

| Feature | Description |
|---------|-------------|
| **Passthrough Detection** | Already wrote a detailed prompt? Boost detects structure and executes as-is — no over-processing |
| **Context Budget** | Stays under 2,000 lines to protect your context window — never bloats your session |
| **Secrets Avoidance** | Never reads `.env`, credentials, keys, or tokens during context discovery |
| **Adaptive Learning** | Suggests new patterns when it encounters unknown team terminology |
| **Auto-Bootstrap** | `/boost --init` scans your codebase and generates a starter patterns file |
| **7 Iron Laws** | Built-in guardrails prevent the AI from guessing, over-engineering, or ignoring conventions |
| **Rationalization Prevention** | 11-point red flags table stops the AI from rationalizing bad behavior |
| **Multi-Task Detection** | If your prompt has two distinct tasks, Boost asks to split them instead of merging |

---

## Why Boost?

| | Without Boost | With Boost |
|---|---|---|
| **Context** | Agent guesses your tech stack | Agent knows Next.js 14, TypeScript, Prisma |
| **Focus** | Agent reads random files | Agent starts from recently changed files |
| **Constraints** | Agent changes whatever it wants | Agent respects team conventions |
| **Success** | "Does it work?" | Specific, measurable criteria |
| **Consistency** | Every prompt is different | Same structure, every time |
| **Onboarding** | New devs write vague prompts | New devs get team context automatically |

---

## Architecture

```
skills/boost/
├── SKILL.md                 # Trigger file (<150 words)
├── references/
│   ├── flow.md              # Complete 9-step process
│   ├── task-templates.md    # 7 category templates
│   ├── context-discovery.md # 4-priority context system
│   ├── red-flags.md         # Iron laws & guardrails
│   └── prompt-passthrough.md # Structure detection
├── examples/
│   └── before-after.md      # 7 transformations
tests/
├── eval-triggers.md         # 20 trigger accuracy tests
└── eval-quality.md          # Quality grading rubric
patterns/
└── boost-patterns.md        # Starter template
```

**Progressive disclosure:** Only `SKILL.md` loads initially (~145 words). Reference files load on demand. Your context window stays clean.

---

## Platform Support

Built on the [Agent Skills spec](https://agentskills.io) — works everywhere:

| Platform | Status | Installation |
|----------|--------|-------------|
| Claude Code | Supported | `claude plugins install boost` |
| Cursor | Supported | Copy to `.cursor/skills/` |
| Gemini CLI | Supported | Copy to `.gemini/skills/` |
| OpenAI Codex | Supported | Copy to `.agents/skills/` |
| GitHub Copilot | Supported | Copy to skills directory |
| Windsurf | Supported | Copy to `.windsurf/skills/` |
| Any Agent Skills tool | Supported | Copy to tool's skills location |

---

## Evaluation Framework

Boost includes built-in quality assurance:

- **Trigger Tests** — 20 scenarios validating when Boost activates vs stays silent (90% accuracy threshold)
- **Quality Rubric** — 4-dimension grading (Task Clarity, Context Completeness, Constraints Relevance, Success Criteria) with 14/20 minimum threshold

See `tests/eval-triggers.md` and `tests/eval-quality.md`.

---

## Contributing

See `CLAUDE.md` for contributor conventions. Key rules:

- `SKILL.md` must stay under 150 words
- All logic lives in `references/` (loaded on demand)
- Test changes with the eval framework
- Follow progressive disclosure: metadata → SKILL.md → references → examples

---

## License

MIT — see [LICENSE](LICENSE)

---

<p align="center">
  <strong>Stop writing bad prompts. Start boosting.</strong>
</p>
