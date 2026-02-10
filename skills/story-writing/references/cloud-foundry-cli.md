# Cloud Foundry CLI Story Patterns

Patterns and conventions for writing user stories about Cloud Foundry CLI (cf CLI) commands.

## Overview

This skill provides comprehensive guidance for writing Cloud Foundry CLI (cf CLI) stories, including:

1. **CLI Story Principles** - Command-focused story structure
2. **CF CLI Command Structure** - CF-specific command patterns
3. **CF CLI Roles** - Cloud Foundry specific roles and permissions
4. **CF CLI Output** - Standard output formats and patterns
5. **CF CLI Style Guide** - Follow cf CLI conventions

## CLI Story Principles

1. **Focus on user commands** - Stories describe command execution and output
2. **Show actual commands** - Include complete command examples
3. **Include help text** - Document command usage and options
4. **Cover error cases** - Missing flags, invalid input, confirmation prompts
5. **Show exit codes** - Be explicit about success (0) and failure (non-zero)
6. **Consider UX** - Interactive prompts, progress indicators, output formatting

## Common CLI Story Types

### Command Execution Stories

#### Essential Scenarios
- **Base Case**: Successful command execution with exit code 0
- **Help Text**: Command usage, options, and examples
- **Missing Required Arguments**: Error message and exit code 1
- **Invalid Input**: Validation error and exit code 1
- **Confirmation Prompts**: Interactive confirmation (if applicable)
- **Permissions**: Insufficient permissions error

### Create/Add Commands

#### Essential Scenarios
- **Base Case**: Successfully creates resource
- **Already Exists**: Idempotent behavior or error
- **Missing Required Flags**: Usage error
- **Invalid Input**: Validation error with helpful message
- **Help Text**: Shows usage and examples

### Delete/Remove Commands

#### Essential Scenarios
- **Base Case**: Successfully deletes resource
- **Confirmation Prompt**: Asks for confirmation unless -f flag
- **Force Flag**: Skips confirmation with --force
- **Not Found**: Error when resource doesn't exist
- **Help Text**: Shows usage and options

**Note:** Don't include "declined confirmation" scenarios - only show positive confirmation case.

### Update/Modify Commands

#### Essential Scenarios
- **Base Case**: Successfully updates resource
- **Not Found**: Error when resource doesn't exist
- **Invalid Input**: Validation error
- **Partial Update**: Updates only specified fields
- **Help Text**: Shows usage and options

### List/View Commands

#### Essential Scenarios
- **Base Case**: Displays list of resources
- **Empty Results**: Appropriate message when no resources
- **Formatting**: Tabular or structured output
- **Filtering**: Optional flags for filtering results

## Exit Codes

