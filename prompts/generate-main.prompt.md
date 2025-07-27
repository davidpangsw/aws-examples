# Lab Generation Instructions

When asked to generate a main note file (`main.md`) for `<lecture>` (usually a number, but not always):

## Process

1. **Read and understand**: Read `lectures/<lecture>/lecture.md` and understand what has been learnt.

2. **Apply rules**: Follow the guidelines in `prompts/common/main-rule.md` for structure and organization, if it exists.

3. **Apply lecture-specific rules**: If `lectures/<lecture>/main.prompt.md` exists, follow those additional guidelines.

4. **Generate lab file**: Create `lectures/<lecture>/main.md` containing practice tasks for ALL content in `lectures/<lecture>/lecture.md`. Do not miss any commands, concepts, or operations.

## Key Requirements

- Cover every command and operation from the source lecture. Never miss anything
- Apply any lecture-specific guidelines if they exist
