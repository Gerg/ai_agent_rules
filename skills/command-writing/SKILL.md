---
name: command-writing
description: Guide for creating effective Cursor commands. Use when creating new commands, updating existing commands, reviewing command quality, or when users request help with command design, organization, or best practices. Commands are simple slash-invoked workflows distinct from skills.
---

# Command Writing Skill

A guide for creating effective Cursor commands - simple, reusable workflows triggered with the `/` prefix.

## What is a Command?

A **command** is a simple Markdown file that defines a reusable workflow. Commands are:

- **Explicit** - Only triggered when user types `/command-name`
- **Simple** - Plain Markdown files without YAML frontmatter
- **Focused** - Single, specific workflow or task
- **Parameterizable** - Accept additional context after the command name
- **Portable** - Can be project-level, user-level, or team-level

## Commands vs Skills

Understanding the distinction is critical:

| Aspect | Commands | Skills |
|--------|----------|--------|
| **Invocation** | Explicit via `/command-name` | Automatic (agent decides) or explicit |
| **Format** | Plain Markdown | Markdown with YAML frontmatter |
| **Structure** | Single `.md` file | Directory with `SKILL.md` + optional scripts/references |
| **Complexity** | Simple workflows | Complex, multi-faceted guidance |
| **Parameters** | Text after command name | Context-aware loading |
| **Use Case** | Specific, repeatable tasks | Domain knowledge and patterns |

**When to create a command:**
- Single, specific workflow (e.g., "create PR", "run tests")
- User wants explicit control over invocation
- Simple enough to fit in one Markdown file
- No need for scripts, references, or progressive disclosure

**When to create a skill:**
- Complex domain knowledge (e.g., "story writing", "PR review")
- Agent should automatically apply based on context
- Needs scripts, references, or multiple variants
- Requires progressive disclosure for efficiency

## Commands That Invoke Skills

**Key principle: Commands define workflow structure, skills contain domain logic.**

**Critical: Avoid tight coupling to skill internals.**

❌ Don't reference: skill sections, reference files, internal structure
✅ Do reference: capabilities, outcomes, what to accomplish

**Examples:**

✅ **Loose coupling:**
```markdown
Perform multiple focused rounds using the `pr-review` skill.
Default: 5 rounds, unless otherwise specified
```

❌ **Tight coupling:**
```markdown
Follow pr-review section 1-2 to initialize.
Use pr-review/references/consistency-patterns.md for consistency.
```

**Guidelines:**
1. Reference skills by capability, not internal structure
2. Define workflow structure (rounds, phases, sequence)
3. Specify defaults and allow customization
4. Include convergence criteria (when to stop)
5. Prompt for clarification rather than assume

## Command Design Principles

### 1. Single Purpose

Each command should do one thing well.

✅ Good: `/create-pr`, `/run-tests`, `/review-code`
❌ Bad: `/development` (too broad), `/create-pr-and-deploy` (multiple concerns)

### 2. Concise and Specific

**Agents are already capable.** Only include what they need for this specific workflow. Challenge each instruction: "Does the agent really need this explanation?"

Commands should be 50-200 lines. If longer, split into multiple commands or create a skill instead.

### 3. Appropriate Degrees of Freedom

Match specificity to the task:

- **High freedom**: Review tasks, analysis, recommendations
- **Medium freedom**: Workflows with some variation (e.g., PR creation with different contexts)
- **Low freedom**: Precise sequences (e.g., deployment steps, git operations)

### 4. Clear, Actionable Instructions

✅ Good:
```markdown
1. Review all changed files
2. Check for test coverage
3. Verify code follows style guide
4. Suggest improvements
```

❌ Bad: "Review the code and make it better."

### 5. Defaults and Flexibility

Provide sensible defaults while allowing customization.

✅ Good: "Default: 5 rounds, unless otherwise specified"
❌ Bad: "Perform exactly 5 rounds on the current PR"

This allows quick invocation while adapting to different contexts.

