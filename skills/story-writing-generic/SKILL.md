---
name: story-writing-generic
description: Universal guide for writing well-structured user stories, regardless of issue tracker or domain. Use when writing any user story to ensure proper structure, Given/When/Then format, and clear acceptance criteria.
---

# Generic Story Writing Skill


A universal guide for writing well-structured user stories, regardless of issue tracker or domain.

## Core Principles

1. **Stories should be small and atomic** - Focus on a single feature or capability
2. **Use Given/When/Then structure** - Makes acceptance criteria testable and clear
3. **Focus on external, user-facing behavior** - Implementation details belong in notes
4. **Reference prior art** - Build on existing patterns for consistency
5. **Be specific and concrete** - Use testable actions and observable outcomes
6. **Avoid coupling stories** - Each story should stand alone when possible
7. **Build incrementally** - Complex features emerge across multiple stories

## Story Structure

### Essential Sections
1. **Title** - Brief, descriptive summary
2. **Metadata** - Status, Type, Priority, Epic/Parent
3. **Summary** - One-line description
4. **Description** containing:
   - High-level context (optional)
   - Requirements or constraints (optional)
   - Acceptance Criteria
   - Implementation notes
   - Links to prior art and related work
5. **Labels/Tags** - For categorization and filtering

### Flexibility
- Simple stories may start with high-level Given/When/Then
- Complex stories may lead with requirements section
- Structure varies based on story complexity and team conventions

## Acceptance Criteria

### Scenario Naming
Use descriptive names that indicate what's being tested:
- **Base Case** - Happy path scenario
- **Already Exists** - Idempotency check
- **Invalid Input** - Validation error
- **Missing Required Field** - Specific error condition
- **Permissions** - Authorization scenario
- **Confirmation Required** - User confirmation flow

**Avoid**: Generic numbering like "Scenario 1", "Scenario 2"

### Given/When/Then Format
```
Given [preconditions and context]
And [additional preconditions]
When [action taken]
And [additional actions]
Then [expected outcome]
And [additional outcomes]
```

**Best Practices:**
- **Given**: Establish context and preconditions
  - Include permissions/authorization state
  - Specify relevant existing resources
  - Note configuration or feature flags
- **When**: Describe the action being tested
  - Be specific and concrete
  - Use active voice
  - Focus on user actions, not system internals
- **Then**: State observable outcomes
  - Describe what the user sees or experiences
  - Avoid internal state that can't be validated
  - Include success indicators or error messages

### Examples in Acceptance Criteria
- Keep examples minimal and focused
- Use placeholder notation for non-relevant fields
- Use consistent, simple placeholder data
- Show both success and error cases
- Demonstrate the feature, don't over-specify

## Content Guidelines

### Be Specific About Actions
✅ Good: "the user submits the form"
✅ Good: "the user runs the command"
✅ Good: "the system displays the result"
❌ Bad: "processing occurs"
❌ Bad: "the system does something"

### Focus on Observable Behavior
✅ Good: "The action succeeds"
✅ Good: "The user sees an error message"
✅ Good: "The resource is created"
❌ Bad: "System determines internal state"
❌ Bad: "Database is updated"
❌ Bad: "Service queries another service"

### Avoid Over-Specification
✅ Good: "Given a user with sufficient permissions"
✅ Good: "Given the resource exists"
❌ Bad: "Given a user with role X from source Y in group Z with attribute A"
❌ Bad: "Given the resource exists with field1=value1, field2=value2, field3=value3"

### Avoid Redundancy
- Each acceptance criterion should test distinct behavior
- Don't repeat scenarios that are variations of already-tested behavior
- Reference prior stories instead of re-testing their functionality
- Collapse similar scenarios when the logic is identical

## Prior Art References

### Purpose
- Build on existing patterns for consistency
- Avoid reinventing solutions
- Make dependencies explicit
- Provide context for reviewers

### What to Reference
- Similar features in the same system
- Related work that this builds upon
- Blocking dependencies
- Style guides and conventions
- Parent epics or initiatives

### How to Reference
- Place references in a dedicated section
- Be explicit about what pattern is being followed
- Link directly to the source
- Provide brief context for each reference

## Story Splitting

### When to Split
- Story covers multiple distinct features
- Validation scenarios are complex
- Automatic resource creation adds complexity
- Error handling is extensive
- Story is too large to complete in one iteration

### How to Split
- **Vertical slicing**: Complete end-to-end functionality for a subset
- **By scenario**: Separate happy path from error cases
- **By role**: Split by different user types or permissions
- **By phase**: Core functionality first, enhancements later

### Keep in Main Story
- Core happy path functionality
- Essential validation
- Basic error handling

