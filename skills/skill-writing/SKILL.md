---
name: skill-writing
description: Meta-skill for creating effective, composable skill documents. Use when creating new skills, updating existing skills, reviewing skill quality and structure, or when users request help with skill design, organization, or best practices.
license: Apache-2.0 (see LICENSE file)
---

# Skill Writing Skill

A meta-skill for creating effective, composable skill documents.

## What is a Skill?

A **skill** is a reusable knowledge document that provides structured guidance on how to accomplish a specific type of task. Skills are designed to be:

- **Focused** - Cover one domain or concern
- **Composable** - Work together with other skills
- **Prescriptive** - Provide clear, actionable guidance
- **Generic** - Avoid project-specific details
- **Reusable** - Apply across multiple contexts
- **Concise** - Respect the shared context window

## Skill Design Principles

### 1. Concise is Key

**The context window is a shared resource.** Skills compete for context with system prompts, conversation history, other skills, and the user's actual request.

**Default assumption: Agents are already capable.** Only add context the agent doesn't already have. Challenge each piece of information:
- "Does the agent really need this explanation?"
- "Does this paragraph justify its token cost?"

**Prefer concise examples over verbose explanations.**

**Target: Keep SKILL.md under 500 lines.** When approaching this limit, split content into reference files.

### 2. Set Appropriate Degrees of Freedom

Match the level of specificity to the task's fragility and variability:

**High freedom (text-based instructions)**: Use when multiple approaches are valid, decisions depend on context, or heuristics guide the approach.

**Medium freedom (pseudocode or structured guidance)**: Use when a preferred pattern exists, some variation is acceptable, or configuration affects behavior.

**Low freedom (specific scripts, templates, few parameters)**: Use when operations are fragile and error-prone, consistency is critical, or a specific sequence must be followed.

Think of the agent as exploring a path: a narrow bridge with cliffs needs specific guardrails (low freedom), while an open field allows many routes (high freedom).

### 3. Single Responsibility
Each skill should focus on one domain, concern, or aspect of work.

✅ Good:
- `story-writing-cloud-foundry-api/` - CAPI-specific patterns
- `story-writing-jira/` - Jira markup and conventions
- `story-writing-cloud-foundry-cli/` - CF CLI-specific patterns

❌ Bad:
- `story-writing/` - Too broad, mixes everything
- `jira-api-story-writing-cloud-foundry-cli/` - Multiple concerns in one skill

### 4. Composability
Skills should work together. Design for combination, not isolation.

**Composition Models:**

There are two main models for skill composition, both valid:

#### Model A: Additive Composition (Story Writing)
Skills are peers that combine together. No hierarchy.

**Pattern:**
```
Foundation + Tracker + Domain = Complete skill set

story-writing-generic/ (foundation)
    +
story-writing-jira/ (tool)
    +
story-writing-cloud-foundry-api/ (domain)
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

pr-review-core/ (required foundation)
    +
pr-review-scope-validation/ (optional extension)
    +
pr-review-consistency-check/ (optional extension)
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

### 5. Clear Prerequisites
State what other skills or knowledge are required.

**Format:**
```markdown
**Prerequisites**: This skill builds on [Generic Story Writing](../story-writing-generic/SKILL.md). 
Combine with [Jira Story Writing](../story-writing-jira/SKILL.md) if using Jira.
```

### 6. Avoid Duplication
Reference other skills instead of repeating content.

✅ Good:
```markdown
For general story structure, see [Generic Story Writing](../story-writing-generic/SKILL.md).
This skill focuses on API-specific patterns.
```

❌ Bad:
```markdown
Stories should be small and atomic. Use Given/When/Then format...
[repeating content from generic skill]
```

### 7. Generic Over Specific
Keep skills applicable across contexts. Extract project-specific details.

✅ Good:
```markdown
Use consistent placeholder data throughout examples.
```

❌ Bad:
```markdown
Use potato-themed examples (e.g., "spud-managers", "potato-user").
```

### 8. Show, Don't Just Tell
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

### YAML Frontmatter

Skills should begin with YAML frontmatter containing metadata:

```markdown
---
name: skill-name
description: Brief description of what this skill does and when to use it. Used by agents to determine relevance.
license: MIT (optional)
compatibility: Requires Python 3.8+ (optional)
disable-model-invocation: false (optional, default false)
---
```

**Required Fields:**
- `name` - Skill identifier. Lowercase letters, numbers, and hyphens only. Must match the parent directory name.
- `description` - **Critical for skill triggering.** Describes both WHAT the skill does AND WHEN to use it. Include all triggering conditions here (not in the body, since the body loads only after triggering). Be comprehensive about use cases and contexts.

**Optional Fields:**
- `license` - License name or reference to a bundled license file
- `compatibility` - Environment requirements (system packages, network access, etc.)
- `metadata` - Arbitrary key-value mapping for additional metadata
- `disable-model-invocation` - When `true`, the skill is only included when explicitly invoked. When `false` (default), agents automatically apply it based on context.

### Essential Sections

#### 1. Title and Purpose
```markdown
# [Skill Name] Skill

Brief description of what this skill covers and when to use it.
```

#### 2. Prerequisites (if applicable)
```markdown
**Prerequisites**: This skill builds on [Other Skill](../other-skill/SKILL.md).
Use together with [Another Skill](../another-skill/SKILL.md) for complete coverage.
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
- **[Skill A](../skill-a/SKILL.md)** - When/why to combine
- **[Skill B](../skill-b/SKILL.md)** - When/why to combine

