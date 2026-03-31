# Pixel Skill — Design Spec

**Date:** 2026-04-01
**Status:** Draft
**Skill name:** `/pixel`
**Purpose:** Transform a Figma design into pixel-perfect, production-ready UI through structured analysis, Q&A, and incremental build-verify cycles.

---

## 1. Skill Identity & Trigger

**Name:** `/pixel`

**Trigger patterns:**
- `/pixel <figma-url>` — primary invocation with Figma link
- `/pixel` with a screenshot already in context
- `/pixel <description>` — when design specs are pasted as text

**Target stack:** React / Next.js primary. Styling varies (Tailwind, CSS modules, plain CSS, component libraries). Skill is stack-aware but not stack-locked.

**Input methods supported:** Figma MCP, screenshots, Figma Dev Mode exports, or any combination.

## 2. File Structure

```
skills/pixel/
├── SKILL.md                    # <150 words, trigger + what to read
├── references/
│   ├── flow.md                 # Complete process (2 phases, all steps)
│   ├── design-map.md           # How to build the design map artifact
│   ├── input-detection.md      # Figma MCP vs screenshot vs manual specs
│   ├── token-extraction.md     # CSS variables, design tokens, theme matching
│   ├── verification.md         # Checkpoint + final audit process
│   └── red-flags.md            # Iron laws specific to pixel
├── examples/
│   └── design-map-example.md   # Complete example of a design map artifact
└── tests/
    ├── eval-triggers.md        # Trigger accuracy tests
    └── eval-quality.md         # Output quality rubric
```

Follows Boost conventions: SKILL.md <150 words, all logic in `references/`, progressive disclosure.

## 3. Two-Phase Process

### Phase 1 — Design Map (understand before building)

#### Step 1: Input Detection

Detect what design input is available and adapt:

| Priority | Input Type | Detection | How to Extract |
|---|---|---|---|
| 1 | Figma MCP | URL contains `figma.com` + MCP tool available | Query MCP for node tree, styles, layout |
| 2 | Screenshot | Image in context (paste/path) | Visual analysis of the image |
| 3 | Dev Mode Export | Pasted text with CSS/spacing specs | Parse structured specs directly |
| 4 | None found | No design input detected | Ask: "Share your design — Figma URL, screenshot, or paste the specs" |

**Cross-referencing:** When multiple inputs are available (e.g., Figma URL + screenshot), cross-reference them. Figma MCP gives precise token values; screenshots give visual layout truth. Use both.

**Figma MCP Failure — Graceful Degradation:**
If the Figma MCP is configured but fails (auth error, rate limit, node not found):
- Do NOT silently fall back to screenshot-only
- Tell the dev: "Figma MCP failed: [reason]. I can proceed with screenshot analysis, but token extraction will be approximate. Want to fix MCP access first or continue?"

#### Step 2: Design Map Creation

Produce a structured **Design Map** artifact with these 7 sections:

1. **Layout Structure** — grid/flex hierarchy, sections, nesting depth, spacing rhythm
2. **Component Inventory** — every distinct component, its variants, and states (hover, active, disabled, loading, error, empty)
3. **Design Tokens** — colors, typography, spacing, border-radius, shadows — mapped to existing CSS variables/Tailwind classes where they exist, flagged as "new token needed" where they don't
4. **Responsive Behavior** — breakpoints, what changes at each (layout shifts, hidden elements, reflow). If Figma only shows one breakpoint, explicitly ask: "No mobile design provided — should I infer responsive behavior or build desktop-only?"
5. **Micro-interactions** — hover effects, transitions, animations, loading states. Extracted from Figma if present, otherwise asked about
6. **Assets** — icons, images, illustrations — what needs exporting, what's available in existing icon libraries
7. **Open Questions** — anything ambiguous, missing, or inconsistent in the design

#### Step 3: Q&A Gate

Present the design map to the dev. The dev reviews and answers open questions *before any code is written*. The skill does NOT proceed to Phase 2 until all questions are resolved or explicitly marked "decide later."

### Phase 2 — Incremental Build & Verify

#### Step 4: Build Order

From the design map, generate a build order — outermost layout first, then components top-to-bottom, left-to-right:

```
1. Page layout shell (grid/flex container)
2. Header component
3. Sidebar component
4. Main content area
   4a. Card component
   4b. Table component
   ...
5. Footer
```

#### Step 5: Build-Verify Loop

For each item in the build order:

1. **Build** — implement the component using the design map specs
2. **Self-check** — compare against the design map (tokens, spacing, responsive, states)
3. **Checkpoint** — pause and let the dev verify before moving on
4. **Fix** — if deviations found, fix before proceeding

**Checkpoint frequency modes:**
- **Strict** (`/pixel` default) — checkpoint after every component
- **Relaxed** (`/pixel --relaxed`) — checkpoint after each major section
- **Fast** (`/pixel!`) — no checkpoints, full build, then final audit only

#### Step 6: Final Audit

After all components are built, run a 6-point audit:

