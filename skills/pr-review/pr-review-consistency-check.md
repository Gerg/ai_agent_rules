# PR Review: Consistency Check

**Category:** Process skill - Extension

## Purpose
Verify that new code follows existing codebase patterns and conventions.

## When to Use
- Reviewing new features
- Checking architectural alignment
- Validating style consistency
- Ensuring maintainability

## Prerequisites
- Understanding of the project's architecture
- Access to existing codebase for comparison
- Familiarity with the programming language and framework
- Knowledge of project's coding standards

## Process

### 1. Identify What's Being Added

Determine the type of change:
- New feature or component
- Modification to existing code
- New test coverage
- Documentation or configuration

### 2. Find Similar Existing Code

Search the codebase for similar functionality:
- Use file naming patterns to find related code
- Look in relevant directories
- Search for similar class/module/function names
- Check recent commits for similar work

### 3. Compare Patterns

Check for consistency with existing code:

#### Code Structure
- [ ] Inherits from correct base classes/interfaces
- [ ] Follows naming conventions
- [ ] Uses established error handling patterns
- [ ] Implements required methods/interfaces
- [ ] Follows file/directory structure conventions
- [ ] Uses consistent coding style

#### Architectural Patterns
- [ ] Respects existing layered architecture (if present)
- [ ] Business logic in appropriate layer
- [ ] Follows separation of concerns
- [ ] Uses established abstractions
- [ ] Matches dependency injection patterns

**Example - Pattern Comparison:**

```
1. Find existing similar code:
   - Search for similar functionality
   - Identify the pattern used

2. Compare structure:
   - Does new code follow the same approach?
   - Are responsibilities organized the same way?
   - Does it use the same abstractions?

3. Document differences:
   - Note any deviations
   - Assess if justified or should be aligned
```

#### Tests
- [ ] Test organization and structure
- [ ] Shared examples/helpers usage
- [ ] Assertion patterns
- [ ] Test data setup
- [ ] Naming conventions

### 4. Document Deviations

For each deviation from patterns, create a tracking item with:
- Component and pattern name
- Existing pattern (with example)
- New code showing deviation
- Impact assessment
- Recommendation (align or justify)

### 5. Check Architectural Patterns

Verify the code respects the codebase's architectural approach:

- [ ] Follows established layering (if present)
- [ ] Respects separation of concerns
- [ ] Uses appropriate abstractions
- [ ] Matches error handling approach
- [ ] Follows dependency patterns
- [ ] Clear boundaries between components

### 6. Generate Consistency Report

Document findings in structured format:
- **Components Checked**: List all reviewed components
- **Patterns Followed**: List correct patterns ✅
- **Deviations Found**: List deviations with references ⚠️
- **Architectural Alignment**: Overall assessment ✅/⚠️
- **Recommendation**: Overall consistency assessment

## Quick Checklist

For any new component:
- [ ] Similar to existing components in structure
- [ ] Follows established naming conventions
- [ ] Uses consistent error handling
- [ ] Respects architectural boundaries
- [ ] Tests follow existing patterns
- [ ] Uses shared helpers/utilities where applicable

## Common Issues

### 🚩 Violating Architectural Boundaries
New code places logic in the wrong layer or violates separation of concerns established in the codebase.

### 🚩 Different Error Handling Pattern
New code uses different error handling approach than existing code without justification.

### 🚩 Reinventing Existing Patterns
New code implements functionality that existing helpers/utilities already provide.

### 🚩 Inconsistent Naming
New code doesn't follow established naming conventions (e.g., `CreateUser` vs existing pattern of `UserCreate`).

### 🚩 Not Using Shared Test Helpers
New tests manually implement patterns that shared examples/helpers already cover.

### 🚩 Skipping Established Abstractions
New code bypasses established abstractions (base classes, mixins, interfaces) without reason.

## Success Criteria
- All components follow existing patterns
- Deviations documented with justification
- Architectural alignment verified
- Consistency report generated
- Tracking items created for misalignments

## Integration with Core Review

In core review process, add consistency as a review category with:
- Clear scope (which components to check)
- Link to main review
- Specific patterns to verify
