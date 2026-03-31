<p align="center">
  <img src="https://img.shields.io/badge/%F0%9F%9A%80_Boost-Prompt_Enhancer-blueviolet?style=for-the-badge" alt="Boost — Prompt Enhancer" />
  <img src="https://img.shields.io/badge/%F0%9F%8E%AF_Pixel-Figma_to_Perfect_UI-blue?style=for-the-badge" alt="Pixel — Figma to Perfect UI" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="MIT License" />
  <img src="https://img.shields.io/badge/platforms-16%2B_tools-blue?style=flat-square" alt="16+ Platforms" />
  <img src="https://img.shields.io/badge/skills-2-orange?style=flat-square" alt="2 Skills" />
  <img src="https://img.shields.io/badge/Agent_Skills_Spec-compliant-brightgreen?style=flat-square" alt="Agent Skills Spec Compliant" />
</p>

<p align="center">
  <strong>Agent skills that help your team ship faster — better prompts, pixel-perfect UI.</strong>
</p>

---

## Skills

| Skill | Trigger | What It Does |
|-------|---------|-------------|
| **Boost** | `/boost` | Transforms rough prompts into structured, context-rich prompts before your AI agent executes them |
| **Pixel** | `/pixel` | Transforms Figma designs into pixel-perfect UI through design maps and incremental build-verify cycles |

---

## Boost — Prompt Enhancer

> *"fix the login thing it's broken"* becomes a structured prompt with task type, context, constraints, and success criteria.

### The Problem

Your AI agent gets vague prompts and starts guessing. No context. No constraints. No success criteria.

### The Solution

Add `/boost` to any prompt. Boost detects the task type, discovers project context, and structures everything before the agent starts working.

### Before & After

<table>
<tr>
<td width="50%">

#### Without Boost

```
fix the login page it keeps
crashing when I click submit
```

The agent guesses what's wrong, reads random files, and maybe fixes it after 3 back-and-forth attempts.

</td>
<td width="50%">

#### With Boost

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

</td>
</tr>
</table>

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

### Task Categories

| Category | Trigger Keywords | What Boost Adds |
|----------|-----------------|-----------------|
| **Debug** | fix, bug, error, crash | Symptoms, expected/actual, investigation points |
| **Feature** | add, create, build, implement | Requirements, acceptance criteria, edge cases |
| **Refactor** | refactor, clean up, simplify | Current state, target state, boundaries |
| **Test** | test, coverage, spec | Test targets, types, edge cases, mocking strategy |
| **Review** | review, audit, check | Scope, focus areas, severity-based output |
| **Docs** | document, explain, readme | Audience, scope, format, examples |
| **General** | *(fallback)* | Goal, approach, constraints |

### Team Collaboration

Boost uses a shared `boost-patterns.md` file as your team's prompt knowledge base:

```bash
# Bootstrap patterns from your codebase
/boost --init

# Commit so everyone gets shared context
git add boost-patterns.md && git commit -m "feat: add boost team patterns"
```

The patterns file maps team terminology, conventions, common tasks, and protected areas — so every team member's prompts get the same shared context.

---

## Pixel — Figma to Pixel-Perfect UI

> *"Build this Figma design"* becomes a structured design map, Q&A session, and incremental build with verification at every step.

### The Problem

You give the AI a Figma URL or screenshot and it builds something that's "close" — wrong spacing, hardcoded colors, missing hover states, no responsive behavior, skipped micro-interactions.

### The Solution

Add `/pixel` to your prompt. Pixel creates a complete design map first, resolves ambiguities through Q&A, then builds one component at a time with verification checkpoints.

### How It Works

```
You type:  "/pixel https://figma.com/design/abc123"
                    |
           Phase 1: Design Map
                    |
            1. Detect input (Figma MCP, screenshot, specs)
                    |
            2. Create Design Map
               ├─ Layout structure (hierarchy, flexbox/grid)
               ├─ Component inventory (variants, states)
               ├─ Design tokens (mapped to your system)
               ├─ Responsive behavior (breakpoints)
               ├─ Micro-interactions (hover, transitions)
               ├─ Assets (icons, images)
               └─ Open questions (ambiguities)
                    |
            3. Q&A Gate — resolve questions before code
                    |
           Phase 2: Incremental Build
                    |
            4. Generate build order (outside-in)
                    |
            5. Build → Verify → Next component
                    |
            6. Final 6-point audit
               ├─ Pixel comparison
               ├─ Token audit (no hardcoded values)
               ├─ Responsive audit
               ├─ State audit
               ├─ Interaction audit
               └─ Accessibility baseline
```

