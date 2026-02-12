# Deduplication and Consolidation

Identify and consolidate duplicate findings before finalizing review.

## Why Deduplication Matters

Round-based reviews naturally examine code from different angles, which can create duplicate tickets describing the same underlying issue. Consolidating before finalizing prevents:
- Wasted effort fixing "different" issues that share a root cause
- Confusion about which ticket to work on
- Inflated issue counts that misrepresent review findings

## Process

### 1. List All Open Tickets
Gather all tickets created during the review.

### 2. Group by Code Location
Identify tickets affecting the same file, function, or line.

### 3. Identify Root Cause Relationships

Ask these questions:
- Does fixing ticket A automatically fix ticket B?
- Is ticket B a consequence of ticket A?
- Do these tickets describe the same code change?
- Are these symptoms of the same underlying issue?

### 4. Consolidate Duplicates

**For true duplicates:**
- Keep the root cause ticket
- Add notes to root cause explaining related issues
- Close duplicate tickets with reference to root cause
- Update root cause description if needed

**Example structure:**
```
Root cause ticket:
  Title: "Missing field in query response"
  Description: [original description]
  
  Note: "ROOT CAUSE ANALYSIS
  This missing field causes:
  - Inconsistent struct initialization (was ticket #X)
  - Potential panic in extraction at line 50 (was ticket #Y)
  - Broken dependent logic expecting this field (was ticket #Z)
  
  Fixing this resolves all related concerns."

Closed duplicate tickets:
  Each marked: "DUPLICATE: Consolidated into [root-cause-id]"
```

**For related but separate issues:**
- Keep both tickets
- Add cross-references explaining relationship
- Note if one should be fixed before the other

**Example:**
```
Ticket A: "Missing field in query response" (P2)
  Note: "Related to ticket B - that validation should be added 
        even after this field is present (defensive programming)"

Ticket B: "Add field format validation" (P3)
  Note: "Related to ticket A - implements defensive validation
        independent of root cause fix"
```

### 5. Keep Separate When

Issues should remain separate when:
- They require different code changes
- One is defensive programming even after root cause fixed
- Issues are in different subsystems
- Priorities differ significantly
- They can be worked on independently

## Common Duplicate Patterns

| Pattern | Action |
|---------|--------|
| Root cause + symptom | Consolidate into root cause |
| Root cause + consequence | Keep both if consequence needs separate defensive fix |
| Same issue, different framing | Consolidate |
| Related but independent | Keep separate with cross-references |
| Same location, different concerns | Keep separate |

## Examples

### Example 1: Clear Duplicates

**Before consolidation:**
- Ticket #1: "Missing validation in API handler"
- Ticket #2: "Inconsistent error handling in handler"  
- Ticket #3: "Potential null pointer in handler logic"

**Analysis:**
All three stem from missing validation at the handler entry point.

**After consolidation:**
- Ticket #1: "Missing validation in API handler" (P2)
  - Note: "ROOT CAUSE - Also causes inconsistent error handling and null pointer risk at line 42"
- Ticket #2: CLOSED - "DUPLICATE: Consolidated into #1"
- Ticket #3: CLOSED - "DUPLICATE: Consolidated into #1"

---

### Example 2: Root Cause + Defensive Programming

**Before consolidation:**
- Ticket #1: "Query returns incomplete data structure"
- Ticket #2: "Add nil checks before accessing query results"

**Analysis:**
Ticket #1 is root cause, but #2 is still valuable defensive programming.

**After consolidation:**
- Ticket #1: "Query returns incomplete data structure" (P2)
  - Note: "ROOT CAUSE - See also #2 for defensive validation"
- Ticket #2: "Add nil checks before accessing query results" (P3)
  - Note: "RELATED TO #1 - Defensive programming, should implement even after root cause fixed"

Both tickets remain open but relationship is clear.

---

### Example 3: Same Location, Different Concerns

**Before consolidation:**
- Ticket #1: "Missing CSRF protection on POST endpoint"
- Ticket #2: "Endpoint lacks rate limiting"
- Ticket #3: "No audit logging for sensitive operation"

**Analysis:**
All affect same endpoint but are independent security concerns.

**After consolidation:**
All three remain separate - they require different implementations and can be prioritized independently.

Optional: Add note to each: "Part of security hardening for /api/users endpoint (see also #X, #Y)"

## Anti-Patterns

### ❌ Over-consolidating independent issues

```
BAD: Combining "Missing CSRF token" and "Missing rate limiting" 
     because they're in the same file

GOOD: Keep separate - different security concerns requiring 
      different implementations
```

### ❌ Keeping duplicates "just in case"

```
BAD: "These seem related but I'm not sure, so I'll keep both"

GOOD: Analyze the relationship, make a decision, document it
```

### ❌ Consolidating without documentation

```
BAD: Closing tickets without explaining why or referencing root cause

GOOD: Add clear notes explaining consolidation and cross-references
```

## Success Criteria

- [ ] All tickets reviewed for duplication
- [ ] Root cause relationships identified
- [ ] Duplicates consolidated with clear notes
- [ ] Related tickets cross-referenced
- [ ] Final ticket count reflects unique issues
- [ ] Each remaining ticket represents independent work

## Integration with Core Review

Use this reference during step 8 of the core PR review process, before generating the final summary.
