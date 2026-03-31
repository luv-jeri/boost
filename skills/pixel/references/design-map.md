# Pixel — Design Map Structure

## Purpose

The Design Map is the contract between analysis and implementation. Every design decision is captured here before any code is written. It is a living document updated during Q&A.

## The 7 Sections

### 1. Layout Structure

Document the page-level layout hierarchy:

```
Page
├── Header (full-width, sticky)
│   ├── Logo (left)
│   ├── Nav links (center)
│   └── User menu (right)
├── Main (flex row)
│   ├── Sidebar (fixed 280px)
│   └── Content area (flex-1)
│       ├── Page title section
│       ├── Filter bar
│       └── Card grid (3 columns)
└── Footer (full-width)
```

For each container, note:
- Display type (flex, grid, block)
- Direction, alignment, justification
- Gap/spacing between children
- Whether it's sticky, fixed, or in flow
- Max-width constraints

### 2. Component Inventory

List every distinct component with:

| Component | Variants | States | Notes |
|---|---|---|---|
| Button | primary, secondary, ghost | default, hover, active, disabled, loading | Uses existing Button component? |
| Card | standard, featured | default, hover, selected, empty | Shadow on hover |
| Nav Link | — | default, hover, active (current page) | Active has underline |

**States to always check for:**
- Default
- Hover
- Active / pressed
- Focus (keyboard navigation)
- Disabled
- Loading
- Error
- Empty / no data

If a state isn't visible in the Figma, add it to Open Questions. Do NOT skip it.

### 3. Design Tokens

Map every visual value to a token:

**Colors:**
| Usage | Figma Value | Project Token | Status |
|---|---|---|---|
| Page background | `#F9FAFB` | `bg-gray-50` | Exact match |
| Card border | `#E5E7EB` | `border-gray-200` | Exact match |
| Accent | `#6366F1` | — | New token needed |

**Typography:**
| Usage | Font | Size | Weight | Line-height | Token |
|---|---|---|---|---|---|
| Page title | Inter | 24px | 600 | 32px | `text-2xl font-semibold` |
| Body text | Inter | 14px | 400 | 20px | `text-sm` |

**Spacing:**
| Usage | Value | Token |
|---|---|---|
| Card padding | 24px | `p-6` |
| Grid gap | 16px | `gap-4` |
| Section margin | 32px | `mb-8` |

**Other:** border-radius, shadows, borders — same format.

### 4. Responsive Behavior

| Breakpoint | Layout Changes |
|---|---|
| Desktop (>1024px) | 3-column card grid, sidebar visible |
| Tablet (768-1024px) | 2-column grid, sidebar collapsible |
| Mobile (<768px) | 1-column grid, sidebar hidden, hamburger menu |

If the Figma only shows one breakpoint:
- Note: "Only desktop design provided"
- Add to Open Questions: "Should I infer responsive behavior or build desktop-only?"
- NEVER silently build responsive or silently skip it

### 5. Micro-interactions

| Element | Trigger | Animation | Duration | Easing |
|---|---|---|---|---|
| Card | hover | Scale 1.02, shadow increase | 200ms | ease-out |
| Button | click | Scale 0.98 | 100ms | ease-in-out |
| Sidebar | toggle | Slide left/right | 300ms | ease-in-out |
| Page load | mount | Fade in, slide up 8px | 400ms | ease-out |

If Figma doesn't specify animations:
- Add to Open Questions: "No animations specified — should I add standard micro-interactions?"
- Common defaults to suggest: hover states (scale/shadow), transitions (opacity/transform), loading skeletons

### 6. Assets

| Asset | Type | Source | Available? |
|---|---|---|---|
| Logo | SVG | Figma export needed | No — needs export |
| User avatar | Image | Placeholder / dynamic | Use existing avatar component |
| Search icon | Icon | Lucide `Search` | Yes — in icon library |
| Chart | Component | Recharts / custom | Needs implementation |

Check the project's existing icon library before flagging icons as "needs export."

### 7. Open Questions

Capture anything ambiguous, missing, or inconsistent:

- "Card hover state not shown in Figma — should cards have hover effects?"
- "Mobile layout not provided — infer responsive or desktop-only?"
- "The accent color `#6366F1` doesn't match any existing token — add new token or use closest match `--color-indigo-500`?"
- "Empty state for the card grid not shown — what should it display?"
- "Is the sidebar collapsible on desktop or only hidden on mobile?"

**Every question must be answered or marked "decide later" before Phase 2 begins.**

## Design Map Format

Present the map as a single structured artifact with clear section headers. Use tables for tokens, inventory, and responsive behavior. Use tree diagrams for layout structure.

The map should be self-contained — someone reading only the map should understand exactly what needs to be built without looking at the Figma.
