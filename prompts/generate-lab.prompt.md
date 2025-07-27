# Lab Generation Instructions

When asked to generate a lab for `<lecture>` (usually a number, but not always):

## Process

1. **Read and understand**: Read `lectures/<lecture>/lecture.md` and understand what has been learnt.

2. **Apply rules**: Follow the guidelines in `prompts/common/rule.md` for structure and organization.

3. **Apply lecture-specific rules**: If `lectures/<lecture>/lab.prompt.md` exists, follow those additional guidelines.

4. **Generate lab file**: Create `lectures/<lecture>/lab.md` containing practice tasks for ALL content in `lectures/<lecture>/lecture.md`. Do not miss any commands, concepts, or operations.

## Key Requirements

- Cover every command and operation from the source lecture
- Create hands-on practice tasks, not just theoretical questions
- Follow the structural and ordering rules from `rule.md`
- Apply any lecture-specific guidelines if they exist
