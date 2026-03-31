# Pixel — Token Extraction & Matching

## Purpose

Map every design value to an existing project token. Never hardcode values that should be tokens. Flag gaps in the design system.

## Step 1: Discover Existing Tokens

Before extracting anything from the Figma, understand what the project already has.

**Read these files (in order, stop when found):**

### Tailwind Projects
- `tailwind.config.js` / `tailwind.config.ts` — custom theme extensions
- Check for `theme.extend.colors`, `theme.extend.spacing`, `theme.extend.fontSize`, etc.
- Check for CSS variable references in the config (`var(--color-primary)`)

### CSS Variable Projects
- Search for `:root` blocks in CSS files
- Check for theme files (`theme.css`, `variables.css`, `tokens.css`, `globals.css`)
- Check for dark mode variable overrides (`[data-theme="dark"]`, `.dark`, `@media (prefers-color-scheme: dark)`)

### Component Library Projects
- shadcn: `components.json` + `globals.css` (uses CSS variables)
- MUI: theme configuration file
- Radix: theme tokens
- Chakra: theme file

### Additional Sources
- `boost-patterns.md` — may document design conventions
- Design system docs if present in the repo

**Budget:** Stay under 500 lines total for token discovery. Scan files, don't read them end-to-end.

## Step 2: Extract Design Values

From the Figma input (MCP, screenshot, or dev export), extract:

**Colors:**
- Background colors (page, card, section)
- Text colors (headings, body, muted, links)
- Border colors
- Accent/brand colors
- State colors (success, warning, error, info)
- Opacity values

**Typography:**
- Font family
- Font sizes (in px)
- Font weights (numeric: 400, 500, 600, 700)
- Line heights
- Letter spacing
- Text transform (uppercase, capitalize)

**Spacing:**
- Padding (element internal spacing)
- Margin (element external spacing)
- Gap (flex/grid gap)
- Section spacing

**Borders & Effects:**
- Border width, style, color
- Border radius
- Box shadows (offset, blur, spread, color)
- Backdrop blur / filters

**Layout:**
- Container max-widths
- Column widths (fixed vs flexible)
- Breakpoint values

## Step 3: Match or Flag

For each extracted value, find the project token:

### Exact Match
The value maps directly to an existing token.
```
Figma: #3B82F6 → Token: text-blue-500 (Tailwind) or var(--color-primary) (CSS)
Status: MATCH
```

### Close Match (within threshold)
The value is close but not exact. Flag for confirmation.
```
Figma: #3B83F7 → Closest: text-blue-500 (#3B82F6), delta: 1
Status: CLOSE — confirm with designer
```

**Color threshold:** Delta E (CIE76) < 3 is "close." Above 3 is "no match."
**Spacing threshold:** Within 2px is "close" (e.g., Figma says 15px, closest token is 16px/`p-4`).
**Typography threshold:** Exact match only for font sizes and weights. No rounding.

### No Match
No existing token maps to this value.
```
Figma: #FF6B35 → No match in project tokens
Status: NEW TOKEN NEEDED
```

Flag in the Design Map with a recommendation:
- Suggest a token name following the project's naming convention
- Note where to add it (tailwind config, CSS variables file, theme file)

## Stack-Aware Output

Write token mappings in the project's actual syntax:

**Tailwind:**
```
bg-primary text-sm p-4 rounded-lg shadow-md gap-4
```

**CSS Variables:**
```
var(--color-primary) var(--font-sm) var(--spacing-4) var(--radius-lg)
```

**Plain CSS:**
```
#3B82F6 (document raw value, recommend adding as variable)
14px (document raw value, recommend adding to scale)
```

**Component Library (shadcn example):**
```
bg-primary text-muted-foreground — uses CSS variables under the hood
```

## Common Pitfalls

- **Don't assume Tailwind defaults.** A project may have overridden `blue-500` to a different value. Always read the config.
- **Don't match by name, match by value.** A project's `--color-primary` may not be blue.
- **Don't forget dark mode.** If the project has dark mode tokens, note the dark mode mapping too.
- **Don't invent token names.** Follow the existing naming convention exactly. If the project uses `--color-brand-primary`, don't suggest `--brand-color`.