### Move to Separate Stories
- Complex validation scenarios
- Automatic resource creation
- Advanced error handling
- Optional feature enhancements
- Performance optimizations

## Common Story Types

### Create Stories
- Base case with successful creation
- Already exists (idempotency)
- Invalid input
- Permissions/authorization
- Documentation updates

### Update Stories
- Base case with successful update
- Resource doesn't exist
- Invalid input
- Partial vs full updates
- Permissions/authorization

### Delete Stories
- Base case with successful deletion
- Resource doesn't exist
- Confirmation prompts (if applicable)
- Cascading deletes (if applicable)
- Permissions/authorization

### View/Read Stories
- Base case with successful retrieval
- Resource doesn't exist
- Permissions/authorization
- Filtering (if applicable)
- Pagination (if applicable)

### List/Search Stories
- Base case with results
- Empty results
- Filtering
- Sorting
- Pagination
- Permissions (who can search)
- Visibility (what they can see)

### Validation Stories
- Validate that new fields appear correctly
- Show both populated and empty states
- Focus on the new functionality being validated
- Note: "This is primarily a validation story"

## Implementation Notes

### What to Include
- Key implementation considerations
- Technical constraints or requirements
- Performance considerations
- Security considerations
- Backward compatibility notes
- Configuration requirements

### What to Avoid
- Detailed implementation steps (belongs in technical design)
- Code-level details (belongs in code comments)
- Obvious information
- Redundant information from acceptance criteria

## Process Guidelines

### Before Writing
1. Review existing stories for consistency
2. Identify prior art and patterns
3. Understand the user need
4. Clarify scope and boundaries
5. Identify dependencies

### While Writing
1. Start with the title and summary
2. Draft acceptance criteria first
3. Add examples to clarify
4. Include implementation notes
5. Link to prior art and dependencies
6. Add appropriate labels/tags

### After Writing
1. Review for clarity and completeness
2. Verify testability of acceptance criteria
3. Check consistency with prior art
4. Ensure all dependencies are noted
5. Get feedback from team

### Iteration
1. Stories evolve through discussion
2. Accept feedback gracefully
3. Refine based on implementation learnings
4. Update related stories as needed

## Common Mistakes to Avoid

1. ❌ Writing implementation steps instead of acceptance criteria
2. ❌ Using vague or generic scenario names
3. ❌ Forgetting to specify preconditions
4. ❌ Including internal system behavior in Then statements
5. ❌ Creating elaborate examples instead of simple ones
6. ❌ Forgetting to reference prior art
7. ❌ Extrapolating beyond what prior art shows
8. ❌ Adding scenarios that depend on unwritten stories
9. ❌ Coupling stories unnecessarily
10. ❌ Testing the same thing multiple ways
11. ❌ Writing stories that are too large
12. ❌ Omitting error cases
13. ❌ Forgetting about permissions/authorization
14. ❌ Not considering edge cases

## Quality Checklist

### Clarity
- [ ] Title clearly describes the feature
- [ ] Summary is concise and accurate
- [ ] Acceptance criteria are unambiguous
- [ ] Examples clarify rather than confuse

### Completeness
- [ ] All preconditions are stated
- [ ] Success criteria are clear
- [ ] Error cases are covered
- [ ] Permissions are specified
- [ ] Dependencies are noted

### Testability
- [ ] Each criterion is independently testable
- [ ] Outcomes are observable
- [ ] Success/failure is clearly defined
- [ ] Examples demonstrate the feature

### Consistency
- [ ] Follows team conventions
- [ ] Matches prior art patterns
- [ ] Uses consistent terminology
- [ ] Aligns with style guides

### Scope
- [ ] Story is appropriately sized
- [ ] Focus is clear and narrow
- [ ] Dependencies are minimal
- [ ] Can be completed in one iteration

## Adaptation Guidelines

This skill should be adapted based on:
- **Domain**: Web, mobile, API, CLI, database, infrastructure
- **Team conventions**: Specific formatting preferences
- **Project phase**: Early exploration vs mature implementation
- **Issue tracker**: Jira, GitHub, Asana, Linear, etc.
- **Methodology**: Scrum, Kanban, Scrumban, etc.

Always prioritize consistency with existing stories in your project over generic patterns.

## Integration with Other Skills

This generic skill provides the foundation. Combine with:
- **Issue Tracker Skills** (e.g., Jira, GitHub Issues) - For formatting and markup
- **Domain Skills** (e.g., API, CLI, UI) - For domain-specific patterns
- **System Skills** (e.g., specific product or platform) - For system-specific conventions

Use all applicable skills together to create well-formed, consistent stories for your specific context.
