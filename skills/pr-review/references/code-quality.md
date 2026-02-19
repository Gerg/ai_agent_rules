# Code Quality

Evaluate code quality beyond functional correctness: clarity, maintainability, and adherence to project conventions.

## Table of Contents

- [Comment Quality](#comment-quality)
- [Naming Clarity](#naming-clarity)
- [Redundancy Detection](#redundancy-detection)
- [Project Conventions](#project-conventions)
- [Success Criteria](#success-criteria)

## Comment Quality

### Principle

Comments should explain **why**, not **what**. Code should be self-explanatory through clear naming and structure.

### Red Flags

**Comments that restate code:**
- Comment describes what the code obviously does
- Comment adds no information beyond reading the code
- Comment in conditional that just restates the condition

**Examples:**
```
❌ BAD: Comment restates code
// Set the value to x
value = x

// Loop through items
for item in items

// Check if empty
if list.isEmpty()

// Neither A nor B provided
else
```

**Outdated or misleading comments:**
- Comment doesn't match current code
- Comment describes old behavior
- Comment contradicts implementation

### Good Comments

**Explain non-obvious intent:**
- Why this approach was chosen over alternatives
- Trade-offs or constraints
- Performance considerations
- Business rules or domain logic

**Document invariants:**
- Assumptions that must hold
- Preconditions or postconditions
- Thread safety requirements

**Provide context:**
- Link to tickets, RFCs, or documentation
- Reference external systems or APIs
- Explain workarounds or temporary solutions

### Questions to Ask

- Can this code be understood without the comment?
- Does the comment add information not in the code?
- Would better naming eliminate the need for this comment?
- Is the comment explaining "what" or "why"?

### Validation

Check project documentation for comment philosophy:
- Does the project prefer minimal comments?
- Are there specific comment requirements (public APIs, complex algorithms)?
- What's the balance between comments and self-documenting code?

**Important:** Finding the same bad comment pattern elsewhere in the codebase does not make it acceptable. Consistency with a bad pattern is still a bad pattern - flag it regardless.

## Naming Clarity

### Principle

Names should follow **existing conventions** in the codebase. Consistency with established patterns matters more than any specific naming philosophy.

### Red Flags

**Inconsistent with existing patterns:**
- New code uses different naming style than similar existing code
- Related components don't follow same naming pattern
- Name doesn't match what existing code calls similar concepts

**Breaks established layer conventions:**
- Controller doesn't follow controller naming pattern
- Model doesn't follow model naming pattern
- New code uses different convention than existing code in same layer

**Unclear or misleading:**
- Name doesn't convey purpose or role
- Name suggests wrong abstraction or relationship
- Name is ambiguous or confusing

**Note:** Different layers using different names for related concepts can be intentional (e.g., controller uses domain term, model uses table name). This is only a red flag if it breaks the established pattern for that codebase.

### Good Naming

**Follow existing patterns:**
- Check how existing similar code names things
- Use same conventions as related components
- Match the style and terminology already established

**Match the level of abstraction:**
- Identify whether existing code is domain-centric, implementation-centric, or hybrid
- Use the same abstraction level for similar components
- If different layers use different levels, follow the pattern for that layer
- Examples:
  - Domain-centric: `Customer`, `Order`, `Payment`
  - Implementation-centric: `CustomerTable`, `OrderRecord`, `PaymentDTO`
  - Hybrid: `CustomerService` (domain) + `CustomerRepository` (implementation)

**Be consistent across layers:**
- Same concept should have same name in all layers (unless layer-specific conventions differ)
- Related components should follow same naming pattern
- Check what existing code calls this concept in each layer

**When conventions vary by layer:**
- Controllers might use domain terms while models use table names
- Services might use business terms while DAOs use technical terms
- Understand the pattern and follow it

### Questions to Ask

- What do existing similar components call this?
- What naming conventions exist in the codebase?
- Is this name consistent across all layers?
- Does this match the established pattern?
- Would someone familiar with the codebase expect this name?

### Validation

1. **Find similar existing code** - Look for components with similar roles or responsibilities

2. **Identify the naming pattern** - What conventions exist?
   - What words or terms are used?
   - What structure or format (e.g., VerbNoun, NounVerb)?
   
3. **Determine the level of abstraction** - What does "consistent" mean here?
   - **Domain-centric**: Names reflect business concepts (e.g., `Order`, `Customer`, `Invoice`)
   - **Implementation-centric**: Names reflect technical details (e.g., `OrderTable`, `CustomerRecord`, `InvoiceDTO`)
   - **Hybrid**: Mix of both (e.g., `OrderService` uses domain term, `OrderRepository` uses implementation term)
   - **Layer-specific**: Different abstraction levels per layer (e.g., controllers use domain terms, models use table names)

4. **Match the abstraction level** - Use the same level as existing code
   - If existing code uses domain terms, use domain terms
   - If existing code uses implementation terms, use implementation terms
   - If existing code mixes levels, understand the pattern for when to use which

5. **Verify consistency across all layers** - Does each layer follow its own convention?

6. **When in doubt, ask** - "What naming convention should I follow for this type of component?"

## Redundancy Detection

### Principle

Avoid multiple mechanisms doing the same thing. Prefer established patterns over ad-hoc solutions.

### Common Redundancies

**ID/Key generation:**
- Global mechanism (framework plugin, base class)
- Local mechanism (model hook, initializer)
- Explicit setting (in calling code)
- Check if multiple are present - which one actually runs?

**Validation logic:**
- Duplicated across layers (API, service, model)
- Duplicated when extending base classes
- Check if base class already validates

**Authorization checks:**
- Duplicated across layers (middleware, controller, service)
- Check if framework provides global mechanism

**Error handling:**
- Duplicated rescue blocks
- Check if framework provides global error handling

### Detection Process

**1. Find global mechanisms:**
- Check for framework plugins or middleware
- Check for base classes that provide functionality
- Look for configuration that enables automatic behavior

**2. Find local mechanisms:**
- Check for hooks (before/after callbacks)
- Check for explicit code in methods
- Look for duplicated patterns across files

**3. Trace execution:**
- What runs first?
- What actually does the work?
- What's redundant?

**4. Compare with existing patterns:**
- How does existing code handle this?
- Is there an established pattern?
- Why is this different?

### Questions to Ask

- Is there a global mechanism handling this?
- How do existing similar components handle this?
- What's the execution order?
- Which mechanism actually does the work?
- Can we remove any without breaking functionality?
- Why was this duplicated instead of using existing mechanism?

### Validation

**Check for dead code:**
- If explicit setting always runs, hooks are dead code
- If global mechanism runs, local mechanism may be redundant
- Test by removing suspected redundancy

**Verify with existing patterns:**
- Do other similar components have this redundancy?
- Or do they rely on single mechanism?
- What's the established pattern?

## Project Conventions

### Principle

Understand project-specific patterns before reviewing. Don't assume conventions from other projects.

### What to Learn

**Documentation philosophy:**
- Minimal comments vs verbose documentation
- Where should documentation live (code, README, wiki)
- What requires comments (public APIs, complex algorithms, all code)

**Architectural patterns:**
- How to extend existing components
- Where different types of code belong
- Naming conventions for different layers

**Global mechanisms:**
- What's handled automatically by framework
- What plugins or middleware are active
- What base classes provide

**Code style:**
- Formatting preferences
- Idiom preferences
- Testing patterns

### How to Learn

**Read project documentation:**
- Look for CONTRIBUTING, ARCHITECTURE, or similar docs
- Check for coding standards or style guides
- Review README for project philosophy

**Study existing code:**
- Find similar components
- See how they handle common concerns
- Identify patterns and conventions

**Check configuration:**
- Look for framework configuration
- Check for enabled plugins or middleware
- Review base classes and mixins

### Apply to Review

**When evaluating comments:**
- Do they match project philosophy?
- Are they consistent with existing code?

**When checking naming:**
- Do names follow project conventions?
- Are they consistent with existing patterns?

**When spotting redundancy:**
- What's handled globally in this project?
- What's the established pattern?

**When assessing architecture:**
- Does this match project patterns?
- Is this how existing code handles this?

## Success Criteria

- [ ] Comments explain "why" not "what"
- [ ] Names reflect domain concepts, not implementation
- [ ] No unnecessary redundancy
- [ ] Follows project conventions
- [ ] Consistent with existing codebase patterns
- [ ] Code is self-explanatory where possible
