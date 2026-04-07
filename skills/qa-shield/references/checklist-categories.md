# QA Shield — Detailed Category Checklists

Supplementary detail for each of the 9 check categories. The core process is in SKILL.md — this file provides exhaustive per-category checklist items.

---

## Category 1: Figma Fidelity

**Requires:** Figma reference (URL or screenshot) + Preview MCP

### Spacing
- [ ] Padding matches Figma values (top, right, bottom, left)
- [ ] Margin matches Figma values
- [ ] Gap between child elements matches Figma auto-layout spacing
- [ ] Section spacing is consistent and matches Figma

### Colors
- [ ] Background colors match Figma fills
- [ ] Text colors match Figma text fills
- [ ] Border colors match Figma strokes
- [ ] Icon colors match Figma (fill or stroke depending on icon)
- [ ] All colors use project design tokens, not hardcoded hex

### Typography
- [ ] Font family matches Figma
- [ ] Font size matches Figma (exact px)
- [ ] Font weight matches Figma (exact weight number)
- [ ] Line height matches Figma
- [ ] Letter spacing matches Figma (if specified)
- [ ] Text alignment matches Figma

### Sizing
- [ ] Component width matches Figma (or is responsive equivalent)
- [ ] Component height matches Figma (or is content-driven)
- [ ] Image/icon dimensions match Figma
- [ ] Min/max constraints match Figma frame constraints

### Visual Effects
- [ ] Border radius matches Figma corner radius
- [ ] Box shadows match Figma drop shadows (offset, blur, spread, color)
- [ ] Opacity matches Figma layer opacity

---

## Category 2: Data/API Mismatch

**Requires:** Code analysis only

### Type Safety
- [ ] API response data has TypeScript interfaces/types defined
- [ ] Response types match what the UI actually renders (field names, nested paths)
- [ ] No `any` types on API response data
- [ ] Optional fields are marked optional in types

### Null/Empty Handling
- [ ] UI handles `null` values from API (no "null" rendered as text)
- [ ] UI handles `undefined` values (no crashes, no "undefined" text)
- [ ] UI handles empty arrays `[]` (shows empty state, not blank space)
- [ ] UI handles empty strings `""` (shows placeholder or dash)
- [ ] UI handles missing nested objects (e.g., `user.address?.city`)

### Loading States
- [ ] Loading indicator shown while API request is in-flight
- [ ] UI does not flash raw/stale data before loading completes
- [ ] Loading state covers the correct area (not just a spinner in the corner)

### Error Handling
- [ ] API errors are caught (try/catch, .catch(), error boundary)
- [ ] Error state is shown to the user (not just console.error)
- [ ] Error message is user-friendly (not raw error object or stack trace)
- [ ] Retry mechanism available where appropriate

### Field Correctness
- [ ] Field names in UI match API response field names exactly
- [ ] No stale field references (renamed fields in API but not UI)
- [ ] Date/time formats are parsed and displayed correctly
- [ ] Number formats are handled (currency, percentages, decimals)
- [ ] Enum values are mapped to display labels

---

## Category 3: Edge Cases

**Requires:** Code analysis + Preview

### Error States
- [ ] Network failure — what does the user see?
- [ ] Server error (500) — error message displayed?
- [ ] Validation error (400) — specific field errors shown?
- [ ] Auth error (401/403) — redirect or error message?
- [ ] Not found (404) — appropriate UI?

### Loading States
- [ ] Initial load — skeleton or spinner?
- [ ] Refresh/reload — does old data stay visible?
- [ ] Pagination load — loading indicator for next page?
- [ ] Action in progress — button disabled + spinner?

### Empty States
- [ ] No data — helpful message with action (not blank space)
- [ ] No search results — clear message, suggest clearing filters
- [ ] No permissions — explain why empty, suggest action
- [ ] First-time user — onboarding or setup prompt

### Boundary Inputs
- [ ] Zero — handled correctly (not divide-by-zero, not hidden)
- [ ] Negative numbers — rejected or handled
- [ ] Very large numbers — display doesn't break
- [ ] Very long text — truncation or wrapping
- [ ] Special characters — rendered correctly, no XSS
- [ ] Unicode/emoji — displayed correctly

---

## Category 4: User Flow Gaps

**Requires:** Code + Preview

### Navigation
- [ ] Every page is reachable from the navigation
- [ ] Back button/navigation works from every page
- [ ] Breadcrumbs (if present) are accurate and clickable
- [ ] Deep links work (direct URL access to any page)

### Dead Ends
- [ ] Error pages have a way back (link to home, retry button)
- [ ] Empty states have a call-to-action
- [ ] After completing a flow (form submit, etc.) — user knows what happened and where to go

### Destructive Actions
- [ ] Delete/remove actions have confirmation dialogs
- [ ] Confirmation text clearly states what will be deleted
- [ ] Cancel option is available and works
- [ ] Undo is available where appropriate

### Multi-step Flows
- [ ] Progress indication (step 1 of 3)
- [ ] Can go back to previous steps
- [ ] Data is preserved when going back
- [ ] Can cancel the entire flow

---

## Category 5: Micro-interactions

**Requires:** Preview inspect

