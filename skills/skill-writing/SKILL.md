---
name: skill-writing
description: Meta-skill for creating effective skill documents using progressive disclosure. Use when creating new skills, updating existing skills, reviewing skill quality and structure, organizing skills with references, or when users request help with skill design, organization, or best practices.
license: Apache-2.0 (see LICENSE file)
---

# Skill Writing Skill

A meta-skill for creating effective skill documents using progressive disclosure.

## What is a Skill?

A **skill** is a reusable knowledge document that provides structured guidance on how to accomplish a specific type of task. Skills are designed to be:

- **Focused** - Cover one concern or outcome
- **Extensible** - Support variants through references and optional extensions
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
Each skill should focus on one concern or outcome. Use progressive disclosure (references/) for variants.

✅ Good:
- `story-writing/` - Story writing with references for different trackers/domains
- `pr-review/` - PR review with references for different validation types
- `deployment/` - Deployment with references for different platforms

❌ Bad:
- `development/` - Too broad, covers everything
- `jira-api-story-writing-cloud-foundry-cli/` - Multiple concerns in one skill

### 4. When to Create Separate Skills vs Use References

**When to create separate skills:**

Create separate skills for **orthogonal concerns** - fundamentally different types of work or outcomes:

✅ Separate skills:
- `story-writing/` vs `pr-review/` - Different activities
- `issue-tracking-tk/` vs `issue-tracking-jira/` - Different outcomes (AI agent coordination vs human team tracking)
- `deployment/` vs `deployment-rollback/` - Different outcomes (deploy vs undo)

**When to use references:**

Use `references/` subdirectories for **variants within a concern** - different contexts or domains for the same type of work:

✅ Use references:
- `story-writing/references/jira-markup.md` - Tracker-specific formatting
- `story-writing/references/cloud-foundry-api.md` - Domain-specific patterns
- `pr-review/references/ruby-patterns.md` - Language-specific conventions

**When to use extensions:**

Extensions are rare - most enhancements should be references. Only create extensions when the enhancement is substantial and optional.

Consider extensions when:
- The enhancement is a complete, separable capability (not just a variant)
- The enhancement is substantial (hundreds of lines)
- Most users won't need the enhancement
- The enhancement requires its own skill activation

**Example (if truly substantial):**
```
pr-review/ (core process with references for language patterns)
    +
pr-review-security-audit/ (optional, substantial security-focused extension)
    =
Enhanced PR review with security audit
```

**Most cases should use references instead:**
- Scope validation → `pr-review/references/scope-validation.md`
- Consistency checking → `pr-review/references/consistency-patterns.md`
- Language-specific patterns → `pr-review/references/ruby.md`

### 5. Clear Prerequisites and Context
State what other skills are required and guide users to relevant references.

**For core skills:**
```markdown
**Context**: This skill covers story writing across all trackers and domains.
For tracker-specific formatting, see the references/ directory.
For domain-specific patterns, see the appropriate reference file.
```

**For extension skills:**
```markdown
**Prerequisites**: This skill extends [PR Review](../pr-review/SKILL.md).
Use this extension when validating PR scope against detailed acceptance criteria.
```

### 6. Avoid Duplication
Use references for domain-specific content instead of creating separate skills or repeating content.

✅ Good:
```markdown
## Domain-Specific Patterns

For domain-specific story patterns:
- **Cloud Foundry API**: See [references/cloud-foundry-api.md](references/cloud-foundry-api.md)
- **Kubernetes**: See [references/kubernetes.md](references/kubernetes.md)
- **AWS**: See [references/aws.md](references/aws.md)
```

❌ Bad:
```markdown
## Cloud Foundry API Patterns
[Repeating all CAPI patterns in main SKILL.md]

## Kubernetes Patterns
[Repeating all K8s patterns in main SKILL.md]
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

#### 2. Context and Navigation (if applicable)

**For core skills with references:**
```markdown
**Context**: This skill covers [activity] across all [trackers/domains/frameworks].
For [tracker/domain/framework]-specific guidance, see the references/ directory.
```

**For extension skills:**
```markdown
**Prerequisites**: This skill extends [Base Skill](../base-skill/SKILL.md).
Use this extension when [specific scenario].
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
How this skill works with extensions and references.

