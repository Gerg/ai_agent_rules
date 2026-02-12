---
name: pr-review
description: Systematic code review process for pull requests. Use when conducting thorough PR reviews to validate implementation correctness, scope alignment, and code quality with empirical testing and structured issue tracking.
---

# PR Review

Conduct thorough, systematic code reviews using test-driven validation and issue tracking.

## Using This Skill

This skill provides the core PR review process. Load references as needed:

**When validating scope:**
- [references/scope-validation.md](references/scope-validation.md) - Validate PRs against ticket acceptance criteria

**When checking consistency:**
- [references/consistency-patterns.md](references/consistency-patterns.md) - Verify code follows existing patterns

**When validating suspected bugs:**
- [references/test-validation.md](references/test-validation.md) - Empirically validate issues through tests

**When consolidating findings:**
- [references/deduplicating-findings.md](references/deduplicating-findings.md) - Identify and consolidate duplicate tickets

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

**For detailed scope validation:** See [references/scope-validation.md](references/scope-validation.md) to validate implementation against acceptance criteria.

### 3. Create Review Categories
Create separate tracking items for each concern area:
- One item per review category
- Link to main review item
- Clear description of scope

**Key principles:**
- Each category is independently workable (enables parallel work)
- Categories can be assigned to different reviewers
- Makes progress trackable

**Common categories:**
- Consistency (with existing patterns)
- Correctness (against requirements)
- Duplication (with existing functionality)
- Edge cases
- Test coverage
- Documentation
- Security/validation

See [Agent Issue Tracking](../agent-issue-tracking/SKILL.md) for implementation details on creating and linking tickets.

### 4. Validate Findings Empirically
**CRITICAL: Validate concerns before creating tickets**

**For suspected bugs:**
Write a test first. If test passes → not a bug. If test fails → real bug, create ticket with test.

**For consistency/duplication/architectural concerns:**
Search codebase for similar patterns, compare implementations, evaluate tradeoffs with concrete examples.

**See references for detailed validation:**
- [references/test-validation.md](references/test-validation.md) - Test-driven bug validation
- [references/consistency-patterns.md](references/consistency-patterns.md) - Pattern comparison and architectural alignment

### 5. Document Findings
**Create separate tickets for each finding** - don't bundle multiple issues into one ticket.

**Why separate tickets:**
- Each issue can be fixed independently
- Clear progress tracking
- Can be prioritized separately
- Can be assigned to different people
- Easier to close/invalidate individually

**Each ticket should include:**
- Brief, specific title
- Type (bug/task/chore)
- Priority level
- Detailed description with examples
- Link to parent category ticket
- Relevant tags

**Priority guidelines:**
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

### 8. Consolidate Findings

**CRITICAL: Before generating summary, review all tickets for duplicates**

Round-based reviews can create duplicate tickets describing the same underlying issue. Consolidate before finalizing.

**Process:**
1. List all open tickets for the review
2. Group by code location (same file/function/line)
3. Identify root cause relationships
4. Consolidate duplicates or cross-reference related issues
5. Keep separate when issues require different code changes

**For detailed consolidation guidance:** See [references/deduplicating-findings.md](references/deduplicating-findings.md)

### 9. Generate Summary
Create a summary item containing:
- Count of issues by status (fixed/invalid/outstanding)
- Count after deduplication
- List of fixed issues with IDs
- List of invalid issues with reasons
- List of consolidated issues with references
- Table of outstanding issues with priorities
- Overall recommendation (merge/fix-first/needs-discussion)

## Anti-Patterns to Avoid

### ❌ Assuming bugs without testing
```
BAD: "Add validation for whitespace-only strings"
     (assumption without verification)

GOOD: Write test first → if passes, no bug exists
```

### ❌ Creating vague tickets
```
BAD: "Review security"

GOOD: "Add CSRF protection to POST /api/users endpoint"
```

### ❌ Bundling multiple concerns
```
BAD: "Fix validation, update docs, refactor controller"

GOOD: Create separate tickets for each concern
```

### ❌ Creating duplicate tickets
```
BAD: Three tickets for symptoms of one root cause

GOOD: One root cause ticket with notes about related impacts
```

## Success Criteria
- All review categories completed
- Each finding validated (especially bugs)
- Duplicates identified and consolidated
- Root cause relationships documented
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
