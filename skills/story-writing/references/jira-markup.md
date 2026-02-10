# Jira Markup and Formatting

Jira-specific markup, formatting, and conventions for writing user stories.

## Jira Markup Reference

### Headers
```
h1. Story Title
h2. Major Section
h3. Subsection
h4. Sub-subsection
h5. Minor heading
h6. Smallest heading
```

**Convention**: Use `h1.` for title, `h2.` for major sections, `h3.` for acceptance criteria scenarios

### Text Formatting

#### Inline Formatting
- `*bold*` → **bold**
- `_italic_` → _italic_
- `{{monospace}}` → `monospace` (for code, commands, config values)
- `+underline+` → underline
- `-strikethrough-` → ~~strikethrough~~
- `^superscript^` → superscript
- `~subscript~` → subscript

**Important**: Use `{{}}` for inline code, NOT markdown backticks

#### Special Cases
- Escape hyphens in inline code when needed to prevent strikethrough
- Example: `{{\-\-flag-name}}` for flags with hyphens
- Not needed in code blocks

### Code Blocks
```
{code}
Plain text code block
{code}

{code:language}
Syntax-highlighted code block
{code}

{code:json}
{
  "field": "value"
}
{code}

{code:bash}
$ command --flag value
{code}
```

**Supported languages**: java, javascript, python, sql, xml, html, css, bash, and many more

### Block Quotes
```
{quote}
Important note or callout
{quote}
```

### Lists

#### Unordered Lists
```
* Level 1
** Level 2
*** Level 3
```

#### Ordered Lists
```
# Level 1
## Level 2
### Level 3
```

#### Mixed Lists
```
* Unordered item
*# Ordered sub-item
*# Another ordered sub-item
* Another unordered item
```

### Tables

#### Basic Table
```
||Header 1||Header 2||Header 3||
|Cell 1|Cell 2|Cell 3|
|Cell 4|Cell 5|Cell 6|
```

**Rules**:
- `||` (double pipes) for headers
- `|` (single pipes) for data rows
- No separator rows needed
- No need to align columns

#### Bold Data Cells
```
||Header 1||Header 2||
|Normal|Normal|
|*Bold*|*Bold*|
```

Use bold data cells to highlight changes or important rows.

### Links

#### External Links
```
[Link Text|https://example.com]
[https://example.com]  (auto-linked)
```

#### Jira Links
```
[TICKET-123]  (auto-linked to ticket)
[Link Text|TICKET-123]
```

#### Anchors
```
{anchor:section-name}

[Jump to section|#section-name]
```

### Horizontal Rules
```
----
```

Creates a horizontal line separator.

### Colors
```
{color:red}Red text{color}
{color:#FF0000}Hex color{color}
```

**Standard colors**: red, green, blue, yellow, orange, purple, gray, etc.

### Panels
```
{panel:title=Panel Title}
Panel content
{panel}

{info}
Info panel (blue)
{info}

{note}
Note panel (yellow)
{note}

{warning}
Warning panel (orange)
{warning}

{tip}
Tip panel (green)
{tip}
```

### Images
```
!image.png!
!image.png|thumbnail!
!image.png|width=300!
!^attachment.png!  (from attachments)
```

## Jira Story Structure

### Recommended Section Order
```
h1. TICKET-123: Brief descriptive title

*Status:* Backlog
*Type:* Story
*Epic:* EPIC-456
*Priority:* Medium

h2. Summary

One-line description

h2. Description

[High-level context or Given/When/Then]

----

*Acceptance Criteria*

h3. Base Case

*Given* [preconditions]
*When* [action]
*Then* [outcome]

{code}
Example
{code}

----

h3. Notes

*Implementation Guidance:*
* Key point 1
* Key point 2

h4. Boilerplate
* Standard guidance

h3. Links
* *Prior Art:* [TICKET-000|url] - Description
* *Epic:* [EPIC-456]

h2. Labels

label1, label2, label3

h2. Notes

[Empty section at end]
```

### Metadata Format
Use plain text with `*field:*` format:
```
*Status:* Backlog
*Type:* Story
*Epic:* EPIC-123
*Priority:* High
*Points:* 3
```

## Jira-Specific Conventions

### Ticket References
- Always use bracket notation: `[TICKET-123]`
- Jira will auto-link to the ticket
- Can add custom text: `[See related work|TICKET-123]`

### Attachments
- Reference with `^filename.ext^` in image syntax
- Example: `!^screenshot.png!`

### Mentions
- Use `[~username]` to mention users
- Example: `Assigned to [~jsmith]`