### Usage

```bash
# With Figma URL
/pixel https://figma.com/design/abc123

# With screenshot already in context
/pixel

# With pasted specs from Figma Dev Mode
/pixel

# Relaxed mode (checkpoint after sections, not components)
/pixel --relaxed https://figma.com/design/abc123

# Fast-track (no checkpoints, final audit only)
/pixel!
```

### Input Support

| Input Method | What It Provides |
|---|---|
| **Figma MCP** | Precise token values, component hierarchy, exact styles |
| **Screenshots** | Visual layout truth, overall composition |
| **Dev Mode Export** | Exact CSS values from the designer |
| **Multiple inputs** | Cross-referenced for highest accuracy |

### Key Features

| Feature | Description |
|---------|-------------|
| **Design Map** | 7-section structured analysis before any code is written |
| **Q&A Gate** | All ambiguities resolved before implementation begins |
| **Token Matching** | Every value mapped to your design system — no hardcoded colors or spacing |
| **Incremental Build** | One component at a time with verification after each |
| **6-Point Audit** | Pixel comparison, tokens, responsive, states, interactions, accessibility |
| **Stack-Aware** | Works with Tailwind, CSS modules, plain CSS, shadcn, Radix, MUI |
| **7 Iron Laws** | Non-negotiable guardrails prevent guessing and skipping |
| **Graceful Degradation** | Figma MCP fails? Falls back with transparency, never silently |

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

Copy the `skills/` directory to `.cursor/skills/` in your project.
</details>

<details>
<summary><strong>Gemini CLI</strong></summary>

Copy the `skills/` directory to `.gemini/skills/` in your project.
</details>

<details>
<summary><strong>OpenAI Codex</strong></summary>

Copy the `skills/` directory to `.agents/skills/` in your project.
</details>

<details>
<summary><strong>Any Agent Skills-compatible tool</strong></summary>

This plugin follows the [Agent Skills spec](https://agentskills.io). Copy `skills/` to your tool's skills directory.

Supported: Claude Code, Cursor, Gemini CLI, Codex, Copilot, Windsurf, and 10+ more.
</details>

---

## Architecture

```
skills/
├── boost/
│   ├── SKILL.md                 # Trigger file (<150 words)
│   ├── references/
│   │   ├── flow.md              # Complete 9-step process
│   │   ├── task-templates.md    # 7 category templates
│   │   ├── context-discovery.md # 4-priority context system
│   │   ├── red-flags.md         # Iron laws & guardrails
│   │   └── prompt-passthrough.md # Structure detection
│   ├── examples/
│   │   └── before-after.md      # 7 transformations
│   └── tests/
│       ├── eval-triggers.md     # 20 trigger accuracy tests
│       └── eval-quality.md      # Quality grading rubric
├── pixel/
│   ├── SKILL.md                 # Trigger file (<150 words)
│   ├── references/
│   │   ├── flow.md              # 2-phase process (Map → Build)
│   │   ├── design-map.md        # 7-section design map
│   │   ├── input-detection.md   # Figma MCP, screenshot, specs
│   │   ├── token-extraction.md  # Token discovery & matching
│   │   ├── verification.md      # Checkpoints & final audit
│   │   └── red-flags.md         # Iron laws & guardrails
│   ├── examples/
│   │   └── design-map-example.md # Complete example
│   └── tests/
│       ├── eval-triggers.md     # 20 trigger accuracy tests
│       └── eval-quality.md      # Quality grading rubric
patterns/
└── boost-patterns.md            # Team shared knowledge
```

**Progressive disclosure:** Only `SKILL.md` loads initially (~145 words per skill). Reference files load on demand. Your context window stays clean.

---

## Evaluation Framework

Both skills include built-in quality assurance:

- **Trigger Tests** — 20 scenarios per skill validating when to activate vs stay silent (90% accuracy threshold)
- **Quality Rubric** — Multi-dimension grading with minimum score thresholds

See each skill's `tests/` directory.

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

## Contributing

See `CLAUDE.md` for contributor conventions. Key rules:

- Each skill lives in `skills/<name>/`
- `SKILL.md` must stay under 150 words
- All logic lives in `references/` (loaded on demand)
- Test changes with the eval framework
- Follow progressive disclosure: metadata → SKILL.md → references → examples

---

## License

MIT — see [LICENSE](LICENSE)

---

<p align="center">
  <strong>Better prompts. Pixel-perfect UI. Ship faster.</strong>
</p>
