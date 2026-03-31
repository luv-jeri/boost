# Boost Task Templates

## How to Use

1. Match the detected task category to the corresponding template below
2. Fill in each field using the raw prompt and discovered context
3. If a field cannot be filled, write "Unknown — investigate" — never omit, never guess
4. Check the before/after example to calibrate your output quality

---

## Refactor Template

**Example transformation:**

Raw: `clean up the utils folder it's a mess /boost`

Boosted:
```
## Task: Reorganize utils folder for clarity and maintainability
## Type: Refactor
## Context:
  - Project: Next.js 14 with TypeScript
  - Tech stack: React, Prisma, tailwindcss
  - Related files: src/utils/ (12 files, mixed concerns)
  - Team conventions: Tests go in __tests__/ next to source
## Current State: Utils folder contains mixed concerns — formatting helpers, validation functions, API helpers, and constants in a flat structure with no organization
## Target State: Organized by domain (formatting/, validation/, api/, constants/) with clear module boundaries and index exports
## Boundaries: All existing imports must continue to work; no behavior changes
## Constraints:
  - Preserve all existing behavior and exports
  - Update all import paths across the project
  - Maintain test coverage
## Success Criteria:
  - Each utils file has a single clear responsibility
  - All existing tests pass without modification
  - No behavior changes in consuming code
```

**Field priorities:** Task (required) > Context (required) > Current State (important) > Target State (important) > Boundaries (important) > Constraints (important) > Success Criteria (nice-to-have)

**Anti-pattern:** "Refactor the utils" with no Current State or Target State. Without knowing what's wrong and what "better" looks like, the agent will make arbitrary changes.

**Template:**

```
## Task: [One-line summary: what is being refactored and why]
## Type: Refactor
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Recent changes: [relevant commits, if any]
  - Related files: [file paths for the module being refactored]
  - Team conventions: [relevant entries from boost-patterns.md]
## Current State: [What exists now and what is wrong with it]
## Target State: [What it should look like after refactoring]
## Boundaries: [What must NOT change — APIs, behavior, contracts]
## Constraints:
  - Preserve all existing behavior and API contracts
  - Maintain or improve test coverage
  - [Any project-specific constraints from conventions]
## Success Criteria:
  - Code is reorganized per target state
  - All existing tests pass without modification
  - No behavior changes
```

---

## Debug Template

**Example transformation:**

Raw: `fix the login page it keeps crashing when I click submit /boost`

Boosted:
```
## Task: Fix crash on login form submission
## Type: Debug
## Context:
  - Project: Next.js 14 app with TypeScript
  - Tech stack: React, Prisma, PostgreSQL
  - Recent changes: auth middleware updated 2 days ago (commit abc123)
  - Related files: src/app/login/page.tsx, src/lib/auth.ts
## Symptoms: Login page crashes on submit button click
## Expected Behavior: Form submits successfully and authenticates user
## Actual Behavior: Crash/error occurs on form submit
## Investigation Starting Points:
  - Form submit handler in login page component
  - Auth middleware (recently changed — likely culprit)
  - Network requests triggered on form submit
## Constraints:
  - Do not modify existing auth API contracts
  - Maintain existing test coverage
## Success Criteria:
  - Login form submits without crashing
  - All existing auth tests still pass
  - Root cause is identified and fixed (not just symptoms)
```

**Field priorities:** Task (required) > Context (required) > Symptoms (required) > Investigation Starting Points (important) > Expected/Actual (important) > Constraints (nice-to-have)

**Anti-pattern:** "Fix the crash" with no Symptoms or Investigation Starting Points. The agent will read random files looking for a crash instead of starting from the user's description.

**Template:**

```
## Task: [One-line summary of the bug]
## Type: Debug
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Recent changes: [relevant commits — especially recent ones that may have caused the bug]
  - Related files: [file paths related to the bug area]
  - Team conventions: [relevant entries from boost-patterns.md]
## Symptoms: [What the user described as broken — exact words]
## Expected Behavior: [What should happen]
## Actual Behavior: [What happens instead]
## Investigation Starting Points:
  - [Files and functions most likely involved]
  - [Recently changed files in the area]
  - [Related error handlers or middleware]
## Constraints:
  - Do not modify unrelated code
  - Maintain existing test coverage
  - [Any project-specific constraints]
## Success Criteria:
  - The described bug no longer occurs
  - All existing tests pass
  - Root cause is identified and fixed (not just symptoms)
```

---

## Feature Template

**Example transformation:**

Raw: `add dark mode to settings page /boost`