### 6. Prompting for User Input

Balance defaults with explicit user input. Prompt when:
- Scope is ambiguous or could vary significantly
- Criteria affect the outcome substantially
- User preferences are unknown
- Assumptions would be risky

✅ **Good balance:**
```markdown
- Default scope: Most recent commit, unless otherwise specified
- If uncertain about scope or criteria, prompt user for clarification
- Do not make unfounded assumptions
```

❌ **Too many prompts:**
```markdown
Ask user: Which files? How many rounds? What priority? What format?
```

❌ **Too few prompts:**
```markdown
Assume user wants comprehensive review of entire codebase.
```

**Guideline**: Provide defaults for routine decisions, prompt for significant decisions.

### 7. Parameterization

Commands accept parameters - text after the command name.

**Example:** `/commit and /pr these changes to address DX-523`

**In command:**
```markdown
If additional context is provided (like ticket numbers), incorporate it into the commit message.
```

## Command Structure

### File Organization

Commands are automatically discovered from these locations:

**Project commands:**
```
.cursor/
└── commands/
    ├── create-pr.md
    ├── run-tests.md
    └── review-code.md
```

**User commands (global):**
```
~/.cursor/
└── commands/
    ├── create-pr.md
    └── run-tests.md
```

**Team commands:**
Created via [Cursor Dashboard](https://cursor.com/dashboard?tab=team-content&section=commands) - automatically synced to all team members.

### Development Pattern

Like skills, commands can be developed in a repository and then copied/linked to discovery locations:

```bash
# Repository structure (for development/documentation)
commands/
├── code-review.md
├── create-pr.md
└── run-tests.md

# Activate by symlinking or copying to discovery location
ln -s /path/to/repo/commands ~/.cursor/commands
# or
cp -r /path/to/repo/commands/* ~/.cursor/commands/
```

This allows version control and sharing of commands while keeping them separate from active discovery locations.

### File Content

Commands are plain Markdown - no YAML frontmatter needed.

**Structure:**
```markdown
# [Command Name]

Brief description.

## Instructions
1. Step 1
2. Step 2

## Success Criteria
- [ ] Criterion 1
```

**Optional sections:** "When to Use", "Prerequisites"

## Content Guidelines

**Be specific and concise** - Concrete steps over verbose explanations.

✅ "Run `npm test`. If any fail: 1. Read error 2. Fix 3. Re-run"
❌ "Run tests and fix any issues."

**Use checklists** - For completeness and tracking.

**Provide context** - Explain why, not just what.

✅ "Review test coverage to prevent regressions"
❌ "Check test coverage"

**Handle edge cases** - Address common variations.

**Define convergence** - For iterative commands, specify when to stop.

**Prompt for clarity** - When uncertain, ask the user rather than assume.

## Common Command Patterns

### Workflow Commands
Multi-step processes with clear sequence.

```markdown
# Create Pull Request

1. Ensure all changes are committed
2. Push branch to remote
3. Review changes: `git diff main...HEAD`
4. Generate PR description (summary, test plan, related issues)
5. Create PR: `gh pr create --title "..." --body "..."`
```

### Review Commands
Analysis and feedback with checklists.

```markdown
# Code Review

## Review Checklist
- [ ] Code follows style guidelines
- [ ] Logic is clear and maintainable
- [ ] Tests cover new functionality
- [ ] No security vulnerabilities

Provide specific, actionable feedback for each issue.
```

### Fix Commands
Identify and resolve issues iteratively.

```markdown
# Run Tests and Fix

1. Run full test suite
2. For each failure: analyze error, identify root cause, fix, re-run
3. Continue until all tests pass
4. Report summary of fixes
```

### Setup Commands
Initialize or configure with parameters.

```markdown
# Setup New Feature

Create scaffolding for a new feature.

Usage: `/setup-new-feature user-authentication`

1. Create feature directory structure
2. Generate boilerplate files
3. Add basic tests
4. Create initial commit
```

## Quality Checklist

### Clarity
- [ ] Purpose is immediately clear
- [ ] Instructions are unambiguous
- [ ] Examples provided where helpful
- [ ] Success criteria defined

### Completeness
- [ ] All steps included
- [ ] Edge cases addressed
- [ ] Error handling covered
- [ ] Parameters explained

### Usability
- [ ] Easy to invoke (clear name)
- [ ] Easy to understand (clear writing)
- [ ] Easy to follow (logical flow)
- [ ] Easy to verify (clear outcomes)

### Maintainability
- [ ] Focused on one concern
- [ ] Not duplicating other commands
- [ ] Will remain relevant over time
- [ ] Easy to update

## Common Mistakes

**Too broad** - ❌ "Development tasks" ✅ "Run tests and fix failures"

**Too vague** - ❌ "Review and improve" ✅ "1. Check style 2. Identify bugs 3. Verify coverage"

**Missing context** - ❌ "Run tests" ✅ "Run tests to verify changes don't break functionality"

**Tight coupling to skills** - ❌ "Follow pr-review section 3.5" ✅ "Check for duplication"

**Rigid defaults** - ❌ "Exactly 5 rounds" ✅ "Default: 5 rounds, unless otherwise specified"

**Unfounded assumptions** - ❌ "Assume comprehensive review" ✅ "Prompt user for clarification"

**Should be a skill** - Convert when: >200 lines, needs variants/scripts, extensive domain knowledge, should auto-apply

## Example Commands

### Code Review (Skill-Invoking)

```markdown
# Code Review

Conduct multi-round review using `pr-review` and `agent-issue-tracking` skills.

## Instructions
- Default: 5 rounds, unless otherwise specified
- Default scope: Most recent commit, unless otherwise specified
- Rounds should converge on stable set of issues
- If uncertain about scope or criteria, prompt user

## Success Criteria
- [ ] Scope and criteria well understood
- [ ] All findings tracked and categorized
- [ ] Summary generated with recommendation
```

### Address PR Comments

```markdown
# Address GitHub PR Comments

1. Fetch PR comments: `gh pr view --comments`
2. For each comment: read, make changes, respond
3. Push updates and mark resolved
```

### Security Audit

```markdown
# Security Audit

## Checklist
- [ ] Authentication & authorization proper
- [ ] Inputs validated (SQL injection, XSS prevented)
- [ ] Sensitive data encrypted, no secrets in code
- [ ] No dependency vulnerabilities

List concerns with severity and remediation.
```

## Best Practices

1. **Start simple** - Begin with core workflow, refine over time
2. **Test thoroughly** - Try the command in real scenarios
3. **Get feedback** - Ask team members to use and improve
4. **Keep focused** - One command, one workflow
5. **Be specific** - Concrete steps, not vague guidance
6. **Handle errors** - Address what to do when things go wrong
7. **Use parameters** - Allow customization via additional text
8. **Provide defaults** - Sensible defaults with "unless otherwise specified"
9. **Loose coupling** - Reference skills by capability, not internal structure
10. **Define convergence** - Specify when iterative processes should stop
11. **Prompt for clarity** - Ask rather than assume when uncertain
12. **Document well** - Clear instructions and examples
13. **Review regularly** - Update as practices evolve
14. **Know when to upgrade** - Convert to skill when complexity grows

## Related Skills

- **[Skill Writing](../skill-writing/SKILL.md)** - When command complexity grows, convert to a skill
- **[Agent Session Retro](../agent-session-retro/SKILL.md)** - Review command effectiveness after sessions

## Additional Resources

- **[Cursor Commands Documentation](https://cursor.com/docs/context/commands)** - Official documentation
- **[Cursor Skills Documentation](https://cursor.com/docs/context/skills)** - When to use skills instead
- **[Team Commands Dashboard](https://cursor.com/dashboard?tab=team-content&section=commands)** - Create team-wide commands
