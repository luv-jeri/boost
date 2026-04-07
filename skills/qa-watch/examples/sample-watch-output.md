# QA Watch — Sample Output

## Example: Mid-Build Watch During Dashboard Development

Shows what QA Watch output looks like during a build session where the developer is building a dashboard with sidebar, header, and card grid.

---

### Manual Mode — After Building Sidebar

**Invocation:** `/qa-watch`
**Scope:** `src/components/Sidebar/` (2 files changed)

```
🟡 Warning: Nav items have no hover state — src/components/Sidebar/NavItem.tsx:18
🟡 Warning: Cursor is default on clickable nav items, should be pointer — src/components/Sidebar/NavItem.tsx:18
✅ Overflow: Pass
✅ Missing States: Pass (disabled state on active nav item)
✅ Scroll Behavior: Pass (sidebar scrolls when nav items exceed height)
```

---

### Manual Mode — After Building Card Grid

**Invocation:** `/qa-watch`
**Scope:** `src/components/Dashboard/CardGrid.tsx` (1 file changed)

```
🟠 Critical: Card title overflows container with long text — src/components/Dashboard/CardGrid.tsx:34
🟡 Warning: No empty state when cards array is empty — src/components/Dashboard/CardGrid.tsx:28
🔵 Suggestion: Cards have no hover elevation change — src/components/Dashboard/CardGrid.tsx:32
✅ Scroll Behavior: N/A — not a scrollable container
✅ Attention to Detail: Pass
```

---

### Session Mode — Continuous Checking

**Invocation:** `/qa-watch --session`

```
✅ QA Watch session active. I'll check for issues after each build step.
```

*...developer builds Header component...*

```
✅ QA Watch: 4/5 checks pass, 1 N/A (scroll — header is fixed, not scrollable)
```

*...developer builds Stats Row component...*

```
🟡 Warning: Number values have no loading skeleton — src/components/Dashboard/StatsRow.tsx:22
🔵 Suggestion: Large numbers should use locale formatting — src/components/Dashboard/StatsRow.tsx:30
✅ Overflow: Pass
✅ Scroll: N/A
✅ Detail: Pass
```

*...developer builds Data Table component...*

```
🔴 Blocker: Table has no horizontal scroll on mobile — src/components/Dashboard/DataTable.tsx:15
🟡 Warning: No empty state for zero rows — src/components/Dashboard/DataTable.tsx:42
🟡 Warning: Sort column headers have no hover/focus indication — src/components/Dashboard/DataTable.tsx:25
✅ Detail: Pass
```

*...developer says `/qa-watch stop`...*

```
✅ QA Watch session ended.
   Session summary: 1 blocker, 3 warnings, 2 suggestions across 4 components.
   Run /qa-shield for a full 9-category post-build scan.
```
