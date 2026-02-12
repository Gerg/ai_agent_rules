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

### 2. Verify Review Scope

**Confirm scope when ambiguous or surprising**

**When to verify:**
- User request is vague ("review this PR", "review current branch")
- Scope is surprisingly large (many commits, hundreds of files)
- Multiple commits with unclear boundaries
- Unclear what changed vs what already exists

**When verification is unnecessary:**
- User explicitly specifies single commit ("review commit abc123")
- User specifies exact commit range ("review commits abc123..def456")
- Scope is clear and small (1-2 commits, few files)

**Verification process:**

1. **Check what's in scope:**
   ```bash
   # List commits
   git log --oneline [base-branch]..HEAD
   
   # Show size of changes
   git show --stat [commit-sha]
   ```

2. **If scope is ambiguous or surprising, confirm with user:**
   
   Example (surprising size):
   > "I see 47 commits changing 203 files. Should I review all of these, or specific commits?"
   
   Example (unclear request):
   > "I see 2 commits on this branch. Should I review both, or just the latest?"

3. **Document scope in review tracking:**
   Record commits, base branch, file count, and change size

4. **Use appropriate git commands:**
   ```bash
   # For specific commits
   git show [commit-sha]
   
   # For commit range
   git diff [first-commit]^..[last-commit]
   
   # Avoid: git diff [base]...[head] (three dots - shows all branch differences)
   ```

### 3. Understand Scope
- Read all changed files completely (from verified commits only)
- Identify the feature/fix being implemented
- Review associated tickets/requirements
- Note any external dependencies or patterns

**For detailed scope validation:** See [references/scope-validation.md](references/scope-validation.md) to validate implementation against acceptance criteria.

### 4. Create Review Categories
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

### 5. Validate Findings Empirically
**CRITICAL: Validate concerns before creating tickets**

**For suspected bugs:**
Write a test first. If test passes → not a bug. If test fails → real bug, create ticket with test.

**For consistency/duplication/architectural concerns:**
Search codebase for similar patterns, compare implementations, evaluate tradeoffs with concrete examples.

**See references for detailed validation:**
- [references/test-validation.md](references/test-validation.md) - Test-driven bug validation
- [references/consistency-patterns.md](references/consistency-patterns.md) - Pattern comparison and architectural alignment

### 6. Document Findings
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

### 7. Add Context to Items
For each finding, add structured notes:
- **Context**: Why this matters
- **Evidence**: Code references, test results
- **Recommendation**: How to fix

### 8. Close Review Categories
As you complete each review category:
- Add summary of findings
- Mark as complete

### 9. Consolidate Findings

**CRITICAL: Before generating summary, review all tickets for duplicates**

Round-based reviews can create duplicate tickets describing the same underlying issue. Consolidate before finalizing.

**Process:**
1. List all open tickets for the review
2. Group by code location (same file/function/line)
3. Identify root cause relationships
4. Consolidate duplicates or cross-reference related issues
5. Keep separate when issues require different code changes

**For detailed consolidation guidance:** See [references/deduplicating-findings.md](references/deduplicating-findings.md)

### 10. Generate Summary
Create a summary item containing:
- Count of issues by status (fixed/invalid/outstanding)
- Count after deduplication
- List of fixed issues with IDs
- List of invalid issues with reasons
- List of consolidated issues with references
- Table of outstanding issues with priorities
- Overall recommendation (merge/fix-first/needs-discussion)

## Anti-Patterns to Avoid

### ❌ Reviewing ambiguous or surprising scope without verification
```
BAD: User says "review current branch", you immediately start reviewing
     without checking what's in scope
     - Could be 1 commit or 100 commits
     - Could be 5 files or 500 files
     - Might review existing code as if it's new
     Result: Review wrong code, waste time, irrelevant findings

GOOD: Check scope, verify if surprising or ambiguous
     1. List commits: git log --oneline [base]..HEAD
     2. If surprising (many commits) or ambiguous (unclear request):
        Confirm with user: "I see 47 commits changing 203 files. Review all?"
     3. If clear and small (1-2 commits explicitly requested):
        Proceed without verification
     4. Document scope in tracking
```

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
- Review scope verified when ambiguous or surprising
- Correct commits/range documented in review tracking
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
