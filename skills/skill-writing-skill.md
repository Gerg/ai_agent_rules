# Skill Writing Skill

**Category:** Meta skill

A meta-skill for creating effective, composable skill documents.

## What is a Skill?

A **skill** is a reusable knowledge document that provides structured guidance on how to accomplish a specific type of task. Skills are designed to be:

- **Focused** - Cover one domain or concern
- **Composable** - Work together with other skills
- **Prescriptive** - Provide clear, actionable guidance
- **Generic** - Avoid project-specific details
- **Reusable** - Apply across multiple contexts

## Skill Design Principles

### 1. Single Responsibility
Each skill should focus on one domain, concern, or aspect of work.

✅ Good:
- `story-writing/cloud-foundry/api-story-writing.md` - CAPI-specific patterns
- `story-writing/jira-story-writing.md` - Jira markup and conventions
- `story-writing/cloud-foundry/cli-story-writing.md` - CF CLI-specific patterns

❌ Bad:
- `story-writing.md` - Too broad, mixes everything
- `jira-api-cli-story-writing.md` - Multiple concerns in one skill

### 2. Composability
Skills should work together. Design for combination, not isolation.

**Composition Models:**

There are two main models for skill composition, both valid:

#### Model A: Additive Composition (Story Writing)
Skills are peers that combine together. No hierarchy.

**Pattern:**
```
Foundation + Tracker + Domain = Complete skill set

story-writing/generic-story-writing.md (foundation)
    +
story-writing/jira-story-writing.md (tool)
    +
story-writing/cloud-foundry/api-story-writing.md (domain)
    =
Complete Jira CAPI story writing
```

**Characteristics:**
- Skills are independent peers
- Combine any foundation + tracker + domain
- No skill depends on another
- Mix and match freely
- Example: Generic + GitHub + CLI

**Use when:**
- Multiple orthogonal concerns
- Users need different combinations
- Skills apply across contexts

#### Model B: Core + Extensions (PR Review)
One core skill with optional extensions that build on it.

**Pattern:**
```
Core + Extension(s) = Enhanced capability

pr-review-core.md (required foundation)
    +
pr-review-scope-validation.md (optional extension)
    +
pr-review-consistency-check.md (optional extension)
    =
Enhanced PR review
```

**Characteristics:**
- Core skill is required
- Extensions depend on core
- Extensions are optional
- Extensions can be used independently
- Example: Core + Scope + Consistency

**Use when:**
- Clear foundational process
- Extensions add specialized capabilities
- Not all users need all extensions
- Extensions share common foundation

#### Choosing a Model

**Use Additive (Model A) when:**
- Concerns are orthogonal (tracker vs domain)
- No clear hierarchy
- Users need different combinations
- Skills are equally important

**Use Core + Extensions (Model B) when:**
- Clear foundational process exists
- Extensions are specialized/optional
- Extensions build on core concepts
- Core is always needed

**Both models are valid** - choose based on your skill domain.

### 3. Clear Prerequisites
State what other skills or knowledge are required.

**Format:**
```markdown
**Prerequisites**: This skill builds on [Generic Story Writing](story-writing/generic-story-writing.md). 
Combine with [Jira Story Writing](story-writing/jira-story-writing.md) if using Jira.
```

### 4. Avoid Duplication
Reference other skills instead of repeating content.

✅ Good:
```markdown
For general story structure, see [Generic Story Writing](story-writing/generic-story-writing.md).
This skill focuses on API-specific patterns.
```

❌ Bad:
```markdown
Stories should be small and atomic. Use Given/When/Then format...
[repeating content from generic skill]
```

### 5. Generic Over Specific
Keep skills applicable across contexts. Extract project-specific details.

✅ Good:
```markdown
Use consistent placeholder data throughout examples.
```

❌ Bad:
```markdown
Use potato-themed examples (e.g., "spud-managers", "potato-user").
```

### 6. Show, Don't Just Tell
Provide concrete examples, not just abstract principles.

✅ Good:
```markdown
### Example Structure
```
Given a user has sufficient permissions
When POST to /resources with valid data
Then the request succeeds with status code 201
```
```