### Hover States
- [ ] Buttons change appearance on hover
- [ ] Links have hover indication (underline, color change)
- [ ] Cards/rows have hover indication if clickable
- [ ] Hover state is visually distinct from active/pressed

### Focus States
- [ ] Visible focus ring on all interactive elements
- [ ] Focus ring uses consistent styling (not browser default)
- [ ] Focus order follows logical reading order
- [ ] Focus is trapped in modals/dialogs

### Transitions
- [ ] State changes are animated (not instant jumps)
- [ ] Duration is appropriate (150-300ms for most, up to 500ms for layout changes)
- [ ] Easing is smooth (not linear)
- [ ] No janky/stuttering transitions

### Feedback
- [ ] Button click has visual feedback (pressed state, ripple, scale)
- [ ] Form submission shows progress (loading state on button)
- [ ] Success/error toasts or messages after actions
- [ ] Toggle/switch animates between states

### Cursors
- [ ] `pointer` on clickable elements (buttons, links, clickable cards)
- [ ] `not-allowed` on disabled elements
- [ ] `text` on text inputs
- [ ] `grab`/`grabbing` on draggable elements
- [ ] Default cursor on non-interactive elements

---

## Category 6: Logging/Observability

**Requires:** Code analysis only

### Error Logging
- [ ] Caught errors are logged (not silently swallowed)
- [ ] Error logs include context (what was being done, relevant IDs)
- [ ] API errors log the endpoint, status code, and relevant request data
- [ ] Client-side errors are captured (error boundary, window.onerror)

### Analytics Events
- [ ] Key user actions are tracked (button clicks, form submissions, navigation)
- [ ] Events include relevant context (page, component, action type)
- [ ] Event names follow a consistent naming convention

### Performance
- [ ] Slow operations are measured (API calls, heavy computations)
- [ ] Performance metrics are reported to monitoring

### Console Hygiene
- [ ] No leftover `console.log` statements in production code
- [ ] No `console.warn` or `console.error` for expected behavior
- [ ] Debug logging is gated behind a flag or environment check

---

## Category 7: Overflow

**Requires:** Preview + code

### Text Overflow
- [ ] Long single words don't break containers (`overflow-wrap: break-word`)
- [ ] Long text is truncated with ellipsis OR wraps correctly
- [ ] Truncated text has a tooltip or expand mechanism for full content
- [ ] Text doesn't overlap adjacent elements

### Container Overflow
- [ ] Containers with dynamic content have `overflow: auto` or `hidden`
- [ ] No horizontal scrollbar on the page (unless intentional)
- [ ] Modal/dialog content scrolls if it exceeds viewport height
- [ ] Dropdown menus don't overflow off-screen

### Dynamic Content
- [ ] Test with 2x expected content length
- [ ] Test with 0 content (empty state)
- [ ] Test with very long single values (URL, email, name)
- [ ] Lists with 100+ items don't break layout

### Images
- [ ] Images respect container bounds (`max-width: 100%`, `object-fit`)
- [ ] Broken image links show fallback (not broken icon)
- [ ] Image aspect ratios are preserved

---

## Category 8: Scroll Behavior

**Requires:** Preview inspect

### Scrollable Areas
- [ ] Content taller than viewport is scrollable
- [ ] Scroll containers have visible scrollbar or scroll indicator
- [ ] Nested scroll areas work correctly (inner scrolls before outer)
- [ ] Horizontal scroll is intentional (not accidental overflow)

### Fixed/Sticky Elements
- [ ] Header/navigation stays visible during scroll (if intended)
- [ ] Sticky elements don't overlap content
- [ ] Sticky elements work on mobile
- [ ] Footer stays at bottom (sticky footer or pushed by content)

### Scroll UX
- [ ] Scroll-to-top button on long pages
- [ ] Scroll position preserved when navigating back
- [ ] Anchor links scroll to correct position (with offset for sticky header)
- [ ] Infinite scroll has loading indicator and end-of-list marker

### Mobile Scroll
- [ ] Touch scrolling is smooth (no jank)
- [ ] Pull-to-refresh works (if applicable)
- [ ] Scroll doesn't get "stuck" behind modals/overlays
- [ ] Body scroll is locked when modal is open

---

## Category 9: Attention to Detail

**Requires:** Preview + code

### Visual Consistency
- [ ] Border radius is consistent between similar elements
- [ ] Shadows are consistent between similar elements
- [ ] Spacing between similar groups is identical
- [ ] Font sizes follow a consistent scale (no arbitrary sizes)

### Interactive Polish
- [ ] Disabled elements have reduced opacity + `cursor: not-allowed`
- [ ] Selected/active state is visually distinct
- [ ] Active tab/nav item is highlighted
- [ ] Badges/counts update correctly

### Layout Polish
- [ ] Elements are vertically centered (not 1-2px off)
- [ ] Icons are aligned with text baseline
- [ ] Grid items are consistent height (no ragged rows)
- [ ] Spacing between icon and label is consistent

### Z-index
- [ ] Modals appear above all other content
- [ ] Dropdowns appear above sibling elements
- [ ] Tooltips appear above their trigger element
- [ ] No unexpected overlapping elements