**For core skills with references:**
```markdown
## Using This Skill

This skill provides universal principles for [activity].

**Tracker-specific guidance:**
- **Jira**: See [references/jira-markup.md](references/jira-markup.md)
- **GitHub**: See [references/github-markdown.md](references/github-markdown.md)

**Domain-specific patterns:**
- **Cloud Foundry**: See [references/cloud-foundry.md](references/cloud-foundry.md)
- **Kubernetes**: See [references/kubernetes.md](references/kubernetes.md)
```

**For extension skills:**
```markdown
## Integration

**Prerequisites**: This skill extends [Base Skill](../base-skill/SKILL.md).

Use this extension when [specific scenario requiring the extension].
```

### Optional Sections

- **Quick Reference** - Cheat sheet or summary
- **Workflows** - Multi-step processes with checklists (see Workflows and Checklists section)
- **Templates** - Output format templates (see Template Patterns section)
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

**Progressive disclosure is the primary pattern for managing skill complexity.** Skills use a three-level loading system to manage context efficiently:

1. **Metadata (name + description)** - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<500 lines recommended)
3. **Reference files** - As needed by agent (loaded on demand)

**When to use references:**
- SKILL.md approaching 500 lines
- Multiple trackers/tools supported (Jira, GitHub, Linear)
- Multiple domains/platforms (Cloud Foundry, Kubernetes, AWS)
- Multiple frameworks/variants (RSpec, Jest, pytest)
- Detailed reference material (markup guides, API patterns)
- Conditional features (advanced topics, troubleshooting)

**Splitting patterns:**

**Pattern 1: Tracker/tool-specific details**
```
story-writing/
├── SKILL.md (core principles + navigation)
└── references/
    ├── jira-markup.md
    ├── github-markdown.md
    └── linear-formatting.md
```

When user works in Jira, agent reads only jira-markup.md.

**Pattern 2: Domain/platform-specific patterns**
```
story-writing/
├── SKILL.md (universal principles + navigation)
└── references/
    ├── cloud-foundry-api.md
    ├── kubernetes.md
    └── aws.md
```

When user asks about Cloud Foundry, agent reads only cloud-foundry-api.md.

**Pattern 3: Framework/language variants**
```
testing/
├── SKILL.md (testing principles + navigation)
└── references/
    ├── rspec.md
    ├── jest.md
    └── pytest.md
```

When user works with Ruby, agent reads only rspec.md.

**Pattern 4: Conditional details**
```markdown
# SKILL.md

## Basic Usage
[Core instructions]

## Advanced Features
For advanced features, see:
- **Feature X**: See [references/feature-x.md](references/feature-x.md)
- **Feature Y**: See [references/feature-y.md](references/feature-y.md)
```

Agent loads reference files only when needed.

**Guidelines:**
- Keep SKILL.md focused on universal principles and navigation
- Put tracker/domain/framework-specific content in references/
- Keep references one level deep (link directly from SKILL.md)
- For files >100 lines, include table of contents at top
- Reference files clearly from SKILL.md with context about when to read them
- Prefer references over creating separate skills for variants

### Workflows and Checklists

For complex multi-step processes, provide a workflow with a copy-paste checklist that agents can track.

**When to use workflows:**
- Process has 3+ sequential steps
- Steps must be completed in order
- Easy to skip critical steps
- Progress tracking is valuable

**Pattern:**
```markdown
## [Process Name] Workflow

Copy this checklist and track your progress:

```
Task Progress:
- [ ] Step 1: [First action]
- [ ] Step 2: [Second action]
- [ ] Step 3: [Third action]
- [ ] Step 4: [Fourth action]
- [ ] Step 5: [Final verification]
```

**Step 1: [First action]**
[Detailed instructions for step 1]

**Step 2: [Second action]**
[Detailed instructions for step 2]

...
```

