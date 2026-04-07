# QA Shield — Report Format

Supplementary template for the structured QA report. The core reporting flow is in SKILL.md.

---

## Summary Table

Always present the summary first:

| Category | Status | Findings |
|---|---|---|
| Figma Fidelity | ✅ Pass / ⚠ N warnings / 🔴 N blockers / N/A — [reason] | Brief description |
| Data/API Mismatch | ... | ... |
| Edge Cases | ... | ... |
| User Flow Gaps | ... | ... |
| Micro-interactions | ... | ... |
| Logging/Observability | ... | ... |
| Overflow | ... | ... |
| Scroll Behavior | ... | ... |
| Attention to Detail | ... | ... |

**Total: N blockers, N criticals, N warnings, N suggestions**

---

## Finding Detail Format

For each finding, provide:

### [Severity Icon] [Category] — [Brief Title]

**Severity:** Blocker / Critical / Warning / Suggestion
**Location:** `src/components/Card.tsx:24` or DOM element `div.card-container`
**Description:** Clear explanation of what's wrong
**Expected:** What should happen
**Actual:** What actually happens
**Suggested fix:**
\`\`\`
// Show the exact code change or CSS change needed
\`\`\`

---

## Severity Icons

- 🔴 Blocker
- 🟠 Critical
- 🟡 Warning
- 🔵 Suggestion
- ✅ Pass
- ⬚ N/A

---

## Fix Offer Format

After the report, offer fixes grouped by effort:

**Auto-fixable (I can apply these now):**
1. [Finding] — [what the fix is]
2. ...

**Manual fix needed (requires design decision):**
1. [Finding] — [options to consider]
2. ...

**Deferred (suggestions for later):**
1. [Finding] — [why it can wait]
2. ...

"Want me to apply the auto-fixable items? I'll show each change before applying."