| Audit | What It Checks | How |
|---|---|---|
| 1. Pixel Comparison | Overall visual match to Figma | Side-by-side description, call out every deviation |
| 2. Token Audit | No hardcoded colors, spacing, typography | Grep for hex codes, px values not in the token system |
| 3. Responsive Audit | All breakpoints from design map work | Walk through each breakpoint, verify layout changes |
| 4. State Audit | All component states render correctly | List every state from design map, verify each |
| 5. Interaction Audit | Transitions, animations, hover effects work | Walk through each micro-interaction from design map |
| 6. Accessibility Baseline | Semantic HTML, contrast, focus, aria | Check contrast ratios, tab order, screen reader basics |

Each audit item gets: **Pass**, **Fail (with specific deviation)**, or **Skipped (with reason)**.

The skill marks the task complete only when all non-skipped items pass.

## 4. Token Extraction & Matching

### Step 1: Discover existing tokens

- Read `tailwind.config.js` / `tailwind.config.ts` for custom theme extensions
- Read CSS variable definitions (`:root` blocks, theme files)
- Check for component library theme config (shadcn `components.json`, Radix theme, MUI theme)
- Check `boost-patterns.md` for documented design conventions

### Step 2: Extract design values from Figma

- Colors (hex, with opacity)
- Typography (font family, size, weight, line-height, letter-spacing)
- Spacing (padding, margin, gap — exact pixel values)
- Border radius, shadows, borders
- Layout (flex/grid, alignment, distribution)

### Step 3: Match or flag

For each extracted value:
- **Exact match found** — use the token (`text-primary`, `var(--spacing-md)`, `p-4`)
- **Close match found** — flag in design map: "Figma says `#3B83F7`, closest token is `--color-primary: #3B82F6` — confirm with designer"
- **No match** — flag as "New token needed: `#FF6B35` — no existing token within ΔE < 3"

### Stack-aware output

The design map writes token mappings in the project's actual syntax:
- Tailwind project → `bg-primary`, `text-sm`, `p-4`
- CSS variables project → `var(--color-primary)`, `var(--font-sm)`
- Plain CSS project → document raw values with a note to add variables

## 5. Iron Laws

Non-negotiable. Violating any of these is a skill failure.

1. **NEVER write code before the design map is complete and reviewed** — the map is the contract
2. **NEVER hardcode values that exist as design tokens** — always use CSS variables, Tailwind theme values, or component library tokens
3. **NEVER skip responsive behavior** — if the Figma shows one breakpoint only, ask. Never silently build desktop-only
4. **NEVER assume a missing state means it doesn't exist** — hover, disabled, loading, error, empty states must be explicitly confirmed as "not needed" or implemented
5. **NEVER build multiple components without verifying the previous one** (in strict/default mode)
6. **NEVER invent design decisions** — if the Figma doesn't specify something (e.g., mobile layout, animation timing), ask. Write "Unknown — ask designer" in the map
7. **NEVER skip the token-matching step** — before using `bg-blue-500` or `#3B82F6`, check if the project has an existing token for that color

## 6. Red Flags — Rationalization Prevention

| Thought | Wrong Action | Correct Action |
|---|---|---|
| "This color is close enough to the token" | Use the closest token | Extract exact hex from Figma, find exact match or flag as new token |
| "Mobile will probably just stack" | Build a guessed responsive layout | Add to Open Questions in the design map |
| "This component is simple, no need to checkpoint" | Skip verification | Every component gets verified in strict mode |
| "The Figma doesn't show hover states" | Skip hover states | Add to Open Questions — hover states are almost always expected |
| "I'll add animations later" | Build static, never come back | Micro-interactions are part of the build order, not a follow-up |
| "This spacing looks like 16px" | Hardcode `16px` or guess `p-4` | Extract exact value from Figma, match to design system scale |
| "The design system doesn't have this token" | Invent one inline | Flag as "new token needed" in the design map, ask the dev |
| "I'll just use a div" | Non-semantic markup | Use semantic HTML (`section`, `nav`, `article`, `aside`) then style it |

## 7. Checkpoint Verification Checklist

### Per-Component Checkpoint

**Visual Match:**
- Layout matches Figma — correct flex/grid, alignment, distribution
- Spacing matches — padding, margin, gap use correct tokens
- Typography matches — font, size, weight, line-height, color
- Colors match — all using design tokens, no hardcoded hex

**Completeness:**
- All states implemented (or explicitly skipped per Q&A answers)
- Responsive behavior matches design map spec
- Micro-interactions from design map are in place

**Code Quality:**
- Semantic HTML elements used
- No inline styles where tokens exist
- Component is isolated — no layout leaking into parent/children
- Accessibility: proper aria labels, focus states, contrast

Failures block progress in strict mode.

## 8. Scope & Non-Goals

### In Scope
- Figma-to-code workflow with structured analysis
- Design token discovery and matching
- Incremental build with verification checkpoints
- Responsive behavior analysis and implementation
- Micro-interaction implementation
- Accessibility baseline

### Out of Scope (for v1)
- Automated visual regression testing / screenshot diffing tooling
- Figma plugin or custom MCP development
- Design system creation from scratch (skill uses existing tokens, flags gaps)
- Backend/API integration — skill focuses on UI layer only
- Storybook story generation (potential v2 feature)