When writing stories, be explicit about expected exit codes in acceptance criteria. Refer to the [CF CLI Style Guide](https://github.com/cloudfoundry/cli/wiki/CF-CLI-Style-Guide) for complete details.

### In Acceptance Criteria
```
Then the command completes successfully (exit 0)
Then the command fails (exit 1)
```

Common exit codes: 0 (success), 1 (error). Check style guide for specific usage.

## Cloud Foundry CLI Specifics

1. **CF CLI Conventions** - Specific command structure and style
2. **CF CLI Roles** - Cloud Foundry specific roles and permissions
3. **CF CLI Output** - Standard output formats and patterns
4. **CF CLI Style Guide** - Follow cf CLI best practices

## CF CLI Command Structure

### Standard Format

```bash
cf command-name [ARGUMENTS] [OPTIONS]
```

## CF CLI Story Structure

### Given/When/Then with Code Blocks

```
h3. Base Case

*Given* a cf CLI user with sufficient permissions
*When* the user runs {{set-org-role}}
*Then* the command completes successfully (exit {{0}})
*And* displays appropriate output

{code}
$ cf set-org-role user@example.com my-org OrgManager
Assigning role OrgManager to user user@example.com in org my-org...
OK

TIP: Use 'cf org-users my-org' to see all users and roles.
{code}
```

**Key patterns:**
- Given: "cf CLI user" with appropriate permissions
- When: Reference command name without `cf` prefix (e.g., "runs {{set-org-role}}")
- Then: Include exit code (e.g., "exit {{0}}")
- Code block: Show full command with `cf` prefix and expected output

### Scenario Naming

Use descriptive names:
- **Base Case** - Happy path
- **Already Exists** - Idempotent behavior
- **Confirmation Prompt** - User confirmation required
- **Help Text** - Command usage and options
- **Missing Required Flag** - Error case
- **Conflicting Flags** - Invalid flag combinations

**Don't use:** "Scenario #0", "Scenario #1", numbered scenarios

## CF CLI Roles and Permissions

### Cloud Foundry Roles

- **Admin** - Global administrator
- **OrgManager** - Organization manager
- **OrgAuditor** - Organization auditor (read-only)
- **OrgBillingManager** - Billing manager
- **SpaceManager** - Space manager
- **SpaceDeveloper** - Space developer
- **SpaceAuditor** - Space auditor (read-only)
- **SpaceSupporter** - [Beta role, subject to change] Manage app lifecycle and service bindings

### Role Descriptions

When documenting roles in help text:
```
SpaceSupporter: [Beta role, subject to change] - Manage app lifecycle and service bindings
```

## CF CLI Output Patterns

### Success Output

```
$ cf set-label app my-app environment=production
Setting label environment=production for app my-app...
OK

TIP: Use 'cf labels app my-app' to see all labels.
```

**Elements:**
- Action message with parameters
- "OK" on success
- Optional TIP with related command

### Error Output

```
$ cf set-label app nonexistent environment=prod
App 'nonexistent' not found.
FAILED
```

**Elements:**
- Descriptive error message
- "FAILED" indicator
- Exit code 1

### Idempotent Commands

```
$ cf set-org-role user@example.com my-org OrgManager
Assigning role OrgManager to user user@example.com in org my-org...
OK (idempotent)
```

Note idempotency in the story:
```
*Then* the command completes successfully (exit {{0}}) (idempotent)
```

### Confirmation Prompts

```
$ cf delete-space dev-space
Really delete the space dev-space? [yN]: y
Deleting space dev-space...
OK
```

**Don't include "declined confirmation" scenarios** - only show positive confirmation case.

## CF CLI Help Text

### Standard Help Format

```
NAME:
   set-label - Set a label on a resource

USAGE:
   cf set-label RESOURCE RESOURCE_NAME KEY=VALUE...

EXAMPLES:
   cf set-label app my-app environment=production team=platform
   cf set-label space dev-space purpose=development
   cf set-label org my-org cost-center=engineering

SEE ALSO:
   labels, unset-label
```

### Help Text Scenario

```
h3. Help Text

*When* the user runs {{set-label --help}}
*Then* the command displays help text including:
- Command description
- Usage patterns
- Examples
- Related commands
```

## CF CLI Style Guide

### Style Guide Reference

```
* Conform to cf CLI best practices and existing patterns
* cf CLI style guide: https://github.com/cloudfoundry/cli/wiki/CF-CLI-Style-Guide
```

### Style Guide Violations

When a command violates the style guide (e.g., required flag instead of positional argument):

```
{quote}
*Note on Style Guide:* The cf CLI style guide recommends positional arguments for required values. However, we're using {{--origin}} as a required flag because [rationale]. Future work tracked in [TICKET-ID] to align with style guide.

Style guide reference: "Required inputs should be positional arguments, not flags"
{quote}
```

## CF CLI Style Guide

### For CF CLI Stories

Include in boilerplate:

```
h4. Boilerplate
* Acceptance criteria are negotiable. If something can be accomplished in an easier way or there is a better option, let's chat!
* Conform to cf CLI best practices and existing patterns
  ** cf CLI style guide: https://github.com/cloudfoundry/cli/wiki/CF-CLI-Style-Guide
  ** Lean on CLI maintainers for guidance
```

## Example CF CLI Story Structure

```
h1. Add set-label Command for Apps

*Status:* In Progress
*Type:* Story
*Epic:* Metadata CLI

h2. Summary

Add {{set-label}} command to cf CLI for managing app labels

h2. Description

*Given* a cf CLI user
*When* managing app metadata
*Then* provide command to set labels on apps

----

*Acceptance Criteria*

h3. Base Case

*Given* a cf CLI user with sufficient permissions
*And* an app "my-app" exists
*When* the user runs {{set-label app my-app environment=production}}
*Then* the command completes successfully (exit {{0}})
*And* sets the label on the app

{code}
$ cf set-label app my-app environment=production
Setting label environment=production for app my-app...
OK

TIP: Use 'cf labels app my-app' to see all labels.
{code}

h3. Invalid Label Format

*Given* a cf CLI user with sufficient permissions
*And* an app "my-app" exists
*When* the user runs {{set-label}} with invalid label format
*Then* the command fails (exit {{1}})
*And* displays an appropriate error message

{code}
$ cf set-label app my-app invalid-format
Error: Label must be in KEY=VALUE format

Usage: cf set-label RESOURCE RESOURCE_NAME KEY=VALUE...

Run 'cf set-label --help' for more information.
FAILED
{code}

h3. Help Text

*When* the user runs {{set-label --help}}
*Then* the command displays help text including:
- Command description
- Usage: {{cf set-label RESOURCE RESOURCE_NAME KEY=VALUE...}}
- Resource types: {{app}}, {{space}}, {{org}}
- Examples
- Related commands: {{labels}}, {{unset-label}}

h3. App Not Found

*When* the user runs {{set-label app nonexistent environment=prod}}
*Then* the command fails (exit {{1}})
*And* displays error message

{code}
$ cf set-label app nonexistent environment=prod
App 'nonexistent' not found.
FAILED

TIP: Use 'cf apps' to see all apps.
{code}

----

h3. Notes

*Implementation Guidance:*
* Follow cf CLI style guide for command structure
* Use standard output format
* Include helpful TIP messages
* Support multiple labels in single command

h4. Boilerplate
* Conform to cf CLI best practices
* Follow cf CLI style guide

h3. Links
* *Prior Art:* https://cli.cloudfoundry.org/en-US/v8/set-label.html

h2. Labels

cli, apps, metadata, cf-cli

h2. Notes

[Empty]
```

## Common CF CLI Patterns

### List Commands

```
$ cf apps
Getting apps in org my-org / space dev-space...

name      state     instances   memory   disk
my-app    started   2/2         256M     1G
api-app   stopped   0/1         512M     2G
worker    started   1/1         128M     512M
```

**Pattern:**
- "Getting [resources]..." message with context
- Tabular output with headers
- Clean, aligned columns

### Delete Commands with Confirmation

```
$ cf delete-space dev-space
Really delete the space dev-space? [yN]: y
Deleting space dev-space...
OK
```

**Pattern:**
- Confirmation prompt (default No)
- Action message
- Success indicator

### Commands with --force Flag

```
$ cf delete-space dev-space --force
Deleting space dev-space...
OK
```

**Pattern:**
- Skip confirmation with `--force` or `-f`
- Proceed directly to action

## Error Message Patterns

### Include Context in Errors

```
App 'my-app' already exists in space 'dev-space'.
```

**Pattern:**
- Include relevant identifiers (name, space)
- Be specific about what failed
- Provide actionable information

### Reference Related Commands

```
Error: App 'my-app' not found.

TIP: Use 'cf apps' to see all apps.
```

**Pattern:**
- Clear error message
- Helpful TIP with related command

## Integration with Other Skills

Use this skill with:
- **[Story Writing](../SKILL.md)** - Core structure and principles
- **[Jira Markup](jira-markup.md)** - Jira markup and formatting

This skill provides comprehensive Cloud Foundry CLI story writing guidance.
