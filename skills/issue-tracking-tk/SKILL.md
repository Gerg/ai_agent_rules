---
name: issue-tracking-tk
description: Track issues, findings, and tasks during code reviews or development work using the tk utility. Use when conducting code reviews, managing multiple parallel tasks, tracking findings that need action, coordinating work with other agents, or generating work summaries.
---

# Issue Tracking with tk

Track issues, findings, and tasks during code reviews or development work using the `tk` utility.

## Prerequisites
- `tk` utility available in PATH
- `.tickets` directory initialized (or will be created on first use)

## Basic Operations

### Initialize Tracking
```bash
# Create a main tracking ticket
tk create "[Task description]" \
  -t task \
  -p [1-4] \
  --external-ref [JIRA-123] \
  -d "[Detailed description]" \
  --tags [tag1,tag2]

# Start working on it
tk start [ticket-id]
```

### Create Issue Tickets
```bash
# Create specific issue tickets
tk create "[Issue description]" \
  -t [bug|feature|task|epic|chore] \
  -p [1-4] \
  --external-ref [JIRA-123] \
  -d "[Detailed description]" \
  --tags [relevant,tags]
```

**Priority Guidelines:**
- P1: Critical, blocks progress
- P2: Important, should address soon
- P3: Nice to have
- P4: Polish, low priority

**Type Guidelines:**
- `bug`: Something broken or incorrect
- `feature`: New functionality
- `task`: Work item, investigation
- `chore`: Maintenance, refactoring
- `epic`: Large work item with sub-tasks

### Add Context and Notes
```bash
# Add notes to tickets
tk add-note [ticket-id] "
## Context
[Why this matters]

## Evidence
[Supporting information]

## Recommendation
[What to do]
"
```

### Track Progress
```bash
# List all open tickets
tk list --status=open

# List by status
tk list --status=closed

# Show ticket details
tk show [ticket-id]

# Update status
tk start [ticket-id]      # Set to in_progress
tk close [ticket-id]      # Set to closed
```

### Mark Issues as Fixed/Invalid
```bash
# Document resolution
tk add-note [ticket-id] "FIXED: [explanation]"
tk close [ticket-id]

# Or mark as invalid
tk add-note [ticket-id] "INVALID: [reason why]"
tk close [ticket-id]
```

### Generate Summary
```bash
# Create summary ticket
tk create "Summary - [what was done]" \
  -t task \
  -p 1 \
  --external-ref [JIRA-123] \
  -d "Comprehensive summary"

# Add summary notes
tk add-note [summary-id] "
## Completed: [count]
[List with ticket IDs]

## Invalid: [count]
[List with reasons]

## Outstanding: [count]
[List with priorities]
"

tk close [summary-id]
```

## Ticket Decomposition

### When to Split Tickets

**Split into multiple tickets when:**
- Work involves multiple independent files or components
- Analysis reveals multiple distinct issues
- Work can be parallelized
- Task is complex with multiple steps
- Want to track progress granularly

**Keep in single ticket when:**
- Changes are tightly coupled (must be done together)
- Task is simple and quick
- Splitting adds more overhead than value

### Creating Parent/Child Relationships

```bash
# Create parent ticket for overall work
parent_id=$(tk create "Fix HTTP status code details" -t task -p 1)

# Create child tickets that belong to parent
tk create "Review api-story-writing.md for issues" \
  -t task -p 2 --parent $parent_id

tk create "Remove 409 Conflict references" \
  -t task -p 2 --parent $parent_id

tk create "Update README descriptions" \
  -t task -p 3 --parent $parent_id
```

**Benefits:**
- Clear hierarchy and organization
- Can work on children in parallel
- Track progress (3/5 children complete)
- Parent closes when all children done

### Using Dependencies

```bash
# Create analysis ticket
analysis_id=$(tk create "Analyze skills for consistency" -t task -p 1)

# Create implementation tickets that depend on analysis
impl_id=$(tk create "Fix category labels" -t task -p 2)
tk dep $impl_id $analysis_id  # impl_id depends on (blocked by) analysis_id

# Work on analysis first
tk start $analysis_id
# ... do analysis, create more tickets ...
tk close $analysis_id

# Now implementation is unblocked
tk start $impl_id
```

**Use dependencies for:**
- Decisions that block implementation
- Analysis that must complete before fixes
- Sequential work that can't be parallelized

### Pattern: Analysis Creates Tickets

```bash
# 1. Create analysis ticket
analysis_id=$(tk create "Review CF skills for Tanzu references" -t task -p 1)
tk start $analysis_id

# 2. Do analysis, discover issues

# 3. Create tickets for each finding
tk create "Remove Tanzu from api-story-writing.md" \
  -t task -p 2 --parent $analysis_id

tk create "Remove Tanzu from cli-story-writing.md" \
  -t task -p 2 --parent $analysis_id

tk create "Update README to remove Tanzu" \
  -t task -p 3 --parent $analysis_id

# 4. Close analysis, work on findings
tk close $analysis_id

# 5. Work through child tickets
# Can be done in parallel or by different agents
```

