# Skill: efficient-coding

# Core Directive

Code efficiently without cognitive or structural waste.
Every character in the codebase should earn its keep.

# Principles

## 1. Solve the Task, Nothing More
- Answer the exact question. Do not add extras, do not pre-optimize.
- If a test passes and the code is clean, you are done — do not polish further.
- Do not refactor unrelated code unless it blocks the task.

## 2. Small Focused Edits
- Prefer 1-3 line changes over rewriting entire functions.
- When fixing a bug, change only what's broken.
- When adding a feature, touch only the minimum necessary files.
- Use `Edit` over `Write` when modifying existing files.

## 3. Leverage Existing Patterns
- Before creating anything, grep for similar patterns in the codebase.
- Mirror the style, conventions, and structure the project already uses.
- If the project uses DTOs, create DTOs. If it uses inline types, use inline types.

## 4. No Dead Ends
- Do not leave TODO comments without action.
- Do not add console.log, debugger, or commented-out code.
- Do not leave unused imports, variables, or parameters.
- Do not leave empty catch blocks or unhandled promises.

## 5. Names That Reveal Intent
- `getUserById()` not `fetchData()`
- `isActive()` not `checkStatus()`
- `updateInventory(onHand, committed)` not `updateInventory(a, b)`
- A name should tell you what and why, not how.

## 6. One File, One Responsibility
- If a file has more than 300 lines, consider splitting.
- If a function has more than 30 lines, consider splitting.
- If a function does more than one thing, it's doing too much.

## 7. Tests Are Not Optional
- Every new function deserves a test.
- Every bug fix needs a regression test.
- Test behavior, not implementation.

## 8. Avoid Premature Abstraction
- Do not extract shared code before the third occurrence.
- Do not create interfaces with a single implementation.
- Do not create configuration systems for one environment.
- Do not create factories for one product type.

## 9. Prefer Standard Library
- Use built-in language features before pulling in dependencies.
- Use language-native patterns before framework-specific ones.
- A dependency is a liability — every import is a contract.

## 10. Read Before You Write
- Read the file you're about to edit.
- Read related files to understand context.
- Read the test file for the module you're changing.
- Reading first prevents rewriting.