❌ Bad:
```markdown
Include preconditions, actions, and expected outcomes.
```

## Skill Structure

### Essential Sections

#### 1. Title and Purpose
```markdown
# [Skill Name] Skill

Brief description of what this skill covers and when to use it.
```

#### 2. Prerequisites (if applicable)
```markdown
**Prerequisites**: This skill builds on [Other Skill](other-skill.md).
Use together with [Another Skill](another-skill.md) for complete coverage.
```

#### 3. Core Principles or Overview
High-level concepts and philosophy.

```markdown
## Core Principles

1. **Principle 1** - Explanation
2. **Principle 2** - Explanation
3. **Principle 3** - Explanation
```

#### 4. Main Content
Organized by topic, pattern, or use case.

**Common Structures:**
- **By Pattern**: Common patterns and how to apply them
- **By Type**: Different types of scenarios
- **By Phase**: Sequential steps or stages
- **Reference**: Comprehensive reference material

#### 5. Examples
Concrete, realistic examples throughout.

```markdown
### Example: [Scenario Name]

[Setup/context]

[Example code/content]

[Explanation]
```

#### 6. Common Mistakes
What to avoid, with examples.

```markdown
## Common Mistakes

1. ❌ **Mistake description** - Why it's wrong
   ✅ **Correct approach** - How to do it right
```

#### 7. Integration Guidance
How this skill combines with others.

```markdown
## Integration with Other Skills

Use this skill with:
- **[Skill A](skill-a.md)** - When/why to combine
- **[Skill B](skill-b.md)** - When/why to combine

Together, these provide [complete coverage of what].
```

### Optional Sections

- **Quick Reference** - Cheat sheet or summary
- **Templates** - Copy-paste starting points
- **Checklist** - Quality or completeness checklist
- **Advanced Topics** - Deep dives for power users
- **Troubleshooting** - Common issues and solutions

## Content Guidelines

### Use Clear Headings
Organize with a clear hierarchy:
- `#` - Skill title
- `##` - Major sections
- `###` - Subsections
- `####` - Minor subsections

### Provide Context
Explain *why*, not just *what* or *how*.

✅ Good:
```markdown
Use descriptive scenario names instead of numbers because they make 
stories easier to scan and understand at a glance.
```

❌ Bad:
```markdown
Use descriptive scenario names.
```

### Use Consistent Formatting

**For Examples:**
```markdown
### Example: [Name]

[Setup]

```
[Code/content]
```

[Explanation]
```

**For Comparisons:**
```markdown
✅ Good: [example]
❌ Bad: [example]
```

**For Lists:**
- Use bullets for unordered items
- Use numbers for sequential steps
- Use checkboxes for checklists

### Include Anti-Patterns
Show what *not* to do, with explanations.

```markdown
### Common Mistakes

1. ❌ Mixing multiple concerns in one skill
   - Makes skills hard to reuse
   - Creates unnecessary dependencies
   - Reduces composability
   
   ✅ Instead: Create focused, single-purpose skills
```

### Link Between Skills
Create a web of related knowledge.

```markdown
See also:
- [Related Skill](related-skill.md) - For related topic
- [Foundation Skill](foundation.md) - For prerequisites
- [Advanced Skill](advanced.md) - For deeper coverage
```

## Skill Organization

### Directory Structure

**Pattern:** Group related skills into subdirectories by domain or function.

```
skills/
├── README.md                      # Overview and composition guide
├── [meta-skills].md               # Meta-skills (e.g., skill-writing-skill.md)
├── [tool-skills].md               # Tool-specific skills (e.g., issue-tracking-with-tk.md)
├── [domain-name]/                 # Domain-specific skills (e.g., story-writing/, pr-review/)
│   ├── [foundation-skill].md      # Foundation for this domain
│   ├── [tool-skill].md            # Tool skills for this domain
│   ├── [extension-skill].md       # Extension skills
│   └── [system-name]/             # System-specific subdirectory (e.g., cloud-foundry/)
│       └── [system-skill].md      # System-specific skills
```

