# QA Shield — Sample Report

## Example: E-commerce Product Card

A complete QA Shield report for a product card component with add-to-cart functionality, showing all 9 categories scoped and scanned.

---

### Scope

**Target:** `src/components/ProductCard/` (3 files changed in current branch)
**Inputs detected:** Code files ✅ | Running preview ✅ | Figma reference ❌

### Scoping Results

| # | Category | Status | Reason |
|---|---|---|---|
| 1 | Figma Fidelity | N/A | No Figma reference provided |
| 2 | Data/API Mismatch | Active | Component fetches product data from API |
| 3 | Edge Cases | Active | Interactive component with data fetching |
| 4 | User Flow Gaps | N/A | Single component, no multi-step flow |
| 5 | Micro-interactions | Active | Buttons, hover states, add-to-cart action |
| 6 | Logging/Observability | Active | User actions (add to cart) should be tracked |
| 7 | Overflow | Active | Product name and description are dynamic text |
| 8 | Scroll Behavior | N/A | Component is not a scrollable container |
| 9 | Attention to Detail | Active | Card styling, consistency, disabled states |

**6 categories active, 3 N/A**

---

### Report

| Category | Status | Findings |
|---|---|---|
| Figma Fidelity | ⬚ N/A | No Figma reference |
| Data/API Mismatch | 🟡 1 warning | Missing null check on `product.discount` |
| Edge Cases | 🔴 1 blocker | No error state when API fails |
| User Flow Gaps | ⬚ N/A | Single component |
| Micro-interactions | 🟡 1 warning | No hover state on card |
| Logging/Observability | 🔵 1 suggestion | No analytics event on add-to-cart |
| Overflow | 🟠 1 critical | Product name overflows at 2 lines |
| Scroll Behavior | ⬚ N/A | Not scrollable |
| Attention to Detail | 🟡 1 warning | Inconsistent border-radius |

**Total: 1 blocker, 1 critical, 3 warnings, 1 suggestion**

---

### Detailed Findings

#### 🔴 Edge Cases — No error state when API fails

**Severity:** Blocker
**Location:** `src/components/ProductCard/ProductCard.tsx:45`
**Description:** The `useProduct` hook fetches data but the component doesn't render anything when the request fails. User sees a blank card.
**Expected:** Error message with retry button
**Actual:** Blank white space where card should be
**Suggested fix:**
```tsx
if (error) {
  return (
    <div className="product-card product-card--error">
      <p>Failed to load product</p>
      <button onClick={refetch}>Retry</button>
    </div>
  );
}
```

#### 🟠 Overflow — Product name overflows at 2 lines

**Severity:** Critical
**Location:** `src/components/ProductCard/ProductCard.tsx:58` — `.product-card__name`
**Description:** Product names longer than ~40 characters overflow the card container. No truncation or line-clamping.
**Expected:** Text truncated with ellipsis after 2 lines
**Actual:** Text overflows and overlaps the price section
**Suggested fix:**
```css
.product-card__name {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

#### 🟡 Data/API Mismatch — Missing null check on discount

**Severity:** Warning
**Location:** `src/components/ProductCard/ProductCard.tsx:62`
**Description:** `product.discount` is optional in the API response but rendered without a null check. When no discount exists, "undefined%" is rendered.
**Expected:** Discount badge hidden when no discount
**Actual:** Shows "undefined%" badge
**Suggested fix:**
```tsx
{product.discount != null && (
  <span className="product-card__discount">-{product.discount}%</span>
)}
```

#### 🟡 Micro-interactions — No hover state on card

**Severity:** Warning
**Location:** `src/components/ProductCard/ProductCard.tsx:52` — `.product-card`
**Description:** Card is clickable (navigates to product detail) but has no hover indication. User doesn't know it's interactive.
**Expected:** Subtle shadow lift or border change on hover + `cursor: pointer`
**Suggested fix:**
```css
.product-card {
  cursor: pointer;
  transition: box-shadow 150ms ease-in-out;
}
.product-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}
```

#### 🟡 Attention to Detail — Inconsistent border-radius

**Severity:** Warning
**Location:** `src/components/ProductCard/ProductCard.tsx:52` vs `src/components/ProductCard/AddToCartButton.tsx:12`
**Description:** Card has `border-radius: 8px` but the add-to-cart button inside has `border-radius: 6px`. Other cards in the app use `8px` for both.
**Expected:** Consistent border-radius using design token
**Suggested fix:** Change button to use the same `rounded-lg` (8px) token as the card.

#### 🔵 Logging/Observability — No analytics event on add-to-cart

**Severity:** Suggestion
**Location:** `src/components/ProductCard/AddToCartButton.tsx:28`
**Description:** The add-to-cart click handler updates state but doesn't fire an analytics event. This is a key conversion action.
**Expected:** `track('add_to_cart', { productId, price })` on click
**Suggested fix:**
```tsx
const handleAddToCart = () => {
  addToCart(product);
  analytics.track('add_to_cart', {
    productId: product.id,
    price: product.price,
  });
};
```

---

### Fix Offer

**Auto-fixable (I can apply these now):**
1. Error state — add error handling JSX (blocker)
2. Overflow — add line-clamp CSS (critical)
3. Null check — add conditional rendering for discount (warning)

**Manual fix needed:**
4. Hover state — choose shadow values or check design system (warning)
5. Border radius — confirm 8px is the correct token (warning)

**Deferred:**
6. Analytics — needs team's analytics utility import and event naming (suggestion)

"Want me to apply items 1-3? I'll show each change before applying."
