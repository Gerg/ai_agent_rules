---
name: code-development
description: Guide for writing production code with automated tests. Use when implementing features, fixing bugs, refactoring code, or writing any production software. Covers development practices, testing principles, code style, and architectural alignment.
---

# Code Development

**Context**: This skill covers writing production code and automated tests. The code quality principles here apply universally - both when writing new code and when reviewing code changes. For systematic code review process, see the pr-review skill. For documentation, see the doc-writing skill.

## Development Practices

### Incremental Development

- Write the minimum amount of code necessary to achieve a goal.
- Address fundamentals before adding complexity.
- Prefer simple solutions to problems; avoid premature optimization and premature refactoring.
- Develop implementation and test code iteratively: make a series of small implementations, gradually working towards the end goal.
- Only change one thing at a time; ensure that the code remains functional after each change.
- After making a change, evaluate implementation and test code, keeping focus on maintaining quality and following instructions.

### Before Starting

- Consider multiple implementation and testing options before starting.
- Think through the approach before writing code.

### Completing Work

- Ensure that solutions are complete before deciding that you've achieved a goal.
- For repetitive changes (e.g., updating multiple tests with the same change), identify the pattern and fix them at once.
- Check your work before completing a task (test compilation, run automated tests, lint, etc.).
- Look for opportunities to improve code hygiene by refactoring, deleting code, etc., and propose them to the user.
- Evaluate if user-facing or internal technical documentation needs to be updated, based on a change.

---

## Automated Testing

### Test Coverage

- All implementation code should have coverage from automated tests.
- Tests should always pass after making changes.
- Write the minimum number of tests necessary to fully exercise the implementation code.
- Tests should always fail if the corresponding implementation code is removed.

### Test-Driven Development

- When a test fails, understand why it is failing before changing any code.
- Be extra careful when modifying both test and implementation code at the same time.
- Avoid adding logic to production code that does not have "test pressure" (logic driven by test requirements).

### Test Quality

- Tests should be concise and focused. Avoid excessive test data and assertions.
- Tests should be BDD-style and easy for a user to understand.
- Tests should always be deterministic (no flaky tests).
- Test data and fixtures should be simple, realistic, and human-readable.

---

## Code Quality

See [references/code-quality.md](references/code-quality.md) for code quality principles:
- Comments (avoid unless absolutely necessary)
- Naming (match existing codebase style)
- Redundancy (avoid multiple mechanisms)
- Test pressure (all logic must be driven by tests)

---

## Code Style

### Readability

- Code should be easy to read and understand, even by a user unfamiliar with the codebase.
- Prefer named intermediate variables over one-liners, to increase legibility.
- Error messages should be explicit, informative, and luxurious.

### Consistency

- Code should always be correctly formatted and pass automated linting/style/formatting checks.

### Simplicity

- Avoid using regular expressions, unless there is not a better option.
- Avoid clever patterns like meta-programming or unnecessary indirection.
- Avoid null guards and null-safe operators for internal data when null is not a possible value, to avoid confusion.
- Prefer lazy evaluation; process data only when needed.

---

## Architecture

- Evaluate and follow existing architectural patterns, whenever possible.
- When introducing new patterns, ensure they align with the codebase's existing approach.

---

## Common Mistakes

❌ **Changing multiple things at once**
- Makes debugging harder when something breaks
- Unclear which change caused the issue

✅ **Change one thing at a time, verify it works, then continue**

---

❌ **Adding code without test pressure**
- Speculative features that aren't driven by requirements
- Logic that isn't exercised by tests

✅ **Only add code that has test pressure (driven by test requirements)**

---

❌ **Modifying tests and implementation simultaneously**
- Easy to mask bugs by changing both
- Hard to verify correctness

✅ **Change one or the other, verify, then change the second**

---

❌ **Premature optimization or refactoring**
- Adds complexity before it's needed
- Solves problems that don't exist yet

✅ **Address fundamentals first, optimize when needed**

---

❌ **Excessive test data or assertions**
- Makes tests hard to understand
- Obscures what's actually being tested

✅ **Minimal, focused tests with clear intent**

---

## Related Skills

- **[PR Review](../pr-review/SKILL.md)** - Review code changes systematically
- **[Documentation Writing](../doc-writing/SKILL.md)** - Write user-facing documentation
- **[Story Writing](../story-writing/SKILL.md)** - Write user stories for features