**Example (code-based workflow):**
```markdown
## PDF Form Filling Workflow

Copy this checklist and check off items as you complete them:

```
Task Progress:
- [ ] Step 1: Analyze the form (run analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run validate_fields.py)
- [ ] Step 4: Fill the form (run fill_form.py)
- [ ] Step 5: Verify output (run verify_output.py)
```

**Step 1: Analyze the form**
Run: `python scripts/analyze_form.py input.pdf`
This extracts form fields and their locations, saving to `fields.json`.

**Step 2: Create field mapping**
Edit `fields.json` to add values for each field.

**Step 3: Validate mapping**
Run: `python scripts/validate_fields.py fields.json`
Fix any validation errors before continuing.

**Step 4: Fill the form**
Run: `python scripts/fill_form.py input.pdf fields.json output.pdf`

**Step 5: Verify output**
Run: `python scripts/verify_output.py output.pdf`
If verification fails, return to Step 2.
```

**Example (analysis workflow):**
```markdown
## Research Synthesis Workflow

Copy this checklist and track your progress:

```
Research Progress:
- [ ] Step 1: Read all source documents
- [ ] Step 2: Identify key themes
- [ ] Step 3: Cross-reference claims
- [ ] Step 4: Create structured summary
- [ ] Step 5: Verify citations
```

**Step 1: Read all source documents**
Review each document in the `sources/` directory. Note the main arguments and supporting evidence.

**Step 2: Identify key themes**
Look for patterns across sources. What themes appear repeatedly? Where do sources agree or disagree?

**Step 3: Cross-reference claims**
For each major claim, verify it appears in the source material. Note which source supports each point.

**Step 4: Create structured summary**
Organize findings by theme. Include:
- Main claim
- Supporting evidence from sources
- Conflicting viewpoints (if any)

**Step 5: Verify citations**
Check that every claim references the correct source document. If citations are incomplete, return to Step 3.
```

### Feedback Loops

For tasks where quality depends on validation, implement a "validate → fix → repeat" pattern.

**When to use feedback loops:**
- Output quality is critical
- Validation can be automated or systematic
- Errors are common and fixable
- Multiple iterations improve results

**Pattern:**
```markdown
## [Process Name]

1. [Perform initial action]
2. **Validate immediately**: [validation command or checklist]
3. If validation fails:
   - Review the error message/issues
   - Fix the problems
   - Run validation again
4. **Only proceed when validation passes**
5. [Continue with next steps]
```

**Example (with validation script):**
```markdown
## Document Editing Process

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python scripts/validate.py unpacked_dir/`
3. If validation fails:
   - Review the error message carefully
   - Fix the issues in the XML
   - Run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python scripts/pack.py unpacked_dir/ output.docx`
6. Test the output document
```

**Example (with reference document):**
```markdown
## Content Review Process

1. Draft your content following the guidelines in STYLE_GUIDE.md
2. Review against the checklist:
   - Check terminology consistency
   - Verify examples follow the standard format
   - Confirm all required sections are present
3. If issues found:
   - Note each issue with specific section reference
   - Revise the content
   - Review the checklist again
4. Only proceed when all requirements are met
5. Finalize and save the document
```

**Key principle**: Make validation explicit and mandatory before proceeding. Use bold text like **"Only proceed when validation passes"** to emphasize critical checkpoints.

### Template Patterns

Provide templates for output format. Match the level of strictness to your needs.

**Strict templates** - Use when format is critical (APIs, data formats, required structure):

```markdown
## Report Structure

**ALWAYS use this exact template structure:**

```markdown
# [Analysis Title]

## Executive Summary
[One-paragraph overview of key findings]

## Key Findings
- Finding 1 with supporting data
- Finding 2 with supporting data
- Finding 3 with supporting data

## Recommendations
1. Specific actionable recommendation
2. Specific actionable recommendation
```

Do not deviate from this structure.
```

**Flexible templates** - Use when adaptation is valuable (content varies, context matters):

```markdown
## Report Structure

Here is a sensible default format, but use your best judgment based on the analysis:

```markdown
# [Analysis Title]

## Executive Summary
[Overview]

## Key Findings
[Adapt sections based on what you discover]

## Recommendations
[Tailor to the specific context]
```

Adjust sections as needed for the specific analysis type.
```

**When to be strict:**
- API responses or data formats
- Required compliance/regulatory formats
- Automated processing depends on structure
- Consistency is more important than flexibility

