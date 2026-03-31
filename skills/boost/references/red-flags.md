# Boost — Iron Laws & Red Flags

## Iron Laws

These are non-negotiable. Violating any of these is a skill failure.

1. **NEVER execute without filling the Task and Type fields** — these are the minimum viable enhancement
2. **NEVER modify boost-patterns.md without explicit user confirmation** — this is team-shared state
3. **NEVER exceed the 2000-line context discovery budget** — protect the context window
4. **NEVER skip context discovery even in fast-track mode** — fast-track skips confirmation, not discovery
5. **NEVER re-announce the structured prompt when executing** — just begin working on the task
6. **NEVER guess file paths** — if you can't resolve a module name, write "Unknown — investigate"
7. **NEVER add constraints the user didn't imply** — infer from project conventions only, don't invent restrictions

## Red Flags — Rationalization Prevention

If you catch yourself thinking any of these, STOP and correct course:

| Thought | Wrong Action | Correct Action |
|---------|-------------|----------------|
| "This prompt is already clear enough" | Rewrite it anyway, adding noise | Run passthrough check. If structured, execute as-is |
| "I don't have enough context to fill this field" | Fill it with a guess | Write "Unknown — investigate" |
| "The user said just do it" | Skip all context discovery | Still discover context; fast-track only skips confirmation |
| "This doesn't fit any category" | Force-fit into the closest match | Use the General template |
| "boost-patterns.md is empty anyway" | Skip reading it entirely | Still read it; offer to populate after execution |
| "The context budget is just a guideline" | Read 5000 lines of files | Hard stop at 2000 lines, truncate with [truncated] note |
| "I should add more constraints to be safe" | Invent constraints not in the project | Only use constraints from project conventions or task-type defaults |
| "The user probably means X" | Assume intent for ambiguous terms | Write the ambiguity into the prompt so the executing agent clarifies |
| "This template field isn't important" | Omit it | Fill every field — use "Unknown — investigate" if needed |
| "I'll just fix the code instead of enhancing the prompt" | Start coding | You are a prompt enhancer. Enhance the prompt, then let the agent code |
| "This is two tasks but I can combine them" | Merge into one enhanced prompt | Tell user: "This looks like two tasks. Want me to boost them individually?" |

## DO NOTs

- Do NOT combine multiple task types into one enhanced prompt
- Do NOT add emoji or decorative formatting to the enhanced prompt
- Do NOT change the user's intent — you structure and contextualize, you do not redefine
- Do NOT read files beyond the context budget even if you think they're relevant
- Do NOT suggest patterns for common English words (e.g., "the", "page", "data", "module")
- Do NOT trigger on the word "boost" without a leading slash (e.g., "boost performance" is not /boost)
- Do NOT enhance prompts that are asking ABOUT boost (e.g., "what does /boost do?")
