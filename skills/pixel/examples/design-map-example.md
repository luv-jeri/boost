# Pixel — Design Map Example

## Example: Dashboard Settings Page

This is a complete design map for a settings page, showing what the output of Phase 1 looks like.

---

### 1. Layout Structure

```
Settings Page
├── Page Header (full-width, border-bottom)
│   ├── Breadcrumb (left)
│   └── Save button (right)
├── Main Content (max-w-4xl, centered)
│   ├── Profile Section
│   │   ├── Section heading
│   │   ├── Avatar upload
│   │   └── Form fields (2-column grid)
│   ├── Divider
│   ├── Notifications Section
│   │   ├── Section heading
│   │   └── Toggle list (single column)
│   ├── Divider
│   └── Danger Zone Section
│       ├── Section heading (red)
│       └── Delete account button
└── (No footer on this page)
```

- Page Header: `flex justify-between items-center`, sticky top, `px-8 py-4`
- Main Content: `max-w-4xl mx-auto`, `py-8 px-4`
- Form grid: `grid grid-cols-2 gap-6` on desktop, `grid-cols-1` on mobile
- Sections separated by `border-b border-gray-200`, `py-8`

### 2. Component Inventory

| Component | Variants | States | Notes |
|---|---|---|---|
| Breadcrumb | — | default | Separator: `/` |
| Button | primary, danger, ghost | default, hover, active, disabled, loading | Primary: save. Danger: delete account |
| Avatar Upload | — | default, hover (overlay), uploading, error | Circular, 96px, with camera icon overlay on hover |
| Text Input | — | default, focus, error, disabled | Label above, error message below |
| Toggle | — | on, off, disabled | With label right-aligned |
| Section Heading | — | default | `text-lg font-semibold`, optional description below |
| Divider | — | — | `border-gray-200`, full width of content area |

### 3. Design Tokens

**Colors:**

| Usage | Figma Value | Project Token | Status |
|---|---|---|---|
| Page background | `#FFFFFF` | `bg-white` | Match |
| Section text | `#111827` | `text-gray-900` | Match |
| Muted text | `#6B7280` | `text-gray-500` | Match |
| Primary button bg | `#4F46E5` | `bg-indigo-600` | Match |
| Primary button hover | `#4338CA` | `hover:bg-indigo-700` | Match |
| Danger text | `#DC2626` | `text-red-600` | Match |
| Danger button bg | `#DC2626` | `bg-red-600` | Match |
| Input border | `#D1D5DB` | `border-gray-300` | Match |
| Input focus ring | `#4F46E5` | `ring-indigo-600` | Match |
| Toggle active | `#4F46E5` | `bg-indigo-600` | Match |
| Toggle inactive | `#D1D5DB` | `bg-gray-300` | Match |

**Typography:**

| Usage | Font | Size | Weight | Line-height | Token |
|---|---|---|---|---|---|
| Page title (breadcrumb) | Inter | 14px | 500 | 20px | `text-sm font-medium` |
| Section heading | Inter | 18px | 600 | 28px | `text-lg font-semibold` |
| Section description | Inter | 14px | 400 | 20px | `text-sm text-gray-500` |
| Form label | Inter | 14px | 500 | 20px | `text-sm font-medium` |
| Form input | Inter | 14px | 400 | 20px | `text-sm` |
| Button text | Inter | 14px | 500 | 20px | `text-sm font-medium` |

**Spacing:**

| Usage | Value | Token |
|---|---|---|
| Page header padding | 32px horizontal, 16px vertical | `px-8 py-4` |
| Content max-width | 896px | `max-w-4xl` |
| Content padding | 32px vertical, 16px horizontal | `py-8 px-4` |
| Section padding | 32px vertical | `py-8` |
| Form grid gap | 24px | `gap-6` |
| Input label spacing | 8px below label | `mb-2` |

### 4. Responsive Behavior

| Breakpoint | Layout Changes |
|---|---|
| Desktop (>768px) | Form fields in 2-column grid, avatar beside form |
| Mobile (<768px) | Form fields stack to 1 column, avatar above form, full-width buttons |

Source: Only desktop shown in Figma. Mobile behavior **confirmed with designer**: stack to single column.

### 5. Micro-interactions

| Element | Trigger | Animation | Duration | Easing |
|---|---|---|---|---|
| Avatar overlay | hover | Fade in camera icon + dark overlay | 200ms | ease-out |
| Button (all) | hover | Background color transition | 150ms | ease-in-out |
| Button (primary) | click | Scale 0.98 | 100ms | ease-in-out |
| Toggle | click | Slide knob + color transition | 200ms | ease-in-out |
| Input | focus | Ring fade in | 150ms | ease-out |
| Save button | loading | Spinner replaces text, width maintained | — | — |

### 6. Assets

| Asset | Type | Source | Available? |
|---|---|---|---|
| Camera icon (avatar overlay) | Icon | Lucide `Camera` | Yes |
| Default avatar | Image | UI Avatars placeholder | Generate from initials |
| Breadcrumb separator | Text | `/` character | N/A |

### 7. Open Questions

*All resolved during Q&A:*

- ~~"Mobile layout not provided — infer responsive?"~~ **Resolved: Yes, stack to single column**
- ~~"Avatar upload — drag and drop or click only?"~~ **Resolved: Click only for v1**
- ~~"Delete account — confirmation modal?"~~ **Resolved: Yes, with text confirmation input**
- ~~"Form validation — inline or on submit?"~~ **Resolved: Inline, on blur**

---

*This map was the complete input for Phase 2. Build order was: Page Header → Profile Section (avatar, form fields) → Notifications Section (toggles) → Danger Zone → Responsive adjustments → Final audit.*
