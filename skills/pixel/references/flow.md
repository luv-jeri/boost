# Pixel — Complete Process Flow

## Overview

Two distinct phases. Phase 1 creates a complete understanding. Phase 2 builds from that understanding.

```
/pixel <input>
     |
Phase 1: Design Map
     |
  1. INPUT DETECTION — what design input is available?
     |
  2. DESIGN MAP — create structured 7-section analysis
     |
  3. Q&A GATE — resolve all ambiguities before code
     |
Phase 2: Incremental Build
     |
  4. BUILD ORDER — generate component build sequence
     |
  5. BUILD-VERIFY LOOP — build one, verify one, repeat
     |
  6. FINAL AUDIT — 6-point full-page verification
```

---

## Phase 1 — Design Map

### Step 1: Input Detection

See `input-detection.md` for full logic.

Detect what's available in this priority order:
1. Figma URL in prompt → use Figma MCP if available
2. Image/screenshot in context → visual analysis
3. Pasted CSS/spacing specs → parse as dev mode export
4. None found → ask: "Share your design — Figma URL, screenshot, or paste the specs"

When multiple inputs are available, cross-reference them for highest accuracy.

### Step 2: Design Map Creation

See `design-map.md` for the full 7-section structure.

Create a structured Design Map covering:
1. Layout Structure
2. Component Inventory
3. Design Tokens
4. Responsive Behavior
5. Micro-interactions
6. Assets
7. Open Questions

Present the complete map to the developer.

### Step 3: Q&A Gate

**This is a hard gate. No code is written until it passes.**

Present the design map. The developer reviews and answers open questions. The skill does NOT proceed to Phase 2 until:
- All open questions are answered, OR
- Questions are explicitly marked "decide later"

If there are no open questions, confirm: "Design map complete, no open questions. Ready to build?"

---

## Phase 2 — Incremental Build

### Step 4: Build Order

From the design map, generate a build order:
- Outermost layout first (page shell, grid/flex container)
- Then components top-to-bottom, left-to-right
- Nested components after their parent

Example:
```
1. Page layout shell (grid/flex container)
2. Header component
3. Sidebar component
4. Main content area
   4a. Card component
   4b. Table component
5. Footer
```

Present the build order for approval before starting.

### Step 5: Build-Verify Loop

For each item in the build order:

1. **Build** — implement the component using design map specs
   - Use exact tokens from the design map
   - Use the project's styling approach (Tailwind, CSS modules, plain CSS)
   - Semantic HTML first, then styling
   - Include all states identified in the component inventory

2. **Self-check** — before showing the dev, verify:
   - Tokens match design map (no hardcoded values)
   - Spacing matches design map
   - All states from component inventory are implemented
   - Responsive behavior matches design map

3. **Checkpoint** — pause and let the dev verify
   - Show what was built
   - Note any deviations or judgment calls
   - Wait for approval before proceeding

4. **Fix** — if deviations found, fix before moving to next component

**Checkpoint frequency** depends on invocation mode:
- `/pixel` (default, strict) — checkpoint after every component
- `/pixel --relaxed` — checkpoint after each major section
- `/pixel!` (fast-track) — no checkpoints, build everything, then final audit

### Step 6: Final Audit

See `verification.md` for the complete audit checklist.

After all components are built, run a 6-point audit:
1. Pixel Comparison
2. Token Audit
3. Responsive Audit
4. State Audit
5. Interaction Audit
6. Accessibility Baseline

Each item: **Pass**, **Fail (with deviation)**, or **Skipped (with reason)**.

Present summary table. Task is complete only when all non-skipped items pass.

---

## Edge Cases

**Figma design has multiple pages/frames:**
Ask which frame to implement. Don't assume.

**Design references components not yet built:**
Flag as a dependency in the build order. Ask: "This component depends on [X] which doesn't exist yet. Build it as part of this task or stub it?"

**Design conflicts with existing code:**
Flag in the design map open questions. Never silently override existing design system decisions.

**Very large design (20+ components):**
Break into logical sections. Complete one section's full build-verify cycle before starting the next. Each section gets its own mini final audit.

**Design is a single component, not a page:**
Skip the page layout step. Build order is just the component and its sub-parts. Same verification process applies.