Together, these provide [complete coverage of what].
```

### Optional Sections

- **Quick Reference** - Cheat sheet or summary
- **Templates** - Copy-paste starting points
- **Checklist** - Quality or completeness checklist
- **Advanced Topics** - Deep dives for power users
- **Troubleshooting** - Common issues and solutions

### What NOT to Include

A skill should only contain essential information for agents to do the job. **Do NOT create auxiliary documentation files:**

❌ **Avoid:**
- README.md (separate from SKILL.md)
- INSTALLATION_GUIDE.md
- QUICK_REFERENCE.md
- CHANGELOG.md
- CONTRIBUTING.md
- User-facing documentation
- Setup and testing procedures
- Process documentation about creating the skill

✅ **Include only:**
- SKILL.md (the skill itself)
- scripts/ (executable code if needed)
- references/ (documentation loaded as needed)
- assets/ (files used in output)

The skill should contain only what an agent needs to execute the task, not auxiliary context about the skill's development or maintenance.

## Content Guidelines

### Progressive Disclosure

Skills use a three-level loading system to manage context efficiently:

1. **Metadata (name + description)** - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<500 lines recommended)
3. **Reference files** - As needed by agent (loaded on demand)

**When to split content:**
- SKILL.md approaching 500 lines
- Multiple frameworks/variants supported
- Domain-specific sections that aren't always needed
- Detailed reference material

**Splitting patterns:**

**Pattern 1: Framework/variant-specific details**
```
skill-name/
├── SKILL.md (core workflow + selection guidance)
└── references/
    ├── framework-a.md
    ├── framework-b.md
    └── framework-c.md
```

When user chooses framework A, agent reads only framework-a.md.

**Pattern 2: Domain-specific organization**
```
skill-name/
├── SKILL.md (overview + navigation)
└── references/
    ├── domain-1.md
    ├── domain-2.md
    └── domain-3.md
```

When user asks about domain 1, agent reads only domain-1.md.

**Pattern 3: Conditional details**
```markdown
# SKILL.md

## Basic Usage
[Core instructions]

## Advanced Features
For advanced features, see:
- **Feature X**: See [FEATURE-X.md](references/FEATURE-X.md)
- **Feature Y**: See [FEATURE-Y.md](references/FEATURE-Y.md)
```

Agent loads reference files only when needed.

**Guidelines:**
- Keep references one level deep (link directly from SKILL.md)
- For files >100 lines, include table of contents at top
- Reference files clearly from SKILL.md with context about when to read them

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
- [Related Skill](../related-skill/SKILL.md) - For related topic
- [Foundation Skill](../foundation/SKILL.md) - For prerequisites
- [Advanced Skill](../advanced/SKILL.md) - For deeper coverage
```

## Skill Organization

### File Structure

Each skill is a directory containing a `SKILL.md` file:

```
my-skill/
├── SKILL.md           # Main skill definition (required)
├── scripts/           # Optional: executable scripts
│   └── deploy.sh
├── references/        # Optional: additional documentation
│   └── REFERENCE.md
└── assets/            # Optional: templates, configs, etc.
    └── template.json
```

**Requirements:**
- Main skill file must be named `SKILL.md`
- `name` field in frontmatter must match the directory name
- Optional directories: `scripts/`, `references/`, `assets/`

### Naming Conventions

**Pattern**: `[domain]-[specificity]/`

Use domain-first naming so related skills group together in the filesystem.

**Examples:**
- `story-writing-generic/` - Foundation skill
- `story-writing-jira/` - Tool-specific skill
- `story-writing-cloud-foundry-api/` - System-specific skill
- `pr-review-core/` - Process foundation skill
- `pr-review-scope-validation/` - Process extension skill
- `issue-tracking-tk/` - Tool skill

**Guidelines:**
- Start with the domain (e.g., `story-writing-`, `pr-review-`, `issue-tracking-`)
- Add specificity from generic to specific (e.g., `cloud-foundry-api`)
- Use lowercase with hyphens, not underscores
- Be descriptive but concise
- Directory name must match `name` field in SKILL.md frontmatter
- Avoid redundant suffixes (e.g., `skill-writing` not `skill-writing-skill`)

### README as Composition Guide
The `README.md` should:
- List all available skills
- Explain how to combine them
- Provide a composition matrix
- Include quick start guides
- Show example combinations

## Agent Collaboration on Skills

When AI agents work on skills, they should collaborate effectively with users on key decisions.

**For detailed guidance on:**
- When to prompt for decisions vs. make autonomous decisions
- How to present decision options
- Ticket decomposition patterns

**See:** [Agent Collaboration Reference](references/AGENT-COLLABORATION.md)

## Creating a New Skill

**For step-by-step guidance on:**
- Identifying the need
- Defining scope
- Choosing structure
- Drafting content
- Quality review
- Maintenance

**See:** [Skill Creation Process](references/CREATION-PROCESS.md)

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

## Reference Materials

**For detailed information:**
- **[Skill Template](references/TEMPLATE.md)** - Copy-paste template for new skills
- **[Skill Types](references/SKILL-TYPES.md)** - Foundation, domain, tool, and process skills
- **[Anti-Patterns](references/ANTI-PATTERNS.md)** - Common mistakes and how to avoid them

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

## Additional Resources

- **[Agent Skills Standard](https://agentskills.io/)** - Official specification for the Agent Skills format
- **[Agent Skills Specification](https://agentskills.io/specification)** - Complete format specification for SKILL.md files
- **[Anthropic's Example Skills](https://github.com/anthropics/skills)** - Browse example skills on GitHub

## Meta-Note

This skill itself follows the principles it describes:
- **Focused**: Only covers skill creation
- **Composable**: Can be used with any domain
- **Prescriptive**: Provides clear guidance
- **Generic**: No project-specific details
- **Reusable**: Applies to any skill creation
- **Conforms to standard**: Follows the [Agent Skills specification](https://agentskills.io/specification)

Use this skill as a template for creating new skills in your knowledge base.
