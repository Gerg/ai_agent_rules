# Code Quality

Universal code quality principles. Apply both when **writing code** and when **reviewing code**.

---

## Comments

**Avoid code comments unless absolutely necessary; Code & tests should be self-documenting without comments.**

### When Comments Are Acceptable

Comments are acceptable **only** when they explain non-obvious intent that cannot be expressed in code:
- Why this approach was chosen over alternatives
- Trade-offs or constraints that aren't obvious from the code
- Links to tickets, RFCs, or external documentation
- Workarounds for external system bugs or limitations

### Red Flags

**Remove these comments:**
```
❌ // Set the value to x
value = x

❌ // Loop through items
for item in items

❌ // Check if empty
if list.isEmpty()
```

**Default action: Remove the comment and improve the code.**

If existing code has bad comments, flag them - don't perpetuate bad patterns.

---

## Naming

**New code should match the existing style of a codebase, whenever possible.**

Find similar existing code and use the same naming patterns.

---

## Redundancy

Avoid multiple mechanisms doing the same thing. Common redundancies:

**Dead code:**
- Commented-out blocks
- Unused imports, variables, or methods
- Abandoned feature flags

**Multiple mechanisms:**
- Global mechanism (framework plugin, base class) + local mechanism (model hook, initializer)
- Validation duplicated across layers (API, service, model)
- Authorization checks duplicated across layers (middleware, controller, service)

**Detection:**
1. Find global mechanisms (framework plugins, base classes, configuration)
2. Find local mechanisms (hooks, explicit code)
3. Trace execution - what runs first? What's redundant?
4. Compare with existing patterns - how does existing code handle this?

---

## Test Pressure

**Avoid adding logic to production code that does not have "test pressure".**

**Test pressure** is when tests require specific implementation logic to pass. If you can remove logic without breaking tests, that logic lacks test pressure.

### Red Flags

**Logic without corresponding tests:**
- Conditional branches not exercised by tests
- Error handling paths not tested
- Edge case handling without edge case tests
- Configuration options not tested

**Speculative features:**
- "We might need this later"
- "This makes it more flexible"
- "This handles a case that could happen"
- Without tests demonstrating the need

### Validation

1. **Identify implementation logic** - conditionals, error handling, edge cases, configuration
2. **Find corresponding tests** - is there a test that exercises this branch?
3. **Check if justified** - what test would fail if I removed this logic?
4. **If no test would fail** → logic lacks test pressure → remove it or add tests

### Examples

**With test pressure ✅**
```
if input.empty?
  raise ValidationError, "Input cannot be empty"
end

# Test requires this check
it 'rejects empty input' do
  expect { process('') }.to raise_error(ValidationError)
end
```

**Without test pressure ❌**
```
if input.empty?
  raise ValidationError, "Input cannot be empty"
elsif input.length > 1000
  raise ValidationError, "Input too long"
end

# No test exercises empty or length checks
it 'processes valid input' do
  expect(process('valid')).to eq(result)
end
```
Could remove both checks without breaking tests → no test pressure.

---

### Test Data Should Reflect Production Invariants

Test setup should include prerequisites that the production system enforces. Tests that bypass system-enforced prerequisites can pass vacuously - the scenario may never occur in production.

**Check:** Does the test setup reflect constraints the production system enforces?

---

## Understanding the Codebase

Before applying these principles, understand the existing codebase:

**Study existing code:**
- Find similar components
- See how they handle common concerns
- Identify naming patterns and conventions

**Check for global mechanisms:**
- What's handled automatically by framework
- What plugins or middleware are active
- What base classes provide

**Apply these principles consistently:**
- Remove unnecessary comments (even if existing code has them)
- Match existing naming patterns
- Identify and flag redundancy
- Ensure all logic has test pressure

**These principles apply regardless of existing code quality.** If existing code has bad comments or redundancy, flag it - don't perpetuate bad patterns.