Boosted:
```
## Task: Add dark mode toggle to the settings page
## Type: Feature
## Context:
  - Project: Next.js 14 with TypeScript
  - Tech stack: React, tailwindcss, next-themes
  - Related files: src/app/settings/page.tsx, src/styles/globals.css, tailwind.config.ts
  - Team conventions: Use CSS variables for theming
## Requirements: Toggle switch in settings that switches between light and dark themes
## Acceptance Criteria:
  - Toggle persists across sessions (localStorage or user preferences)
  - All existing components respect the theme
  - No flash of wrong theme on page load
## Edge Cases:
  - System preference changes while app is open
  - Theme persistence across devices (if user auth exists)
  - Components with hardcoded colors that bypass theme
## Integration Points: Global layout component, CSS variables or theme provider, settings state
## Constraints:
  - Follow existing styling patterns (tailwindcss)
  - Use existing state management approach
## Success Criteria:
  - Dark mode toggle works from settings
  - Theme persists across page refreshes
  - No visual regressions in existing components
```

**Field priorities:** Task (required) > Context (required) > Requirements (required) > Acceptance Criteria (important) > Edge Cases (important) > Integration Points (nice-to-have)

**Anti-pattern:** "Add dark mode" with no Acceptance Criteria or Edge Cases. The agent builds a toggle that doesn't persist and misses theme flash on load.

**Template:**

```
## Task: [One-line summary of the feature to build]
## Type: Feature
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Recent changes: [relevant commits, if any]
  - Related files: [existing files this feature integrates with]
  - Team conventions: [relevant entries from boost-patterns.md]
## Requirements: [What the feature should do — extracted from the raw prompt]
## Acceptance Criteria:
  - [Concrete, testable criteria derived from the prompt]
## Edge Cases:
  - [What could go wrong or behave unexpectedly]
## Integration Points: [Existing code this feature touches or extends]
## Constraints:
  - Follow existing project patterns and conventions
  - [Any project-specific constraints]
## Success Criteria:
  - Feature works as described
  - Tests cover happy path and edge cases
  - Integrates cleanly with existing code
```

---

## Review Template

**Example transformation:**

Raw: `look at the payment module for security issues /boost`

Boosted:
```
## Task: Security review of the payment module
## Type: Review
## Context:
  - Project: Express.js API with TypeScript
  - Tech stack: Stripe SDK, Prisma, PostgreSQL
  - Related files: src/payments/, src/middleware/auth.ts
  - Team conventions: All API inputs validated with zod
## Scope: All files in src/payments/ and related middleware
## Focus Areas:
  - Security: injection vulnerabilities, auth bypass, data exposure, PCI compliance
  - Input validation: are all payment amounts, currency codes, and card tokens validated?
  - Error handling: are payment errors handled without leaking sensitive data?
  - Logging: are card numbers or tokens accidentally logged?
## Output Format:
  - List findings by severity: critical, warning, suggestion
  - Include file path and line number for each finding
  - Suggest fixes for critical and warning items
## Constraints:
  - Review only the payment module and directly related middleware
  - Focus on security — not code style or performance
## Success Criteria:
  - All critical security issues identified and flagged
  - Actionable fix suggestions provided for each finding
```

**Field priorities:** Task (required) > Scope (required) > Focus Areas (required) > Output Format (important) > Constraints (nice-to-have)

**Anti-pattern:** "Review the payment code" with no Focus Areas. The agent reviews everything superficially instead of deeply checking security.

**Template:**

```
## Task: [One-line summary of what to review]
## Type: Review
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Related files: [file paths to review]
  - Team conventions: [relevant entries from boost-patterns.md]
## Scope: [Which files, modules, or changes to review]
## Focus Areas:
  - Security (injection, auth, data exposure)
  - Performance (N+1 queries, unnecessary re-renders, memory leaks)
  - Correctness (logic errors, edge cases, error handling)
  - Code quality (readability, naming, duplication)
## Output Format:
  - List findings by severity: critical, warning, suggestion
  - Include file path and line number for each finding
  - Suggest fixes for critical and warning items
## Constraints:
  - Review only the specified scope
  - [Any project-specific review standards]
## Success Criteria:
  - All critical issues identified and flagged
  - Actionable suggestions provided
```

---

## Test Template

**Example transformation:**

Raw: `write tests for the checkout flow /boost`

Boosted:
```
## Task: Write tests for the checkout flow
## Type: Test
## Context:
  - Project: Next.js 14 with TypeScript
  - Tech stack: vitest, testing-library/react, MSW for API mocking
  - Related files: src/app/checkout/page.tsx, src/lib/cart.ts, src/api/orders.ts
  - Team conventions: Tests in __tests__/ next to source, use MSW for API mocks
## Test Targets: Checkout page component, cart calculations, order API integration
## Test Types:
  - Unit: cart total calculations, discount application, tax computation
  - Integration: checkout form submission → order creation → confirmation
  - Component: checkout page renders correctly with various cart states
## Edge Cases:
  - Empty cart attempting checkout
  - Cart item quantity of 0 or negative
  - Discount code that exceeds cart total
  - API timeout during order creation
  - Concurrent cart modifications during checkout
## Mocking Strategy: MSW for API endpoints, real cart logic (no mocking business logic)
## Constraints:
  - Follow existing test patterns (vitest + testing-library)
  - Tests in __tests__/ next to source files
## Success Criteria:
  - Tests cover happy path and all listed edge cases
  - Tests are deterministic and fast (<5s total)
  - All tests pass
```

