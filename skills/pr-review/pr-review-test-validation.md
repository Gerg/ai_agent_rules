# PR Review: Test-Driven Validation

**Category:** Process skill - Extension

## Purpose
Validate suspected bugs and edge cases empirically through tests before creating tickets.

## When to Use
- Suspect a validation gap
- Identify potential edge case
- Question whether framework handles something
- Before creating any bug ticket

## Prerequisites
- Access to test suite and test environment
- Ability to run tests locally
- Understanding of the testing framework
- Knowledge of how to write tests in the codebase

## Note on Examples

This skill includes examples in multiple languages (Ruby/RSpec, Go, Python/pytest, JavaScript/Jest) to illustrate universal patterns. Adapt the concepts to your specific testing framework.

## Process

### 1. Identify the Suspected Issue
Document what you think might be wrong:
- What input/scenario might fail?
- What should happen vs. what might happen?
- Why do you think it's a bug?

### 2. Locate Relevant Test File
Find where tests for this component live:
- Check standard test directory structure
- Look for test files matching component name
- Search for existing tests of similar components
- Follow project's test organization conventions

### 3. Write a Failing Test
Add test that demonstrates the bug:

**General structure:**
- Set up the edge case scenario
- Execute the code under test
- Assert expected behavior
- Check for expected errors/exceptions

**Example (Ruby/RSpec):**
```ruby
context 'when input has trailing whitespace' do
  let(:input) { 'value   ' }

  it 'rejects the input' do
    expect(validator.valid?(input)).to be false
    expect(validator.errors).to include('must not have trailing whitespace')
  end
end
```

**Example (Go):**
```go
func TestValidator_TrailingWhitespace(t *testing.T) {
    input := "value   "
    
    err := validator.Validate(input)
    
    assert.Error(t, err)
    assert.Contains(t, err.Error(), "trailing whitespace")
}
```

**Example (Python/pytest):**
```python
def test_validator_rejects_trailing_whitespace():
    input_value = "value   "
    
    with pytest.raises(ValidationError) as exc:
        validator.validate(input_value)
    
    assert "trailing whitespace" in str(exc.value)
```

**Example (JavaScript/Jest):**
```javascript
test('rejects input with trailing whitespace', () => {
  const input = 'value   ';
  
  expect(() => validator.validate(input))
    .toThrow('trailing whitespace');
});
```

### 4. Run the Test
Execute the test using your project's test runner:

**Ruby/RSpec:**
```bash
bundle exec rspec spec/validators/input_validator_spec.rb:42
```

**Go:**
```bash
go test -v -run TestValidator_TrailingWhitespace ./pkg/validator
```

**Python/pytest:**
```bash
pytest tests/test_validator.py::test_validator_rejects_trailing_whitespace -v
```

**JavaScript/Jest:**
```bash
npm test -- validator.test.js -t "rejects input with trailing whitespace"
```

### 5. Interpret Results

#### If Test PASSES ✅
The suspected bug doesn't exist:
- Document why it's not a bug
- Include test code as evidence
- Describe what actually happens
- Do NOT create bug tracking item

#### If Test FAILS ❌
Real bug confirmed:
- Create bug tracking item
- Include test that demonstrates bug
- Document expected vs actual behavior
- Provide fix recommendation

#### If Test is Inconclusive 🤔
Sometimes tests don't clearly prove or disprove the issue:
- Test setup may not match production conditions
- Framework behavior may differ in test vs. production
- Race conditions may not reproduce in tests
- External dependencies affect results

**Actions:**
- Document what the test does/doesn't prove
- Consider integration or manual testing
- Gather production evidence if available
- Make recommendation with caveats
- Note limitations in tracking item

### 6. Consider Keeping Test
Even if test passes (no bug), consider committing it:
- Documents expected behavior
- Prevents regression
- Clarifies edge case handling

## Examples

### Example 1: Validation Gap (False Positive)

**Suspected:** Whitespace-only strings bypass presence validation

**Ruby/RSpec test:**
```ruby
context 'when name is whitespace-only' do
  let(:params) { { name: '   ' } }

  it 'is not valid' do
    expect(validator).not_to be_valid
    expect(validator.errors[:name]).to include("can't be blank")
  end
end
```

**Run test → PASSES** (validation already handles this)

**Conclusion:** Not a bug, framework's `presence: true` uses `blank?` which handles whitespace

**Action:** Do NOT create ticket, document finding

---

### Example 2: Real Bug

**Suspected:** Input with trailing whitespace bypasses validation

**Python/pytest test:**
```python
def test_rejects_trailing_whitespace():
    validator = InputValidator(value='forbidden   ')
    
    assert not validator.is_valid()
    assert 'trailing whitespace' in validator.errors
```

**Run test → FAILS** (validation doesn't handle trailing whitespace)

**Conclusion:** Real bug, validation needs to strip/trim input before checking

**Action:** Create bug ticket with test and fix

---

### Example 3: Edge Case Coverage

**Suspected:** Concurrent creation might violate uniqueness constraint

**Go test:**
```go
func TestConcurrentCreation(t *testing.T) {
    var wg sync.WaitGroup
    results := make(chan error, 2)
    
    for i := 0; i < 2; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            _, err := service.Create(sameEmail)
            results <- err
        }()
    }
    
    wg.Wait()
    close(results)
    
    errorCount := 0
    for err := range results {
        if err != nil {
            errorCount++
        }
    }
    
    assert.Equal(t, 1, errorCount, "exactly one creation should fail")
}
```

**Run test → Result determines if race condition exists**

**Action:** Create ticket if race condition confirmed

## Decision Tree

```
Suspect a bug?
    ↓
Can you write a test for it?
    ↓
   Yes → Write test
    ↓
Run test
    ↓
    ├─ PASSES → Not a bug
    │           Document & move on
    │
    └─ FAILS → Real bug
               Create ticket with test
```

## Benefits

1. **Eliminates False Positives** - No tickets for non-issues
2. **Provides Evidence** - Test demonstrates the bug
3. **Speeds Up Fixes** - Test is already written
4. **Builds Confidence** - Empirical validation
5. **Documents Behavior** - Test serves as specification

## Anti-Patterns

### ❌ Creating tickets based on "what if"
```
"What if someone passes whitespace-only strings?"
→ Write test first, then decide
```

### ❌ Assuming framework gaps
```
"The validation probably doesn't handle X"
→ Test it, frameworks are mature
```

### ❌ Trusting documentation over tests
```
"The docs say it works, so it must"
→ Test it anyway, docs can be wrong
```

## Integration with Core Review

In core review process, step 4 becomes:
```
4. Validate Findings with Tests
   - Use this add-on skill
   - Write test for each suspected bug
   - Only create tickets for confirmed bugs
```

## Success Criteria
- Every suspected bug has a test
- No bug tickets without failing tests
- False positives documented with passing tests
- Test suite improved with new coverage
