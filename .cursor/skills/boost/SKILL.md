---
name: boost
description: Use when user appends or prefixes /boost to their prompt. Restructures rough prompts into clear, structured, context-rich prompts before execution. Supports /boost! for auto-execute without confirmation.
---

# Boost — Prompt Enhancer

Transform rough, unstructured prompts into clear, structured, context-rich prompts before execution.

## Invocation

The user triggers this skill by including `/boost` or `/boost!` in their prompt:
- **Prefix:** `/boost refactor the auth module`
- **Suffix:** `refactor the auth module /boost`
- **Fast-track:** `refactor the auth module /boost!` (skip confirmation, auto-execute)

## Core Flow

1. CAPTURE raw prompt (strip /boost or /boost! trigger)
2. DETECT task type from keywords
3. DISCOVER project context (read references/context-discovery.md)
4. STRUCTURE prompt using matching template (read references/task-templates.md)
5. PRESENT enhanced prompt to user
6. DECIDE: confirm, edit, or auto-execute
7. EXECUTE the structured prompt
8. SUGGEST pattern additions if unknown terms found

## Step 1: Capture

Strip `/boost` or `/boost!` from the user's input. The remaining text is the raw prompt.

- If `/boost!` was used, set fast-track mode = ON (skip confirmation in Step 6)
- If `/boost` was used, set fast-track mode = OFF (show confirmation)
- The trigger can appear at the start or end of the prompt — handle both

## Step 2: Detect Task Type

Scan the raw prompt for keywords (case-insensitive) to classify into one of 7 categories:

| Category | Keywords |
|----------|----------|
| Debug | fix, bug, broken, error, not working, crash, failing, issue |
| Feature | add, create, build, implement, new, make, setup, introduce |
| Refactor | refactor, clean up, reorganize, simplify, messy, restructure, improve, optimize |
| Test | test, coverage, spec, assert, unit test, integration test |
| Review | review, check, audit, look at, examine, inspect |
| Docs | document, readme, explain, comment, describe, write docs |
| General | (fallback — no keywords matched) |

**Tie-breaking priority:** Debug > Feature > Refactor > Test > Review > Docs > General

Count keyword matches per category. Highest count wins. If tied, use the priority order above. Debug is highest because "fix" and "error" co-occur with other categories but debugging is almost always the primary intent.

## Step 3: Discover Context

Read `references/context-discovery.md` for the full discovery rules.

**Quick summary of priority order:**
1. **Project config** (always) — `.cursorrules`, `.cursor/rules/`, `package.json`, `tsconfig`, etc.
2. **Git context** (if available) — recent commits, changed files, branch name
3. **File structure** (if prompt mentions modules) — directory listing, file previews
4. **Team patterns** (always) — `boost-patterns.md` from project root

Stay within ~2000 line budget. If `boost-patterns.md` does not exist, skip silently — you will offer to create it in Step 8.

**Track unresolved terms** during discovery: any module/component name in the raw prompt that could not be resolved to a file path or alias.

## Step 4: Structure the Prompt

Read `references/task-templates.md` and select the template matching the detected task type.

Fill in every field:
- **Task:** Rewrite the raw prompt as a clear one-line summary
- **Type:** The detected category name
- **Context:** Populate from discovered project context
- **Category-specific sections:** Fill using the raw prompt content and discovered context
- **Constraints:** Combine project conventions + task-type defaults
- **Success Criteria:** Derive measurable outcomes from the task description

If a field cannot be filled, write "Unknown — investigate" rather than omitting it.

## Step 5: Present

Display the enhanced prompt to the user in a clear, readable format.

Prefix it with:

**Boost enhanced your prompt:**

## Step 6: Decide

**If fast-track mode is ON** (`/boost!` was used):
- Skip confirmation entirely
- Proceed directly to Step 7

**If fast-track mode is OFF** (`/boost` was used):
- Show the enhanced prompt (Step 5)
- Ask: "Execute this enhanced prompt? (yes / edit / skip)"
  - **"yes" or "y"** — proceed to Step 7 with the enhanced prompt
  - **"edit"** — user provides modifications, incorporate them, then proceed to Step 7
  - **"skip" or "s"** — proceed to Step 7 immediately

## Step 7: Execute

Execute the structured prompt as if the user had typed it directly. The agent now works on the task with full context and clear structure.

IMPORTANT: Do not re-announce the prompt. Just begin working on the task described in the structured prompt.

## Step 8: Suggest Pattern Additions

After the task execution completes, if there were unresolved terms from Step 3:

1. Collect up to 3 unresolved terms
2. Present them to the user:

   Boost noticed terms it could add to your team patterns:
     - "[term]" — what does this refer to? (file path, module, service?)
   Want me to add these to boost-patterns.md?

3. **Never auto-write** — always ask first
4. If user confirms, append to the appropriate section in `boost-patterns.md`
5. If `boost-patterns.md` does not exist, offer to create it with a starter template

**Rules for suggestions:**
- Maximum 3 suggestions per invocation
- Never interrupt the main task — suggestions come AFTER execution only
- If the user declines, do not ask again for the same terms in the same session
