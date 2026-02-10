# Agent Collaboration on Skills

Guidance for AI agents working on skills with users.

## When to Prompt for Decisions

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

## How to Present Decision Options

1. **State the problem clearly** - What needs to be decided and why
2. **Present 2-4 concrete options** - Not just abstract choices
3. **List pros and cons** - Help user evaluate tradeoffs
4. **Provide recommendation** - Based on analysis, but defer to user
5. **Wait for user input** - Don't implement until confirmed

## Validating Decisions

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

## Making Work Visible with Ticket Decomposition

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

See [Issue Tracking with tk](../../issue-tracking-tk/SKILL.md) for implementation details.
