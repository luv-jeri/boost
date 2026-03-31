---
name: boost
description: Use when user appends or prefixes /boost or /boost! to their prompt. Restructures rough prompts into clear, structured, context-rich prompts before execution.
user-invokable: true
---

# Boost — Prompt Enhancer

Transform rough prompts into structured, context-rich prompts before execution.

## Invocation

- **Prefix:** `/boost refactor the auth module`
- **Suffix:** `refactor the auth module /boost`
- **Fast-track:** `refactor the auth module /boost!` (skip confirmation)

## Process

Read `references/flow.md` for the complete enhancement process.

1. CAPTURE — strip /boost trigger
2. PASSTHROUGH CHECK — already structured? (`references/prompt-passthrough.md`)
3. DETECT — classify task type
4. DISCOVER — read project context (`references/context-discovery.md`)
5. STRUCTURE — apply template (`references/task-templates.md`)
6. PRESENT — show enhanced prompt
7. DECIDE — confirm, edit, skip, or auto-execute
8. EXECUTE — work on the structured prompt
9. SUGGEST — offer pattern additions

## Iron Laws

Read `references/red-flags.md` before proceeding. Non-negotiable.
