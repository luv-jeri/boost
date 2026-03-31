# Pixel — Input Detection

## Purpose

Detect what design input is available and extract maximum information from it. Support all input methods without requiring a specific one.

## Detection Priority

```
1. Check prompt for Figma URL (contains figma.com)
   → Figma MCP available? → Use MCP to query node tree, styles, layout
   → MCP not available? → Warn, offer to proceed with screenshot

2. Check context for image/screenshot
   → Visual analysis: extract layout, colors, typography, spacing

3. Check prompt for pasted specs (CSS values, spacing notation)
   → Parse as Dev Mode export

4. None found
   → Ask: "Share your design — Figma URL, screenshot, or paste the specs"
```

## Input Types

### Figma MCP

**Detection:** URL contains `figma.com/design/` or `figma.com/file/`
**Extraction:** Query MCP for:
- Node tree (component hierarchy)
- Fill colors, stroke colors
- Typography properties (font, size, weight, line-height, letter-spacing)
- Layout properties (auto-layout direction, spacing, padding)
- Corner radius, effects (shadows, blur)
- Component instances and variants
- Frame dimensions

**Strengths:** Precise numeric values, complete style information
**Weaknesses:** May not capture visual nuance, complex overlays, or exact rendering

### Screenshot

**Detection:** Image file in context (PNG, JPG, WebP) or pasted image
**Extraction:** Visual analysis for:
- Layout hierarchy (what contains what)
- Approximate colors (may need confirmation)
- Typography (identify font family, estimate sizes)
- Spacing rhythm (identify consistent gaps)
- Component boundaries
- Interactive element identification (buttons, inputs, links)

**Strengths:** Shows exact visual output, catches rendering details
**Weaknesses:** Values are approximate, no direct token access

### Dev Mode Export

**Detection:** Pasted text containing CSS properties, spacing values, or structured design specs
**Extraction:** Parse directly:
- CSS property values
- Spacing and dimension values
- Color values (hex, rgb, hsl)
- Font specifications

**Strengths:** Precise values from the designer's perspective
**Weaknesses:** Partial — usually covers specific elements, not full layout

### Multiple Inputs

When multiple inputs are available, cross-reference:
- Use Figma MCP for precise token values (colors, spacing, typography)
- Use screenshot for visual layout truth and overall composition
- Use dev mode exports to confirm specific values

Note any discrepancies in Open Questions: "Figma MCP reports `#3B82F6` but screenshot appears closer to `#2563EB` — confirm with designer"

## Figma MCP Failure

If the MCP is configured but fails:

**Auth error:** "Figma MCP auth failed. Check your Figma access token. Want to fix this first or proceed with screenshot analysis?"

**Rate limit:** "Figma MCP rate limited. I can proceed with screenshot analysis (token extraction will be approximate) or wait and retry."

**Node not found:** "Couldn't find that frame in Figma. Check the URL points to the correct frame. Want to try a different URL?"

**Never** silently fall back. Always inform the dev and let them decide.

## Output

Input detection results feed into the Design Map. The detection step should produce:
1. Which input methods were used
2. Confidence level for extracted values (exact from MCP, approximate from screenshot)
3. Any extraction failures or discrepancies to flag