### Smart Links
Jira auto-links many patterns:
- Ticket IDs: `TICKET-123`
- URLs: `https://example.com`
- Email: `user@example.com`

### Checkboxes (in comments)
```
[] Unchecked item
[x] Checked item
```

**Note**: Checkboxes work in comments but not in description fields

## Tables for Story Writing

Tables are useful for enumerating multiple cases and their corresponding outcomes in acceptance criteria.

### Basic Table Syntax

```
||*Header 1*||*Header 2*||*Header 3*||
|Value A|Value B|Value C|
|Value D|Value E|Value F|
```

**Key points:**
- Header row uses `||` (double pipes)
- Data rows use `|` (single pipes)
- Use `*text*` to bold headers or important values
- No separator rows needed (Jira adds them automatically)

### Example: Enumerating Test Cases

```
||*Input*||*Expected Result*||*Notes*||
|Valid email|Success|User created|
|Empty email|Error|Validation message shown|
|Invalid format|Error|Format validation message|
|Duplicate email|Error|Already exists message|
```

**When to use tables:**
- Multiple similar test cases with different inputs/outputs
- Comparing behavior across different roles or states
- Documenting field requirements or constraints
- Any scenario where tabular format aids clarity

**Tips:**
- Keep tables focused and not too wide
- Use Notes column for clarifications
- Bold important or changed values with `*value*`
- Align pipes for readability (optional, not required by Jira)

## Jira Story Best Practices

### Use Sections Wisely
- `h2.` for major sections (Summary, Description, Labels)
- `h3.` for subsections (acceptance criteria, Notes, Links)
- `h4.` for sub-subsections (Boilerplate)
- Don't skip heading levels

### Leverage Jira Features
- Use `{quote}` for important callouts
- Use `{info}`, `{warning}`, `{tip}` panels for special notes
- Use tables for structured data
- Use code blocks with language hints for syntax highlighting

### Keep It Clean
- Remove empty heading markers
- Don't leave trailing whitespace
- Use consistent indentation in lists
- Align table pipes for readability (optional, not required)

### Link Effectively
- Link to related tickets
- Link to documentation
- Link to prior art
- Use descriptive link text

## Common Jira Markup Mistakes

1. ❌ Using markdown headers (`##`) instead of Jira headers (`h2.`)
2. ❌ Using markdown backticks (`` `code` ``) instead of Jira monospace (`{{code}}`)
3. ❌ Using `||value||` for data rows (should be `|value|`)
4. ❌ Adding separator rows in tables (`|-|-|`)
5. ❌ Forgetting to close code blocks with `{code}`
6. ❌ Using wrong quote syntax (markdown `>` instead of `{quote}`)
7. ❌ Indenting code blocks (they should start at column 0)
8. ❌ Mixing markdown and Jira markup
9. ❌ Forgetting language hints in code blocks
10. ❌ Not escaping special characters when needed

## Jira Workflow Integration

### Status Values
Common status values (customize for your workflow):
- Backlog / To Do
- Ready / Refined
- In Progress / In Development
- In Review / Code Review
- Testing / QA
- Done / Closed

### Story Types
Common issue types:
- Story - New feature or capability
- Bug - Defect or issue
- Task - Work item without user-facing change
- Epic - Large feature spanning multiple stories
- Spike - Research or investigation

### Priority Levels
Common priority values:
- Highest / P0 - Critical
- High / P1 - Important
- Medium / P2 - Normal
- Low / P3 - Minor
- Lowest / P4 - Trivial

## Quick Reference Card

### Most Used Markup
```
h1. Title
h2. Section
h3. Subsection

*bold* _italic_ {{code}}

{code:language}
code block
{code}

{quote}
important note
{quote}

||Header||Header||
|Data|Data|

* List item
# Numbered item

[Link Text|URL]
[TICKET-123]

----  (horizontal rule)
```

### Quick Story Template
```
h1. TICKET-123: Title

*Status:* Backlog
*Type:* Story

h2. Summary
Brief description

h2. Description
*Given* context
*When* action
*Then* outcome

----

*Acceptance Criteria*

h3. Base Case
*Given* precondition
*When* action
*Then* result

----

h3. Notes
Implementation guidance

h3. Links
* *Prior Art:* [TICKET-000]

h2. Labels
label1, label2
```

## Integration with Other Skills

Use this skill with:
- **[Story Writing](../SKILL.md)** - Core story structure and principles
- **Domain-specific skills** - API, CLI, UI patterns
- **System-specific skills** - Product or platform conventions

Together, these skills provide a complete framework for writing Jira stories.