**Key principles:**
- Group related skills in subdirectories
- Use clear, descriptive directory names
- System-specific skills go in nested subdirectories
- Keep root directory minimal (meta-skills, cross-cutting tools, README)
- See actual `skills/` directory for current structure

### Naming Conventions

**Pattern**: `[domain]-[topic]-[type].md`

**Examples:**
- `generic-story-writing.md` - Foundation
- `jira-story-writing.md` - Tool
- `cloud-foundry/api-story-writing.md` - System-specific domain
- `pr-review-core.md` - Process foundation

**Guidelines:**
- Use lowercase
- Use hyphens, not underscores
- Be descriptive but concise
- End with `.md`

### README as Composition Guide
The `README.md` should:
- List all available skills
- Explain how to combine them
- Provide a composition matrix
- Include quick start guides
- Show example combinations

## Agent Collaboration on Skills

When AI agents work on skills, they should collaborate effectively with users on key decisions.

### When to Prompt for Decisions

**Always prompt for:**
- **Architectural decisions** - Structure, organization, patterns that affect multiple skills
- **Breaking changes** - Changes that affect existing usage or workflows
- **Multiple valid options** - When there are tradeoffs between approaches
- **Scope changes** - Adding/removing significant functionality
- **Naming/categorization** - How skills are named and organized
- **User preferences** - Style, verbosity, example types

**Example - Architectural Decision:**
```
"I found coupling issues in PR review skills. Three options:

Option A: Language-specific skills (pr-review-ruby.md, pr-review-go.md)
- Pros: Clear, focused, independent evolution
- Cons: More files, potential duplication

Option B: Polyglot examples in single skill
- Pros: Fewer files, shows universal patterns
- Cons: Longer files, requires multiple language knowledge

Option C: Generic principles + example files
- Pros: Separates concepts from syntax
- Cons: More complex structure

Which approach do you prefer?"
```

**Autonomous decisions acceptable for:**
- **Implementation details** - How to structure a specific section
- **Minor improvements** - Fixing typos, improving clarity
- **Following established patterns** - Applying existing conventions
- **Bug fixes** - Correcting obvious errors
- **Adding examples** - When pattern is already established

### How to Present Decision Options

1. **State the problem clearly** - What needs to be decided and why
2. **Present 2-4 concrete options** - Not just abstract choices
3. **List pros and cons** - Help user evaluate tradeoffs
4. **Provide recommendation** - Based on analysis, but defer to user
5. **Wait for user input** - Don't implement until confirmed

### Validating Decisions

Even after making autonomous decisions:
- **Document what was decided** - In tickets or notes
- **Explain rationale** - Why this choice was made
- **Be ready to reverse** - If user disagrees
- **Learn from feedback** - Adjust future decision-making

**Example - Post-Decision Validation:**
```
"I created language-specific skills (Option A) based on:
- Follows existing story-writing pattern
- Clearer for users
- Independent evolution

If you prefer a different approach, I can refactor."
```

### Making Work Visible with Ticket Decomposition

**When working on skills, decompose work into trackable tickets:**

**Anti-pattern - Single Large Ticket:**
```
Ticket: "Fix status code issues in CF skills"
- Review 2 files
- Make 6 different changes
- Update README

Problem: Hard to track, can't parallelize, unclear progress
```

**Better - Decomposed Tickets:**
```
Parent: "Fix status code issues in CF skills"
├─ Child: "Review api-story-writing.md for status code issues"
├─ Child: "Review cli-story-writing.md for status code issues"
├─ Child: "Remove 409 Conflict from api-story-writing.md"
├─ Child: "Replace HTTP Status Codes section with style guide reference"
├─ Child: "Update CLI exit codes to reference style guide"
└─ Child: "Update README.md descriptions"

Benefits: Clear progress (3/6 done), can parallelize, trackable
```

**Pattern: Analysis → Child Tickets**
1. Create analysis/review ticket
2. Do analysis work
3. Create child tickets for each finding
4. Work through child tickets
5. Close parent when all children done

**When to decompose:**
- Multiple independent files/components
- Multiple distinct issues found
- Work can be parallelized
- Want granular progress tracking