**This pattern:**
- Makes work visible and trackable
- Enables parallelization
- Creates clear audit trail
- Allows prioritization of findings

### Checking Dependencies

```bash
# See what's ready to work on (no unresolved deps)
tk ready

# See what's blocked
tk blocked

# View dependency tree
tk dep tree [ticket-id]
```

## Workflow Patterns

### Review Workflow
```bash
# 1. Create main review ticket
tk create "Review PR #123" -t task -p 1 --external-ref JIRA-456

# 2. Create category tickets
tk create "Consistency Review" -t task -p 2 --external-ref JIRA-456
tk create "Test Coverage Review" -t task -p 2 --external-ref JIRA-456

# 3. Work through each category
tk start [category-id]
# ... do review work ...
tk add-note [category-id] "[findings]"
tk close [category-id]

# 4. Create issue tickets for findings
tk create "[Specific issue]" -t bug -p 2 --external-ref JIRA-456

# 5. Generate summary
tk create "Review Summary" -t task -p 1 --external-ref JIRA-456
tk add-note [summary-id] "[comprehensive summary]"
tk close [summary-id]
```

### Multi-Agent Coordination
```bash
# Check what's being worked on
tk list --status=in_progress

# Avoid collisions by checking before starting
tk show [ticket-id]  # See if someone else is working on it
tk start [ticket-id]  # Claim the work

# Leave notes for other agents
tk add-note [ticket-id] "Note for other agents: [information]"
```

## Query and Reporting

### List Tickets
```bash
# All open tickets
tk list --status=open

# By assignee
tk list -a "Agent Name"

# By tags
tk list -T review,bug

# Recently closed
tk closed --limit=20
```

### Export Data
```bash
# Export as JSON for processing
tk query

# Filter with jq
tk query | jq '.[] | select(.status == "open")'
```

## Best Practices

### Ticket Naming
```bash
# ✅ Good: Specific and actionable
tk create "Add whitespace validation for origin field"

# ❌ Bad: Vague
tk create "Fix validation"
```

### Descriptions
```bash
# ✅ Good: Clear description with context
tk create "Issue" -d "
Current: origin.downcase == 'uaa'
Problem: Doesn't strip whitespace
Fix: origin.strip.downcase == 'uaa'
"

# ❌ Bad: No description
tk create "Issue"
```

### Tags
```bash
# ✅ Good: Relevant, searchable tags
--tags validation,security,user-groups

# ❌ Bad: Too generic or too many
--tags stuff,things,code,review,misc,todo
```

### Notes
```bash
# ✅ Good: Structured notes with headers
tk add-note [id] "
## Finding
[What was found]

## Impact
[Why it matters]

## Resolution
[What was done]
"

# ❌ Bad: Stream of consciousness
tk add-note [id] "found a thing, not sure if it matters, maybe fix later"
```

## Integration Points

### With Code Review
Track findings systematically:
```bash
# Create tickets for each finding
# Link to external issue tracker
# Generate summary report
```

### With Testing
Track test results:
```bash
tk add-note [ticket-id] "
Test written: [test description]
Result: PASS/FAIL
Conclusion: [what this means]
"
```

### With Documentation
Track documentation needs:
```bash
tk create "Update docs for [feature]" \
  -t task \
  -p 3 \
  --tags documentation
```

## Output Formats

### Summary Table
Generate markdown tables from tickets:
```
| Ticket | Priority | Status | Description |
|--------|----------|--------|-------------|
| ccn-123 | P2 | open | Fix validation |
| ccn-456 | P3 | closed | Update docs |
```

### Status Report
```
## Issues Fixed: 4
- ccn-q4ps: Removed redundant line
- ccn-pmlk: Removed premature feature
- ccn-1io5: Created action class
- ccn-7qou: Added test coverage

## Issues Invalid: 2
- ccn-hcwg: Framework handles this
- ccn-u9pp: Requirements clarified

## Outstanding: 4
- ccn-53wp (P2): Whitespace validation
- ccn-w2w6 (P2): Experimental label
- ccn-m9c5 (P3): Documentation wording
- ccn-1s9h (P4): Example timestamps
```

## Troubleshooting

### Ticket Not Found
```bash
# Use partial ID matching
tk show ccn-5  # Matches ccn-53wp if unique

# List all to find it
tk list
```

### Ambiguous ID
```bash
# Use more characters
tk show ccn-53  # Instead of ccn-5
```

### Initialize Directory
```bash
# If .tickets doesn't exist
mkdir -p .tickets
tk list  # Will now work
```

## Success Criteria
- All work tracked in tickets
- Clear status on all items
- Comprehensive notes on findings
- Summary generated
- Easy to understand current state