**When to be flexible:**
- Content varies significantly by context
- Agent judgment improves output
- Structure should adapt to findings
- Creativity and adaptation are valuable

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
   - Bloats the skill with unrelated content
   - Reduces clarity and focus
   
   ✅ Instead: Create focused, single-concern skills with references for variants
```

### Link Between Skills and References
Create clear navigation between skills and to reference materials.

**Linking to extensions (rare):**
```markdown
## Extensions

This skill can be enhanced with:
- **[Security Audit](../pr-review-security-audit/SKILL.md)** - Add comprehensive security-focused review
```

**Linking to references:**
```markdown
## Domain-Specific Patterns

For domain-specific guidance:
- **Cloud Foundry API**: [references/cloud-foundry-api.md](references/cloud-foundry-api.md)
- **Kubernetes**: [references/kubernetes.md](references/kubernetes.md)
```

**Linking to related skills:**
```markdown
## Related Skills

- **[Issue Tracking](../issue-tracking-tk/SKILL.md)** - Track findings during review
- **[Story Writing](../story-writing/SKILL.md)** - Write stories for identified issues
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

**Pattern**: `[concern]/`

Name skills by the type of work or outcome they address. Domains, tools, and variants go in references/.

**Examples:**
- `story-writing/` - Story writing (references for trackers/domains)
- `pr-review/` - PR review (references for languages/patterns)
- `deployment/` - Deployment (references for platforms/tools)
- `skill-writing/` - Creating skills

**Guidelines:**
- Name by the concern/activity/outcome
- Use lowercase with hyphens, not underscores
- Be descriptive but concise
- Directory name must match `name` field in SKILL.md frontmatter
- Avoid redundant suffixes (e.g., `skill-writing` not `skill-writing-skill`)

**Note**: If you have truly different outcomes (like `issue-tracking-tk/` for AI coordination vs `issue-tracking-jira/` for human team tracking), you may need more specific names to distinguish them. But most cases should use a single broad skill with references.

### README as Navigation Guide
The `README.md` should:
- List all available skills with brief descriptions
- Explain which skills have extensions
- Note which skills have extensive references/ directories
- Provide quick start guides
- Show how to use skills together (e.g., pr-review + issue-tracking-tk)

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

### Integration Quality
- [ ] Context clearly stated (for core skills)
- [ ] Prerequisites clearly stated (for extensions)
- [ ] References well-organized and linked
- [ ] Navigation to references provided
- [ ] Links to related skills (different concerns)

### Usability Quality
- [ ] Easy to find (good naming)
- [ ] Easy to understand (clear writing)
- [ ] Easy to apply (actionable guidance)
- [ ] Easy to reference (good structure)

## Reference Materials

**For detailed information:**
- **[Skill Template](references/TEMPLATE.md)** - Copy-paste template for new skills
- **[Skill Types](references/SKILL-TYPES.md)** - Core skills, extensions, and references
- **[Anti-Patterns](references/ANTI-PATTERNS.md)** - Common mistakes and how to avoid them

## Best Practices

1. **Start small** - Begin with core content, expand over time
2. **Get feedback** - Test with real users, iterate
3. **Keep it current** - Update as practices evolve
4. **Link generously** - Connect related skills
5. **Show examples** - Concrete beats abstract
6. **Be prescriptive** - Give clear guidance, not just options
7. **Stay focused** - One skill, one concern
8. **Use references** - Split variants into references/
9. **Document mistakes** - Learn from errors
10. **Maintain quality** - Regular reviews and updates

## Additional Resources

- **[Agent Skills Standard](https://agentskills.io/)** - Official specification for the Agent Skills format
- **[Agent Skills Specification](https://agentskills.io/specification)** - Complete format specification for SKILL.md files
- **[Anthropic's Example Skills](https://github.com/anthropics/skills)** - Browse example skills on GitHub

## Meta-Note

This skill itself follows the principles it describes:
- **Focused**: Only covers skill creation
- **Extensible**: Uses references/ for detailed guidance
- **Prescriptive**: Provides clear guidance
- **Generic**: No project-specific details
- **Reusable**: Applies to any skill creation
- **Conforms to standard**: Follows the [Agent Skills specification](https://agentskills.io/specification)

Use this skill as a template for creating new skills in your knowledge base.