**When to keep single ticket:**
- Changes are tightly coupled
- Task is simple and quick
- Splitting adds overhead

See [Issue Tracking with tk](issue-tracking-with-tk.md) for implementation details.

## Creating a New Skill

### 1. Identify the Need
Ask:
- What knowledge needs to be captured?
- Is this a foundation, domain, or system-specific skill?
- Does this overlap with existing skills?
- Will this be reused across contexts?

### 2. Define the Scope
Determine:
- What's included in this skill
- What's excluded (belongs in other skills)
- Prerequisites and dependencies
- Target audience

### 3. Choose a Structure
Select based on content:
- **Reference**: Comprehensive coverage (e.g., Jira markup)
- **Pattern-based**: Common patterns (e.g., API story types)
- **Sequential**: Step-by-step process
- **Conceptual**: Principles and philosophy

### 4. Draft the Content
Follow the structure:
1. Title and purpose
2. Prerequisites (if applicable)
3. Core principles
4. Main content (organized logically)
5. Examples throughout
6. Common mistakes
7. Integration guidance

### 5. Add Examples
For each major concept:
- Provide concrete examples
- Show both good and bad approaches
- Include realistic scenarios
- Keep examples focused

### 6. Review for Quality
Check:
- [ ] Single, clear purpose
- [ ] Composable with other skills
- [ ] Prerequisites stated
- [ ] Examples provided
- [ ] Common mistakes covered
- [ ] Integration guidance included
- [ ] No duplication with other skills
- [ ] Generic (not project-specific)
- [ ] Clear, actionable guidance

### 7. Update the README
Add the new skill to:
- Skills list
- Composition matrix
- Quick start guides
- Related skills sections

## Skill Maintenance

### When to Update
- New patterns emerge
- Common mistakes are discovered
- User feedback identifies gaps
- Related skills change
- Best practices evolve

### How to Update
1. Identify what needs to change
2. Check impact on dependent skills
3. Update content
4. Update examples
5. Review integration guidance
6. Update README if needed
7. Notify users of changes

### Versioning (Optional)
For major changes, consider:
- Dating updates in changelog
- Noting breaking changes
- Providing migration guidance
- Maintaining backward compatibility

## Quality Checklist

### Content Quality
- [ ] Clear, focused purpose
- [ ] Appropriate scope (not too broad or narrow)
- [ ] Concrete examples provided
- [ ] Anti-patterns documented
- [ ] Prescriptive guidance (not just options)
- [ ] Consistent terminology
- [ ] No unnecessary jargon

### Structure Quality
- [ ] Logical organization
- [ ] Clear heading hierarchy
- [ ] Consistent formatting
- [ ] Easy to scan
- [ ] Quick reference available (if needed)

### Composability Quality
- [ ] Prerequisites clearly stated
- [ ] No duplication with other skills
- [ ] Integration guidance provided
- [ ] Links to related skills
- [ ] Works well in combination

### Usability Quality
- [ ] Easy to find (good naming)
- [ ] Easy to understand (clear writing)
- [ ] Easy to apply (actionable guidance)
- [ ] Easy to reference (good structure)

## Example Skill Template

```markdown
# [Skill Name] Skill

Brief description of what this skill covers and when to use it.

**Prerequisites**: [If applicable, list prerequisite skills]

## Core Principles

1. **Principle 1** - Explanation
2. **Principle 2** - Explanation
3. **Principle 3** - Explanation

## [Main Section 1]

### [Subsection]

Content with examples.

✅ Good: [example]
❌ Bad: [example]

### [Another Subsection]

More content with examples.

## [Main Section 2]

### [Pattern or Type 1]

Description and example.

```
Example code or content
```

### [Pattern or Type 2]

Description and example.

## Common Mistakes

1. ❌ **Mistake** - Why it's wrong
   ✅ **Correct** - How to do it right

2. ❌ **Another mistake** - Why it's wrong
   ✅ **Correct** - How to do it right

## Quick Reference

[Optional: Summary or cheat sheet]

## Integration with Other Skills

Use this skill with:
- **[Skill A](skill-a.md)** - When/why
- **[Skill B](skill-b.md)** - When/why

Together, these provide [complete coverage].
```

