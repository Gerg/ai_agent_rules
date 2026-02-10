---
name: pr-review
description: Systematic code review process for pull requests. Use when conducting code reviews to ensure comprehensive coverage including scope understanding, categorization, duplication checking, test-driven validation, scope validation, and consistency checking.
---

# PR Review

Conduct thorough, systematic code reviews using test-driven validation and issue tracking.

## Using This Skill

This skill provides the core PR review process. For specialized review techniques, see:

**Validation techniques:**
- **Scope Validation**: See [references/scope-validation.md](references/scope-validation.md) for validating PRs against acceptance criteria
- **Consistency Checking**: See [references/consistency-patterns.md](references/consistency-patterns.md) for verifying code follows existing patterns
- **Test-Driven Validation**: See [references/test-validation.md](references/test-validation.md) for empirically validating suspected bugs

## Prerequisites
- Issue tracking system available (see: ../agent-issue-tracking/SKILL.md for agent-based tracking)
- Test environment accessible
- Access to codebase and related documentation

## Process

### 1. Initialize Review Tracking
Create a main review tracking item with:
- Description of what's being reviewed
- Link to external ticket/PR
- Relevant tags for searchability

**Example structure:**
- **Title:** "Review PR #123 - Add user authentication"
- **Type:** Task
- **Priority:** High
- **Description:** "Thorough review of authentication implementation"
- **Tags:** review, security, authentication
- **External Reference:** Link to PR or ticket

See [Agent Issue Tracking](../agent-issue-tracking/SKILL.md) for agent-based tracking, or use your project's issue tracking system.

### 2. Understand Scope
- Read all changed files completely
- Identify the feature/fix being implemented
- Review associated tickets/requirements
- Note any external dependencies or patterns

For detailed scope validation against acceptance criteria, see [references/scope-validation.md](references/scope-validation.md).

### 3. Create Review Categories
Create separate tracking items for each concern area:
- One item per review category
- Link to main review item
- Clear description of scope

**Key principles:**
- Each category is independently workable (enables parallel work)
- Categories can be assigned to different reviewers
- Makes progress trackable

Common categories:
- Consistency (with existing patterns)
- Correctness (against requirements)
- Duplication (with existing functionality)
- Edge cases
- Test coverage
- Documentation
- Security/validation

See [Agent Issue Tracking](../agent-issue-tracking/SKILL.md) for implementation details on creating and linking tickets.

### 3.5. Check for Duplication

Before accepting new code, verify it doesn't duplicate existing functionality:

1. Search for similar functionality in the codebase
2. Check if existing code already provides the needed capability
3. Consider if existing code can be extended or reused
4. Evaluate if differences justify separate implementations

**Questions to ask:**
- Does existing code already do this?
- Can we reuse or extend existing code instead?
- Is there a significant difference that justifies duplication?
- Would combining implementations reduce maintenance burden?
- Are there subtle requirements that make separate implementations necessary?

**When duplication is acceptable:**
- Significant performance difference (measured, not assumed)
- Different error handling or transaction requirements
- Different contexts with incompatible constraints
- Temporary during refactoring (with plan to consolidate)

**When to consolidate:**
- New code is subset of existing functionality
- Differences are negligible or can be parameterized
- Maintenance burden of duplication is high
- Risk of logic drift between implementations

### 4. Validate Findings with Tests and Analysis
**CRITICAL: Validate concerns empirically before creating tickets**

**For suspected bugs:**
1. Write a test that demonstrates the bug
2. Run the test
3. If test PASSES → Not a bug (framework handles it)
4. If test FAILS → Real bug, create ticket with test

See [references/test-validation.md](references/test-validation.md) for detailed test-driven validation process.

**For architectural concerns:**
1. Search codebase for similar patterns
2. Compare implementations side-by-side
3. Evaluate tradeoffs (performance, maintainability, clarity)
4. Propose alternatives with concrete examples
5. Ask clarifying questions rather than making assumptions

