# AI Agent Skills

A modular collection of composable skills for AI agents, covering story writing, code review, issue tracking, and skill development.

> **Note:** These skills were created with AI assistance and are provided as starting points. Review and adapt them to your specific context, team conventions, and project requirements. Examples use generic patterns and may need customization for your codebase.

## Standards and Licensing

These skills follow the [Agent Skills](https://agentskills.io/) standard, an open format originally developed by Anthropic and implemented by Cursor, Claude, and other tools. Each skill is a directory containing a `SKILL.md` file with YAML frontmatter.

**License**: Skills without their own LICENSE file are covered by the repository's MIT license (see top-level LICENSE file). The [Skill Writing Skill](skill-writing/SKILL.md) incorporates content from [Anthropic's skill-creator](https://github.com/anthropics/skills/blob/main/skills/skill-creator/SKILL.md) and is licensed under Apache-2.0 (see `skill-writing/LICENSE`).

## Overview

These skills are designed to be **composable** - use them together or separately depending on your context. Each skill builds on or complements others to provide comprehensive guidance.

## Skill Categories

- **Story Writing Skills** - Writing user stories for Cloud Foundry
- **PR Review Skills** - Conducting systematic code reviews
- **Issue Tracking Skills** - Managing work with issue tracking tools
- **Meta Skills** - Creating and maintaining skills themselves

---

## Story Writing Skills

### [Generic Story Writing](story-writing-generic/SKILL.md)
**Foundation skill** - Universal principles for writing user stories

- Core principles and best practices
- Story structure and acceptance criteria
- Given/When/Then format
- Story splitting strategies
- Common mistakes to avoid
- Quality checklist

**Use when**: Writing any user story, regardless of domain or tracker

---

### [Jira Story Writing](story-writing-jira/SKILL.md)
**Tool skill** - Jira-specific markup and conventions

- Complete Jira markup reference
- Headers, formatting, code blocks, tables
- Jira-specific story structure
- Ticket references and linking
- Panels, quotes, and special formatting

**Use when**: Writing stories in Jira

**Combines with**: Generic Story Writing + Cloud Foundry skills

---

## Cloud Foundry Skills

### [Cloud Foundry API Story Writing](story-writing-cloud-foundry-api/SKILL.md)
**Domain skill** - Comprehensive CAPI story writing

- Story structure for API endpoints
- CRUD patterns (Create, Read, Update, Delete, List)
- CAPI error format (CF-* error titles)
- CAPI pagination structure
- Cloud Foundry roles and permissions
- Request/response patterns
- Query parameters and filtering
- Async operations (job pattern)
- Metadata stories
- References to CAPI v3 style guide (authoritative source)

**Use when**: Writing stories about Cloud Foundry API (CAPI) endpoints

**Combines with**: Generic Story Writing + Jira Story Writing

---

### [Cloud Foundry CLI Story Writing](story-writing-cloud-foundry-cli/SKILL.md)
**Domain skill** - Comprehensive CF CLI story writing

- CLI story principles
- Command execution patterns
- CF CLI command structure
- Cloud Foundry roles (SpaceDeveloper, OrgManager, SpaceSupporter, etc.)
- CF CLI output formats
- Help text structure
- Error handling patterns
- Confirmation prompts
- Exit codes
- CF CLI style guide conventions

**Use when**: Writing stories about Cloud Foundry CLI (cf CLI) commands

**Combines with**: Generic Story Writing + Jira Story Writing

---

## PR Review Skills

### [PR Review: Core](pr-review-core/SKILL.md)
**Foundation skill** - Systematic code review process

- Initialize review tracking
- Understand scope and requirements
- Create review categories
- Check for duplication
- Validate findings with tests
- Document findings systematically
- Generate review summaries

**Use when**: Conducting any code review or PR review

---

### [PR Review: Scope Validation](pr-review-scope-validation/SKILL.md)
**Add-on skill** - Validate PR scope against ticket requirements

- Extract ticket requirements
- Map implementation to acceptance criteria
- Identify scope boundaries
- Categorize completeness (Complete/Partial/Incomplete/Mismatch)
- Document scope assessment
- Clarify with stakeholders

**Use when**: Reviewing PRs with detailed acceptance criteria or as part of larger epics

**Combines with**: PR Review: Core

---

### [PR Review: Consistency Check](pr-review-consistency-check/SKILL.md)
**Add-on skill** - Verify code follows existing patterns

- Identify component types
- Find similar existing components
- Compare patterns and conventions
- Document deviations
- Check architectural alignment
- Generate consistency report

**Use when**: Reviewing new features or checking architectural alignment

**Combines with**: PR Review: Core

**Note**: Includes polyglot examples (Ruby, Go, Python, JavaScript)

---

### [PR Review: Test-Driven Validation](pr-review-test-validation/SKILL.md)
**Add-on skill** - Validate suspected bugs empirically through tests

- Identify suspected issues
- Write failing tests
- Run tests to validate
- Interpret results (pass/fail/inconclusive)
- Create tickets only for confirmed bugs

**Use when**: Validating suspected bugs or edge cases during review

**Combines with**: PR Review: Core

**Note**: Includes polyglot examples (Ruby/RSpec, Go, Python/pytest, JavaScript/Jest)

---

## Issue Tracking Skills

### [Issue Tracking with tk](issue-tracking-tk/SKILL.md)
**Tool skill** - Track issues and tasks using the tk utility

- Initialize tracking
- Create and manage tickets
- Add context and notes
- Track progress
- Generate summaries
- Multi-agent coordination

**Use when**: Managing code reviews, development tasks, or coordinating with other agents

**Combines with**: All PR Review skills

---

## Meta Skills

### [Skill Writing Skill](skill-writing/SKILL.md)
**Meta skill** - Create effective, composable skill documents

- Skill design principles (including token efficiency and degrees of freedom)
- YAML frontmatter and file structure
- Structure and content guidelines
- Composability patterns
- Progressive disclosure for large skills
- Quality checklists
- Skill types and organization
- Agent collaboration guidance
- Maintenance guidelines

**Use when**: Creating new skills or updating existing ones

**License**: Apache-2.0 (incorporates content from Anthropic's skill-creator)

---

## How to Use These Skills

### Story Writing

### For a Cloud Foundry API Story in Jira
Combine:
1. [Generic Story Writing](story-writing-generic/SKILL.md) - Core structure
2. [Jira Story Writing](story-writing-jira/SKILL.md) - Markup and formatting
3. [Cloud Foundry API Story Writing](story-writing-cloud-foundry-api/SKILL.md) - Complete CAPI guidance

### For a Cloud Foundry CLI Story in Jira
Combine:
1. [Generic Story Writing](story-writing-generic/SKILL.md) - Core structure
2. [Jira Story Writing](story-writing-jira/SKILL.md) - Markup and formatting
3. [Cloud Foundry CLI Story Writing](story-writing-cloud-foundry-cli/SKILL.md) - Complete CF CLI guidance

### For a Feature Touching Both CAPI and CF CLI
Combine:
1. [Generic Story Writing](story-writing-generic/SKILL.md) - Core structure
2. [Jira Story Writing](story-writing-jira/SKILL.md) - Markup and formatting
3. [Cloud Foundry API Story Writing](story-writing-cloud-foundry-api/SKILL.md) - For API acceptance criteria
4. [Cloud Foundry CLI Story Writing](story-writing-cloud-foundry-cli/SKILL.md) - For CLI acceptance criteria

### For a Simple Task in Any Tracker
Use:
1. [Generic Story Writing](story-writing-generic/SKILL.md) - Core structure
2. (Adapt formatting for your specific tracker)

### PR Review

### For a Standard PR Review
Combine:
1. [PR Review: Core](pr-review-core/SKILL.md) - Review process
2. [Issue Tracking with tk](issue-tracking-tk/SKILL.md) - Track findings
3. [PR Review: Test-Driven Validation](pr-review-test-validation/SKILL.md) - Validate bugs

### For a PR with Detailed Acceptance Criteria
Combine:
1. [PR Review: Core](pr-review-core/SKILL.md) - Review process
2. [PR Review: Scope Validation](pr-review-scope-validation/SKILL.md) - Validate scope
3. [Issue Tracking with tk](issue-tracking-tk/SKILL.md) - Track findings

### For a PR Adding New Features
Combine:
1. [PR Review: Core](pr-review-core/SKILL.md) - Review process
2. [PR Review: Scope Validation](pr-review-scope-validation/SKILL.md) - Check scope
3. [PR Review: Consistency Check](pr-review-consistency-check/SKILL.md) - Check patterns
4. [Issue Tracking with tk](issue-tracking-tk/SKILL.md) - Track findings

---

## Skill Composition Matrix

### Story Writing (Cloud Foundry)

| Context | Generic | Jira | CF API | CF CLI |
|---------|---------|------|--------|--------|
| Jira CAPI Story | ✅ | ✅ | ✅ | ❌ |
| Jira CF CLI Story | ✅ | ✅ | ❌ | ✅ |
| Jira CAPI+CLI Story | ✅ | ✅ | ✅ | ✅ |
| Simple Story | ✅ | ✅ | ❌ | ❌ |

### PR Review

| Context | Core | Scope | Consistency | Test Val | tk |
|---------|------|-------|-------------|----------|-----|
| Standard PR | ✅ | ❌ | ❌ | ✅ | ✅ |
| PR with AC | ✅ | ✅ | ❌ | ✅ | ✅ |
| New Feature | ✅ | ✅ | ✅ | ✅ | ✅ |
| Bug Fix | ✅ | ❌ | ❌ | ✅ | ✅ |
| Refactoring | ✅ | ❌ | ✅ | ✅ | ✅ |

---

## Adding New Skills

### System-Specific Skills
Create skills for specific products or platforms:
- `story-writing-cloud-foundry/` - CF-specific patterns
- `story-writing-kubernetes/` - K8s-specific patterns
- `story-writing-aws/` - AWS-specific patterns

### Domain-Specific Skills
Create skills for other domains:
- `story-writing-ui/` - Web/mobile UI patterns
- `story-writing-database/` - Database schema/migration patterns
- `story-writing-infrastructure/` - IaC and deployment patterns

### Issue Tracker Skills
Create skills for other trackers:
- `story-writing-github/` - GitHub Issues/Projects
- `story-writing-asana/` - Asana tasks
- `story-writing-linear/` - Linear issues

**Note**: Each skill should be a directory containing `SKILL.md` with YAML frontmatter, following the [Agent Skills standard](https://agentskills.io/).

---

## Principles for Creating New Skills

1. **Keep skills focused** - One domain or concern per skill
2. **Make them composable** - Skills should work together
3. **Avoid duplication** - Reference other skills instead of repeating
4. **Provide examples** - Show, don't just tell
5. **Be prescriptive** - Give clear guidance, not just options
6. **Stay generic** - Avoid project-specific details
7. **Link between skills** - Show how they combine

---

## Quick Start

### Story Writing

**I'm writing a Cloud Foundry API story in Jira**
1. Read [Generic Story Writing](story-writing-generic/SKILL.md) for structure
2. Read [Jira Story Writing](story-writing-jira/SKILL.md) for markup
3. Read [Cloud Foundry API Story Writing](story-writing-cloud-foundry-api/SKILL.md) for complete CAPI guidance

**I'm writing a Cloud Foundry CLI story in Jira**
1. Read [Generic Story Writing](story-writing-generic/SKILL.md) for structure
2. Read [Jira Story Writing](story-writing-jira/SKILL.md) for markup
3. Read [Cloud Foundry CLI Story Writing](story-writing-cloud-foundry-cli/SKILL.md) for complete CF CLI guidance

**I'm new to story writing**
1. Start with [Generic Story Writing](story-writing-generic/SKILL.md)
2. Learn your issue tracker's formatting (e.g., [Jira Story Writing](story-writing-jira/SKILL.md))
3. Add system-specific skills as needed (e.g., Cloud Foundry)

### PR Review

**I'm reviewing a pull request**
1. Read [PR Review: Core](pr-review-core/SKILL.md) for the process
2. Use [Issue Tracking with tk](issue-tracking-tk/SKILL.md) to track findings
3. Add [PR Review: Scope Validation](pr-review-scope-validation/SKILL.md) if PR has detailed acceptance criteria
4. Add [PR Review: Consistency Check](pr-review-consistency-check/SKILL.md) for new features
5. Use [PR Review: Test-Driven Validation](pr-review-test-validation/SKILL.md) to validate bugs

**I'm new to code review**
1. Start with [PR Review: Core](pr-review-core/SKILL.md)
2. Learn [Issue Tracking with tk](issue-tracking-tk/SKILL.md) for tracking
3. Add specialized skills as needed

### Creating Skills

**I'm creating a new skill**
1. Read [Skill Writing Skill](skill-writing/SKILL.md)
2. Follow the structure and principles
3. Update this README when done

---

## Contributing

When adding or updating skills:
- Keep the modular structure
- Update this README with new skills
- Show how skills compose together
- Provide clear examples
- Test with real stories