## Skill Types

### Foundation Skills
Provide core concepts applicable everywhere.

**Characteristics:**
- Broad applicability
- No dependencies
- Referenced by many other skills
- Generic principles

**Example:** `generic-story-writing.md`

### Domain Skills
Cover specific domains or technologies.

**Characteristics:**
- Focused on one domain
- Build on foundation skills
- Composable with tool skills
- Domain-specific patterns
- Can be system-specific

**Examples:** `story-writing/cloud-foundry/api-story-writing.md`, `story-writing/cloud-foundry/cli-story-writing.md`

### Tool Skills
Cover specific tools or platforms.

**Characteristics:**
- Tool-specific markup or conventions
- Orthogonal to domain skills
- Composable with domain skills
- Technical reference

**Examples:** `story-writing/jira-story-writing.md`, `issue-tracking-with-tk.md`

### Process Skills
Cover systematic processes and workflows.

**Characteristics:**
- Process-oriented guidance
- Can be foundation or extension
- Composable with tool skills
- Step-by-step procedures

**Examples:** `pr-review/pr-review-core.md`, `pr-review/pr-review-consistency-check.md`

## Anti-Patterns

### ❌ The Monolith
Putting everything in one massive skill.

**Problem:** Hard to navigate, not reusable, mixes concerns

**Solution:** Break into focused, composable skills

### ❌ The Duplicate
Repeating content from other skills.

**Problem:** Maintenance burden, inconsistency, confusion

**Solution:** Reference other skills, don't repeat

### ❌ The Abstract
Only providing principles without examples.

**Problem:** Hard to apply, unclear guidance

**Solution:** Include concrete examples throughout

### ❌ The Project-Specific
Including project or team-specific details.

**Problem:** Not reusable, requires customization

**Solution:** Keep generic, extract specifics to separate docs

#### Detecting Codebase Leakage

Even skills intended to be generic can inadvertently reveal their origin through subtle details:

**Architectural terminology:**
```
❌ Specific framework layers: "Controllers/Models/Actions/Messages/Presenters"
✅ Generic patterns: "Identify and conform to existing layered architecture"
```

**Domain concepts:**
```
❌ Project-specific terms: "tenant/workspace visibility", "group-level permissions"
✅ Generic concepts: "resource visibility", "hierarchical permissions"
```

**API patterns:**
```
❌ Specific endpoints: "POST /v3/users", "ProjectAPI pagination format"
✅ Generic REST: "POST /api/resources", "pagination patterns"
```

**How to review for leakage:**
1. Search for proper nouns and acronyms
2. Look for framework-specific class names
3. Check for domain-specific terminology
4. Review examples for specific endpoint patterns
5. Verify architectural terms are generic

**When specificity is appropriate:**
- System-specific skills in dedicated subdirectories
- Tool-specific skills clearly labeled
- Title and category indicate the specific scope

### ❌ The Orphan
Creating a skill with no connections to others.

**Problem:** Hard to discover, unclear when to use

**Solution:** Link to related skills, update README

### ❌ The Kitchen Sink
Including tangentially related content.

**Problem:** Unfocused, hard to maintain

**Solution:** Stay focused, create separate skills for other topics

## Best Practices

1. **Start small** - Begin with core content, expand over time
2. **Get feedback** - Test with real users, iterate
3. **Keep it current** - Update as practices evolve
4. **Link generously** - Connect related skills
5. **Show examples** - Concrete beats abstract
6. **Be prescriptive** - Give clear guidance, not just options
7. **Stay focused** - One skill, one purpose
8. **Think composition** - Design for combination
9. **Document mistakes** - Learn from errors
10. **Maintain quality** - Regular reviews and updates

## Meta-Note

This skill itself follows the principles it describes:
- **Focused**: Only covers skill creation
- **Composable**: Can be used with any domain
- **Prescriptive**: Provides clear guidance
- **Generic**: No project-specific details
- **Reusable**: Applies to any skill creation

Use this skill as a template for creating new skills in your knowledge base.