See [references/consistency-patterns.md](references/consistency-patterns.md) for detailed consistency checking process.

**For duplication concerns:**
1. Identify what data both implementations fetch/compute
2. Check if one is a subset of the other
3. Measure or estimate performance impact of consolidation
4. Recommend refactoring with code examples
5. Consider if duplication is justified by requirements

**For naming/organization concerns:**
1. Examine existing file/module naming patterns
2. Identify inconsistencies with concrete examples
3. Propose alternatives that align with existing patterns
4. Consider impact on discoverability and maintainability

### 5. Document Findings
**Create separate tickets for each finding** - don't bundle multiple issues into one ticket.

```bash
# Create individual tickets for each finding
tk create "Add CSRF protection to POST endpoint" \
  -t bug -p 1 --parent [category-id] \
  -d "Endpoint lacks CSRF token validation..."

tk create "Extract business logic to service class" \
  -t task -p 3 --parent [category-id] \
  -d "Controller has business logic that should be in service..."
```

**Why separate tickets:**
- Each issue can be fixed independently
- Clear progress tracking
- Can be prioritized separately
- Can be assigned to different people
- Easier to close/invalidate individually

**Each ticket should include:**
- Brief, specific description
- Type (bug/task/chore)
- Priority level
- Detailed description with examples
- Link to parent category ticket
- Relevant tags

Priority guidelines:
- P1: Blocks merge, security, data integrity
- P2: Should fix before merge, user-facing bugs
- P3: Nice to have, refactoring, minor improvements
- P4: Polish, documentation tweaks

### 6. Add Context to Items
For each finding, add structured notes:
- **Context**: Why this matters
- **Evidence**: Code references, test results
- **Recommendation**: How to fix

### 7. Close Review Categories
As you complete each review category:
- Add summary of findings
- Mark as complete

### 8. Generate Summary
Create a summary item containing:
- Count of issues by status (fixed/invalid/outstanding)
- List of fixed issues with IDs
- List of invalid issues with reasons
- Table of outstanding issues with priorities
- Overall recommendation (merge/fix-first/needs-discussion)

## Anti-Patterns to Avoid

### ❌ Don't: Assume validation gaps without testing
```
BAD: Creating issue based on assumption
"Add validation for whitespace-only strings"
```

### ✅ Do: Write test first, then decide
```ruby
# GOOD: Test first
it 'rejects whitespace-only input' do
  expect(message).not_to be_valid
end
# Run test → if passes, no bug exists
```

### ❌ Don't: Create items without clear action items
```
BAD: Vague item
"Review security"
```

### ✅ Do: Create specific, actionable items
```
GOOD: Specific issue
"Add CSRF protection to POST /api/users endpoint"
Description: "Endpoint lacks CSRF token validation. Add protection."
```

### ❌ Don't: Mix multiple concerns in one item
```
BAD: Multiple unrelated issues
"Fix validation, update docs, refactor controller"
```

### ✅ Do: One issue per item
```
GOOD: Separate items
- "Fix input validation to strip whitespace"
- "Update documentation timestamps"
- "Extract business logic to service class"
```

## Success Criteria
- All review categories completed
- Each finding validated (especially bugs)
- Clear priorities assigned
- Actionable items created
- Summary document generated
- Recommendation provided

## Output Format
Provide user with:
1. Count of issues by status (fixed/invalid/outstanding)
2. Table of outstanding issues with priorities
3. Clear recommendation (merge/fix-first/needs-discussion)
4. Reference to summary for details

## Tool Integration
This skill works with any issue tracking system. Examples:
- **tk utility**: See [agent-issue-tracking](../agent-issue-tracking/SKILL.md) for agent-based tracking with tk
- **GitHub Issues**: Create issues via gh CLI or API
- **JIRA**: Create tickets via API or CLI tools
- **Linear**: Create issues via API
- **Simple markdown files**: Track in TODO.md or REVIEW.md

The process is tool-agnostic; adapt the tracking commands to your system.