**Field priorities:** Task (required) > Test Targets (required) > Test Types (important) > Edge Cases (important) > Mocking Strategy (important)

**Anti-pattern:** "Write tests for checkout" with no Edge Cases or Mocking Strategy. The agent writes happy-path-only tests that mock everything, catching nothing.

**Template:**

```
## Task: [One-line summary of what to test]
## Type: Test
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files, including test framework]
  - Related files: [source files to test + existing test files]
  - Team conventions: [testing conventions from boost-patterns.md]
## Test Targets: [Specific functions, modules, or behaviors to test]
## Test Types:
  - [Unit / Integration / E2E — as appropriate]
## Edge Cases:
  - [Boundary conditions, empty inputs, error states, concurrent access]
## Mocking Strategy: [What to mock vs use real implementations]
## Constraints:
  - Follow existing test patterns in the project
  - [Test file location conventions from boost-patterns.md]
## Success Criteria:
  - Tests cover happy path and specified edge cases
  - Tests are deterministic and fast
  - All tests pass
```

---

## Docs Template

**Example transformation:**

Raw: `/boost document the API endpoints`

Boosted:
```
## Task: Document all API endpoints
## Type: Documentation
## Context:
  - Project: Express.js API with TypeScript
  - Related files: src/api/routes/, src/middleware/
  - Team conventions: OpenAPI/Swagger preferred for API docs
## Audience: Frontend developers consuming the API and new backend team members
## Scope: All REST endpoints — method, path, params, body, response, errors
## Format: OpenAPI 3.0 spec (YAML) + human-readable endpoint reference
## Examples Needed:
  - Request/response example for each endpoint
  - Error response examples for common failure cases
  - Authentication header examples
## Constraints:
  - Match existing documentation style
  - Include both success and error responses
## Success Criteria:
  - Every endpoint is documented with method, path, params, body, response
  - Examples are copy-pasteable (valid JSON)
  - New developer can understand and use any endpoint from docs alone
```

**Field priorities:** Task (required) > Audience (required) > Scope (required) > Format (important) > Examples Needed (important)

**Anti-pattern:** "Document the API" with no Audience. The agent writes developer docs when the user needed user-facing docs, or vice versa.

**Template:**

```
## Task: [One-line summary of what to document]
## Type: Documentation
## Context:
  - Project: [framework, language, key dependencies]
  - Related files: [code files being documented]
  - Team conventions: [documentation conventions from boost-patterns.md]
## Audience: [Who will read this — developers, end users, new team members]
## Scope: [What to document — API, architecture, usage, setup]
## Format: [README, inline comments, API docs, runbook, etc.]
## Examples Needed: [What usage examples to include]
## Constraints:
  - Match existing documentation style in the project
  - [Any project-specific docs conventions]
## Success Criteria:
  - Documentation is accurate and complete for the specified scope
  - Examples are runnable
  - Audience can understand without prior context
```

---

## General Template

**Example transformation:**

Raw: `set up CI/CD for this project /boost`

Boosted:
```
## Task: Set up CI/CD pipeline for the project
## Type: General
## Context:
  - Project: Next.js 14 with TypeScript, deployed on Vercel
  - Tech stack: pnpm, vitest, ESLint, Prettier
  - Related files: package.json (scripts section), .github/ (if exists)
  - Team conventions: Main branch is protected, PRs require review
## Goal: Automated pipeline that runs linting, testing, and type-checking on every PR, with automatic deployment to Vercel on merge to main
## Approach:
  - GitHub Actions for CI (lint, test, typecheck on PR)
  - Vercel integration for CD (auto-deploy on merge to main)
  - Status checks required before merge
## Constraints:
  - Use existing package manager (pnpm)
  - Keep pipeline fast (<5 minutes)
## Success Criteria:
  - PRs automatically run lint, test, and typecheck
  - Failed checks block merge
  - Merge to main triggers Vercel deployment
  - Pipeline completes in under 5 minutes
```

**Field priorities:** Task (required) > Goal (required) > Approach (important) > Constraints (nice-to-have)

**Anti-pattern:** "Set up CI/CD" with no Goal or Approach. The agent picks random CI tools and configurations without understanding the team's deployment target.

**Template:**

```
## Task: [One-line summary of the goal]
## Type: General
## Context:
  - Project: [framework, language, key dependencies]
  - Tech stack: [from config files]
  - Recent changes: [relevant commits, if any]
  - Related files: [any files mentioned or implied]
  - Team conventions: [relevant entries from boost-patterns.md]
## Goal: [What the user wants to achieve — clarified and expanded]
## Approach: [Suggested strategy based on the prompt and context]
## Constraints:
  - [Any discovered project constraints]
## Success Criteria:
  - [Measurable outcomes derived from the goal]
```
