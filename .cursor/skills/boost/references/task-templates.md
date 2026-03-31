# Boost Task Templates

## How to Use

Match the detected task category to the corresponding template below. Fill in each field using the raw prompt and discovered context. If a field cannot be filled, write "Unknown — investigate" rather than omitting it.

---

## Refactor Template

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

## Debug Template

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

## Feature Template

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

## Review Template

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

## Test Template

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

## Docs Template

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

## General Template

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
