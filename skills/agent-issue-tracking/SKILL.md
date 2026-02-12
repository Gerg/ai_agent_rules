---
name: agent-issue-tracking
description: Track issues, findings, and tasks during code reviews or development work using agent-friendly issue tracking tools. Use when conducting code reviews, managing multiple parallel tasks, tracking findings that need action, coordinating work with other agents, or generating work summaries.
---

# Agent Issue Tracking

Track issues, findings, and tasks using lightweight, agent-friendly issue tracking tools for coordination and local work management.

## Using This Skill

This skill provides universal principles for agent-based issue tracking. For tool-specific commands, see:

**Tool-specific references:**
- **tk**: See [references/tk.md](references/tk.md) for tk utility commands and syntax

Default to using tk, if no other tool is specified.

**Important**: Use only ONE tracking tool per session/project. If tk is being used, ignore other tracking systems (like Cursor's built-in TODOs, GitHub Issues for local work, etc.). Mixing tools creates confusion and duplicate tracking overhead.

## Core Concepts

### Purpose
Agent-based issue tracking serves different needs than human-oriented systems (Jira, GitHub Issues):
- **AI agent coordination**: Track work across multiple agent sessions
- **Local work management**: Organize tasks without external dependencies
- **Review findings**: Systematically track code review issues
- **Work postponement**: Defer work to later sessions
- **Progress visibility**: Show what's done, in progress, or blocked

### Key Differences from Human Issue Trackers
| Aspect | Human Trackers (Jira, GitHub) | Agent Trackers (tk, beans, beads) |
|--------|-------------------------------|-----------------------------------|
| **Audience** | Team members, stakeholders | AI agents, local coordination |
| **Scope** | Project-wide, long-lived | Session-scoped, ephemeral |
| **Overhead** | Rich UI, workflows, notifications | Minimal CLI, fast operations |
| **Integration** | CI/CD, project management | Local filesystem, version control |
| **Persistence** | Database, cloud | Local files, git-tracked |

## Ticket Structure

### Essential Fields
- **ID**: Unique identifier (auto-generated)
- **Title**: Brief, specific description
- **Type**: bug, feature, task, epic, chore
- **Priority**: P1 (critical) through P4 (low)
- **Status**: open, in_progress, closed
- **Description**: Detailed context and requirements

### Optional Fields
- **Parent**: For hierarchical relationships
- **Dependencies**: For blocking relationships
- **Tags**: For categorization and filtering
- **Assignee**: For multi-agent coordination
- **External Reference**: Link to Jira, GitHub, etc.
- **Notes**: Structured findings and updates

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

Use hierarchical tickets for complex work:

```
parent-id: "Fix HTTP status code details"
├── child-1: "Review api-story-writing.md for issues"
├── child-2: "Remove 409 Conflict references"
└── child-3: "Update README descriptions"
```

**Benefits:**
- Clear hierarchy and organization
- Can work on children in parallel
- Track progress (3/5 children complete)
- Parent closes when all children done

### Using Dependencies

Use dependencies for sequential work:

```
analysis-id: "Analyze skills for consistency"
    ↓ (blocks)
impl-id: "Fix category labels"
```

**Use dependencies for:**
- Decisions that block implementation
- Analysis that must complete before fixes
- Sequential work that can't be parallelized

### Pattern: Analysis Creates Tickets

Common pattern for discovery work:

1. **Create analysis ticket** - High-level investigation
2. **Do analysis** - Discover specific issues
3. **Create child tickets** - One per finding
4. **Close analysis** - Mark discovery complete
5. **Work through children** - Can be parallel or sequential

**Benefits:**
- Makes work visible and trackable
- Enables parallelization
- Creates clear audit trail
- Allows prioritization of findings

## Workflow Patterns

### Review Workflow

1. **Create main review ticket** - Overall review task
2. **Create category tickets** - One per review concern
   - Consistency Review
   - Test Coverage Review
   - Security Review
   - etc.
3. **Work through categories** - Complete each independently
4. **Create issue tickets** - One per finding
5. **Generate summary** - Overall status and recommendations

### Multi-Agent Coordination

**Check before starting:**
- List in-progress tickets to avoid collisions
- Show ticket details to see who's working on it
- Start ticket to claim the work

**Leave notes for other agents:**
- Document findings and decisions
- Note blockers or dependencies
- Provide context for future work

### Work Postponement

**When postponing work:**
1. Create ticket with clear description
2. Add context about why it's deferred
3. Tag appropriately for later discovery
4. Link to related work or discussions
5. Set appropriate priority

**Benefits:**
- Don't lose track of deferred work
- Context preserved for later
- Can be picked up by any agent
- Clear audit trail of decisions

## Ticket Types

### Bug
Something broken or incorrect that needs fixing.

**Examples:**
- "Add whitespace validation for origin field"
- "Fix null pointer when user has no groups"
- "Correct HTTP status code in error response"

### Feature
New functionality or capability.

**Examples:**
- "Add pagination support to list endpoint"
- "Implement CSV export for reports"
- "Add dark mode toggle"

### Task
General work item, investigation, or analysis.

**Examples:**
- "Review skills for consistency"
- "Analyze performance bottleneck"
- "Document API authentication flow"

### Chore
Maintenance, refactoring, or cleanup work.

**Examples:**
- "Extract business logic to service class"
- "Update dependencies to latest versions"
- "Remove deprecated feature flags"

### Epic
Large work item with multiple sub-tasks.

**Examples:**
- "Implement user authentication system"
- "Migrate to new API version"
- "Consolidate skills to progressive disclosure"

## Priority Guidelines

- **P1**: Critical, blocks progress, security issues, data integrity
- **P2**: Important, should address soon, user-facing bugs
- **P3**: Nice to have, refactoring, minor improvements
- **P4**: Polish, documentation tweaks, low priority

## Tool Selection

**Use one tool consistently:**
- Choose tk, beans, beads, or another agent-friendly tool
- Stick with it for the entire session/project
- Ignore other tracking systems (Cursor TODOs, etc.) to avoid duplication

**When to use agent tracking vs human tracking:**
- **Agent tracking (tk, beans, beads)**: Local work, code reviews, agent coordination, ephemeral tasks
- **Human tracking (Jira, GitHub Issues)**: Project-wide work, team coordination, long-lived issues, stakeholder visibility

**Don't mix tools for the same work** - creates confusion and duplicate effort.

## Best Practices

### Ticket Naming
✅ **Good**: Specific and actionable
- "Add whitespace validation for origin field"
- "Extract business logic to service class"
- "Update README to reflect new structure"

❌ **Bad**: Vague or generic
- "Fix validation"
- "Refactor code"
- "Update docs"

### Descriptions
✅ **Good**: Clear description with context
```
Current: origin.downcase == 'uaa'
Problem: Doesn't strip whitespace
Fix: origin.strip.downcase == 'uaa'
```

❌ **Bad**: No description or minimal context
```
Fix the thing
```

### Tags
✅ **Good**: Relevant, searchable tags
- `validation,security,user-groups`
- `refactoring,performance`
- `documentation,api`

❌ **Bad**: Too generic or too many
- `stuff,things,code,review,misc,todo`

### Notes
✅ **Good**: Structured notes with headers
```
## Finding
[What was found]

## Impact
[Why it matters]

## Resolution
[What was done]
```

❌ **Bad**: Stream of consciousness
```
found a thing, not sure if it matters, maybe fix later
```

## Integration Points

### With Code Review
- Track findings systematically
- Link to external issue tracker
- Generate summary report
- Document validation results

### With Testing
- Track test results
- Document test coverage gaps
- Link tests to tickets
- Note validation outcomes

### With Documentation
- Track documentation needs
- Link to relevant docs
- Note documentation gaps
- Track doc updates

## Output Formats

### Summary Table
Generate markdown tables from tickets:
```markdown
| Ticket | Priority | Status | Description |
|--------|----------|--------|-------------|
| abc-123 | P2 | open | Fix validation |
| abc-456 | P3 | closed | Update docs |
```

### Status Report
```markdown
## Issues Fixed: 4
- abc-123: Removed redundant line
- abc-456: Removed premature feature
- abc-789: Created action class
- abc-012: Added test coverage

## Issues Invalid: 2
- abc-345: Framework handles this
- abc-678: Requirements clarified

## Outstanding: 4
- abc-901 (P2): Whitespace validation
- abc-234 (P2): Experimental label
- abc-567 (P3): Documentation wording
- abc-890 (P4): Example timestamps
```

## Success Criteria
- All work tracked in tickets
- Clear status on all items
- Comprehensive notes on findings
- Summary generated when needed
- Easy to understand current state
- Minimal overhead for agents
