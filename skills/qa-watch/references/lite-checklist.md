# QA Watch — Detailed Lite Checklist

Supplementary detail for the 5 lite check categories. The core checklist is in SKILL.md — this file provides exhaustive per-category items for thoroughness.

---

## Category 1: Overflow

### Text
- [ ] Long single words don't break containers (`overflow-wrap: break-word`)
- [ ] Multi-line text is truncated with ellipsis OR wraps correctly
- [ ] Truncated text has a tooltip or expand for full content
- [ ] Text doesn't overlap adjacent elements
- [ ] Text at different font sizes still fits

### Containers
- [ ] Dynamic content containers have `overflow: auto` or `hidden`
- [ ] No unexpected horizontal scrollbar
- [ ] Modal/dialog content scrolls if exceeding viewport
- [ ] Dropdown menus stay within viewport bounds

### Images
- [ ] Images respect container bounds (`max-width: 100%`, `object-fit`)
- [ ] Broken image links show fallback
- [ ] Image aspect ratios are preserved

---

## Category 2: Missing States

### Error
- [ ] Network failure shows error message (not blank)
- [ ] API errors display user-friendly message
- [ ] Form validation errors shown inline at relevant fields
- [ ] Error state has recovery action (retry, go back)

### Loading
- [ ] Skeleton or spinner during initial data fetch
- [ ] Button shows loading indicator during async action
- [ ] Old data doesn't flash before new data loads

### Empty
- [ ] No-data state has helpful message and CTA
- [ ] No-results state suggests clearing filters
- [ ] First-time empty state explains what to do

### Disabled
- [ ] Disabled buttons have reduced opacity
- [ ] Disabled inputs are non-interactive with visual indication
- [ ] Cursor changes to `not-allowed` on disabled elements

---

## Category 3: Micro-interactions

### Hover
- [ ] Buttons change on hover (background, shadow, or scale)
- [ ] Links have hover indication
- [ ] Clickable cards/rows have hover indication
- [ ] Hover is distinct from active/pressed

### Focus
- [ ] Visible focus ring on all interactive elements
- [ ] Focus ring is consistent (not browser default blue)
- [ ] Focus order matches visual reading order
- [ ] Focus trapped in modals/overlays

### Feedback
- [ ] Click has visual response (pressed state, ripple)
- [ ] Form submit shows progress on button
- [ ] Success/error feedback after actions
- [ ] Toggle animates between states

### Cursors
- [ ] `pointer` on clickable elements
- [ ] `not-allowed` on disabled elements
- [ ] `text` on text inputs
- [ ] Default on non-interactive

---

## Category 4: Scroll Behavior

### Scrollable Areas
- [ ] Content taller than viewport scrolls
- [ ] Scroll indicator visible (scrollbar or visual cue)
- [ ] Nested scrolls work correctly (inner before outer)

### Fixed/Sticky
- [ ] Header stays visible during scroll (if intended)
- [ ] Sticky elements don't overlap content
- [ ] Footer stays at bottom

### UX
- [ ] Scroll position preserved on back navigation
- [ ] Anchor links account for sticky header offset
- [ ] Body scroll locked when modal is open

---

## Category 5: Attention to Detail

### Consistency
- [ ] Border radius same between similar elements
- [ ] Shadows same between similar elements
- [ ] Spacing between similar groups is identical
- [ ] Font sizes follow consistent scale

### Polish
- [ ] Disabled elements have opacity + not-allowed cursor
- [ ] Selected/active state visually distinct
- [ ] Icons aligned with text baseline
- [ ] Grid items consistent height
