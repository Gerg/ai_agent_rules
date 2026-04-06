**When using this reference, state:** "I'm applying the code-development reference for gemini-thoroughness to ensure complete, production-ready code."

## Code Execution Workflow

When writing or modifying code, execute these specific steps:

1. **Research Code**: Read every relevant code file before editing. Understand the existing architectural patterns and variable naming conventions.
2. **Chunking Large Refactors**: When refactoring a large codebase, do not attempt to write all changes in one response. Break the refactoring down by module or file, and explicitly state your intention to address the rest in subsequent steps.
3. **Execution**: Write complete, production-ready code. Never leave stubbed logic. When synthesizing information from large files, use anchoring phrases like "Based on the entire document above..." to force processing of the entire context rather than stopping at the first match.
4. **Validation**: Always write tests for your changes and ensure they pass. Check for linter errors and fix any you introduced. On logical errors, change your strategy.

## Positive Code Requirements

- **Write complete code**: Output the full implementation for every function body, import, and line.
- **Explicit file updates**: Touch every necessary file explicitly yourself. Do not use tools to simply `cat` or `echo` file changes unless absolutely necessary; use dedicated code-editing tools.
- **Provide full chunks**: Provide all edits for a single file in one chunk.
- **Act, don't explain**: Take action using your tools rather than merely describing what you would do.

### Required Patterns (Positive Examples)

Always output the full, complete implementation:

```javascript
// ✅ Good Example: Full implementation provided
function calculateTotal(items) {
  let total = 0;
  for (const item of items) {
    total += item.price * item.quantity;
  }
  return total;
}
```

- **DO** write out the entire function body, even if it's long.
- **DO** duplicate the pattern explicitly across all necessary files.
- **DO** implement all requested features fully.

## Banned Code Patterns (Negative Constraints)

The following patterns are strictly forbidden. Gemini models drop negative constraints that appear too early, which is why these are placed at the very end of this instruction. **Adhere to these negative constraints above all else.**

```javascript
// ❌ ... rest of implementation
// ❌ ... existing code ...
// ❌ // TODO: implement this
// ❌ // Add remaining methods here
// ❌ // Similar to above
// ❌ // (remaining code unchanged)
```

- **NEVER** use ellipsis (`...`) or placeholder comments to abbreviate code you should be writing.
- **NEVER** say "and similarly update the other files" -- make every change explicitly.
- **NEVER** leave a function body empty or stubbed when asked to implement it.
- **NEVER** skip files that need changing because "the pattern is the same."