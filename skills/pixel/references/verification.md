# Pixel — Verification & Final Audit

## Purpose

Ensure every component and the full page match the Figma design exactly. Catch deviations before the dev considers the work done.

## Checkpoint Verification (Per Component)

Run after each component (strict mode) or section (relaxed mode).

### Visual Match
- [ ] Layout matches Figma — correct flex/grid, alignment, distribution
- [ ] Spacing matches — padding, margin, gap use correct tokens
- [ ] Typography matches — font, size, weight, line-height, color
- [ ] Colors match — all using design tokens, no hardcoded hex values
- [ ] Border radius, shadows, borders match design map

### Completeness
- [ ] All states from component inventory are implemented (or explicitly skipped per Q&A)
- [ ] Responsive behavior matches design map spec for this component
- [ ] Micro-interactions from design map are in place

### Code Quality
- [ ] Semantic HTML elements used (`section`, `nav`, `article`, `aside`, `header`, `footer`)
- [ ] No inline styles where tokens exist
- [ ] Component is isolated — no layout leaking into parent/children
- [ ] Props/variants are clean (no unnecessary complexity)
- [ ] Accessibility: proper aria labels, focus states, contrast

### Checkpoint Behavior

**On pass:** Proceed to next component in build order.

**On fail (strict mode):** Fix deviations before proceeding. Show what was wrong and what was fixed.

**On fail (relaxed mode):** Note deviations, continue building. Fix all at section checkpoint.

**On fail (fast mode):** No checkpoints during build. All deviations caught in final audit.

---

## Final Audit (Full Page)

Run after all components are assembled into the complete page/view.

### 1. Pixel Comparison

Compare the built UI against the Figma design:
- Overall layout proportions and alignment
- Spacing consistency across the page
- Visual rhythm (are repeated elements consistent?)
- Color application across all components
- Typography hierarchy (do headings, body, captions feel right?)

**Output:** List every deviation found, no matter how small. Include:
- What: "Card padding is 20px, design map says 24px"
- Where: component name and CSS property
- Fix: specific token to use

### 2. Token Audit

Verify no hardcoded values slipped through:
- Grep for hex color codes (`#[0-9A-Fa-f]{3,8}`) in the built components
- Grep for raw pixel values in inline styles
- Check for Tailwind arbitrary values (`bg-[#xxx]`, `p-[xxpx]`) that should be tokens
- Verify all colors, spacing, typography reference the design system

**Output:** List of any hardcoded values with the correct token replacement.

### 3. Responsive Audit

Walk through each breakpoint from the design map:
- Does the layout change correctly at each breakpoint?
- Are elements hidden/shown as specified?
- Does text reflow properly?
- Do images/cards resize/restack correctly?
- Is touch target size adequate on mobile (44x44px minimum)?

**Output:** Pass/fail per breakpoint with specific deviations.

### 4. State Audit

For every component in the inventory, verify each state:
- Default rendering
- Hover state (visual change, cursor)
- Active/pressed state
- Focus state (keyboard navigation visible ring)
- Disabled state (opacity, cursor, non-interactive)
- Loading state (skeleton, spinner, or placeholder)
- Error state (styling, message placement)
- Empty state (no data messaging)

**Output:** Matrix of component x state, each marked pass/fail/skipped.

### 5. Interaction Audit

For every micro-interaction in the design map:
- Does the animation trigger correctly?
- Is the duration appropriate (not too fast, not too slow)?
- Is the easing curve smooth?
- Does the animation work on all target breakpoints?
- No janky or stuttering transitions?

**Output:** Pass/fail per interaction with notes.

### 6. Accessibility Baseline

Not a full a11y audit, but catch the basics:
- [ ] Semantic HTML structure (headings in order, landmarks present)
- [ ] Color contrast meets WCAG 2.1 AA (4.5:1 for text, 3:1 for large text)
- [ ] All interactive elements are keyboard accessible
- [ ] Focus order is logical (matches visual order)
- [ ] Images have alt text
- [ ] Form inputs have labels
- [ ] No information conveyed by color alone

**Output:** Pass/fail per item.

---

## Audit Summary Table

Present results as:

| Audit | Status | Issues |
|---|---|---|
| Pixel Comparison | Pass / Fail (N issues) | [list if fail] |
| Token Audit | Pass / Fail (N hardcoded) | [list if fail] |
| Responsive Audit | Pass / Fail (N breakpoints) | [list if fail] |
| State Audit | Pass / Fail (N missing) | [list if fail] |
| Interaction Audit | Pass / Fail (N issues) | [list if fail] |
| Accessibility | Pass / Fail (N issues) | [list if fail] |

**Task is complete only when all non-skipped audits pass.**

If any audit fails, fix the issues and re-run only the failed audits. No need to re-run passing audits.
