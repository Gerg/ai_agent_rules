# Skill Anti-Patterns

Common mistakes to avoid when creating skills.

## ❌ The Monolith
Putting everything in one massive skill.

**Problem:** Hard to navigate, not reusable, mixes concerns

**Solution:** Break into focused, composable skills

## ❌ The Duplicate
Repeating content from other skills.

**Problem:** Maintenance burden, inconsistency, confusion

**Solution:** Reference other skills, don't repeat

## ❌ The Abstract
Only providing principles without examples.

**Problem:** Hard to apply, unclear guidance

**Solution:** Include concrete examples throughout

## ❌ The Project-Specific
Including project or team-specific details.

**Problem:** Not reusable, requires customization

**Solution:** Keep generic, extract specifics to separate docs

### Detecting Codebase Leakage

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

## ❌ The Orphan
Creating a skill with no connections to others.

**Problem:** Hard to discover, unclear when to use

**Solution:** Link to related skills, update README

## ❌ The Kitchen Sink
Including tangentially related content.

**Problem:** Unfocused, hard to maintain

**Solution:** Stay focused, create separate skills for other topics
