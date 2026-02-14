# End-to-End Validation

Validate that features work from the user's perspective, not just that individual operations succeed.

## Table of Contents

- [Core Principle](#core-principle)
- [User Workflow Thinking](#user-workflow-thinking)
  - [The Question](#the-question)
  - [The Process](#the-process)
  - [Example Workflows by Domain](#example-workflows-by-domain)
- [Red Flags](#red-flags)
  - [Tests Only Verify One Step](#tests-only-verify-one-step)
  - [Tests Use Internal APIs](#tests-use-internal-apis-instead-of-user-facing-apis)
  - [Separate Storage Without Integration](#separate-storage-without-integration)
  - [No Integration Tests](#no-integration-tests)
- [Facade Bug Pattern](#facade-bug-pattern)
- [Architectural Integration](#architectural-integration)
- [Manual Testing Validation](#manual-testing-validation)
- [Test Coverage Completeness](#test-coverage-completeness)
- [Practical Application](#practical-application)
- [Success Criteria](#success-criteria)
- [Integration with Core Review](#integration-with-core-review)

## Core Principle

**"Operation succeeds" ≠ "Feature works"**

A feature can successfully execute individual operations while being completely unusable to the end user.

## User Workflow Thinking

### The Question

For any new feature or significant change, ask:

**"How would a user actually use this end-to-end?"**

### The Process

1. **Identify the trigger** - How does the user initiate this?
2. **Trace the flow** - What happens step by step?
3. **Find the result** - Where/how does the user access the outcome?
4. **Verify usability** - Can the user actually use what was created?

### Example Workflows by Domain

**REST API:**
- Create resource → Retrieve resource → Use resource → Delete resource

**CLI Tool:**
- Run command → Check output → Use output in next step

**Library/SDK:**
- Call function → Receive result → Use result in application

**UI Feature:**
- User action → Visual feedback → State change → User can see/use result

**Data Processing:**
- Input data → Process → Output data → Downstream systems can consume

**Background Job:**
- Trigger job → Job executes → Result stored → User can access result

## Red Flags

### Tests Only Verify One Step

**Warning signs:**
- Tests verify creation but not retrieval
- Tests verify processing but not output accessibility
- Tests verify storage but not querying
- Tests verify success response but not actual functionality

**Questions to ask:**
- "Do tests cover the complete workflow?"
- "Is there a test that goes from start to finish?"
- "Do tests verify from the user's perspective?"

### Tests Use Internal APIs Instead of User-Facing APIs

**Warning signs:**
- Tests query database directly instead of using public API
- Tests call internal methods instead of public interface
- Tests verify internal state instead of external behavior

**Questions to ask:**
- "How would a real user access this?"
- "Do tests use the same interface users will use?"
- "Are we testing implementation or behavior?"

### Separate Storage Without Integration

**Warning signs:**
- New data stored in separate location (table, file, cache)
- No code showing how existing queries include new data
- No integration with existing retrieval mechanisms

**Questions to ask:**
- "How does this integrate with existing queries?"
- "Where does this data appear in existing interfaces?"
- "Is there glue code connecting this to existing systems?"

### No Integration Tests

**Warning signs:**
- Only unit tests for new code
- No tests showing interaction with existing code
- Tests run in isolation without dependencies

**Questions to ask:**
- "How does this integrate with existing functionality?"
- "Are there tests showing the integration works?"
- "What happens when existing code tries to use this?"

## Facade Bug Pattern

### What Is a Facade Bug?

A bug where a feature **appears to work** but **doesn't actually work**.

**Characteristics:**
- Operation returns success
- Data is created/stored
- Tests pass
- But... users can't access or use the result

### How to Detect

1. **Operation succeeds** ✅
2. **Data is created** ✅
3. **But where does it appear?** ❓
4. **Can users access it?** ❓
5. **Does it actually function?** ❓

### Questions to Ask

- "Where does this appear in the user interface?"
- "How do users discover this exists?"
- "Can you trace the path from creation to usage?"
- "What happens when a user tries to access this?"

### Prevention

- Always think through the complete workflow
- Don't trust success responses without verifying accessibility
- Consider the user's perspective, not just the implementation
- Verify integration with existing discovery/access mechanisms

## Architectural Integration

### When Adding to Existing Systems

**Questions to ask:**

**About storage:**
- Why separate storage vs extending existing?
- How does new data integrate with existing queries?
- Is there code that bridges the two?

**About interfaces:**
- Does this extend existing interfaces or create new ones?
- How do existing callers discover this?
- Will future changes to base system apply here?

**About patterns:**
- Why different pattern vs extending existing?
- Is logic duplicated or shared?
- Will changes to existing code affect this?

### Red Flags

- Separate storage for same conceptual resource
- Separate implementation instead of extending base
- No integration code with existing access patterns
- Duplicated logic that will drift over time

## Manual Testing Validation

### When to Request Evidence

For significant changes or new features, consider asking:

**General:**
- "Have you tested this manually end-to-end?"
- "Can you walk through the user workflow?"
- "What does the user experience look like?"

**For data changes:**
- "Can you show the data is accessible?"
- "Does it appear in relevant queries/views?"

**For integration changes:**
- "How does this integrate with existing functionality?"
- "Can you demonstrate the integration works?"

### What Not to Rely On Solely

❌ "Tests pass" - Tests may not cover complete workflow
❌ "Code looks good" - May be a facade
❌ "Follows patterns" - May be wrong patterns for this case

✅ Manual testing performed
✅ Complete workflow verified
✅ Integration demonstrated

## Test Coverage Completeness

### Beyond Quantity

**Many lines of tests ≠ Complete coverage**

What matters:
- Do tests cover the complete user workflow?
- Do tests use user-facing interfaces?
- Do tests verify integration with existing code?

### Required Coverage for New Features

**Minimum workflow coverage:**
1. Trigger operation (create, process, etc.)
2. Access result (retrieve, query, etc.)
3. Use result (consume, display, etc.)
4. Clean up (delete, reset, etc.)

**Integration coverage:**
- New code works with existing code
- Existing code can access new data
- Changes don't break existing workflows

## Practical Application

### During Review

**When you see new feature code:**

1. **Identify the user workflow**
   - What's the intended use case?
   - How would a user interact with this?

2. **Check test coverage**
   - Do tests cover the complete workflow?
   - Do tests use user-facing interfaces?
   - Are there integration tests?

3. **Trace the integration**
   - How does this connect to existing code?
   - Where does this appear in existing interfaces?
   - Is there glue code?

4. **Ask questions if unclear**
   - "How would a user access this?"
   - "Can you show me the complete flow?"
   - "How does this integrate with existing functionality?"

### Example Questions by Scenario

**New resource creation:**
- "How do users retrieve what they created?"
- "Where does this appear in list/query operations?"

**New data processing:**
- "Where does the output go?"
- "How do downstream systems access it?"

**New UI feature:**
- "How do users discover this?"
- "What's the visual feedback?"

**New background job:**
- "How do users see the results?"
- "What happens if the job fails?"

**New API endpoint:**
- "How does this fit with existing endpoints?"
- "Is this discoverable via existing mechanisms?"

## Success Criteria

- [ ] Complete user workflow identified
- [ ] Tests cover end-to-end flow, not just individual operations
- [ ] Integration with existing code verified
- [ ] User perspective considered
- [ ] No facade bugs (feature actually works, not just succeeds)
- [ ] Manual testing evidence considered when appropriate

## Integration with Core Review

Use this reference during step 5 (Validate Findings Empirically) when reviewing new features or significant changes to existing functionality.
