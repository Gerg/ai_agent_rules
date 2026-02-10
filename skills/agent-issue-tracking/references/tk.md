# tk Command Reference

Quick reference for the `tk` utility. See [Agent Issue Tracking](../SKILL.md) for concepts and patterns.

## Prerequisites
- `tk` utility in PATH
- `.tickets` directory (created automatically on first use)

## Core Commands

### Create Ticket
```bash
tk create "[title]" -t [type] -p [1-4] [options]
```

**Options:**
- `-t, --type`: bug, feature, task, epic, chore
- `-p, --priority`: 1-4 (1=critical, 4=low)
- `--parent [id]`: Create as child of parent ticket
- `--external-ref [ref]`: Link to external ticket (Jira, GitHub, etc.)
- `--tags [tag1,tag2]`: Comma-separated tags
- `-d, --description`: Detailed description

**Example:**
```bash
tk create "Fix validation bug" -t bug -p 1 --tags validation,security
```

### Status Management
```bash
tk start [id]      # Mark as in_progress
tk close [id]      # Mark as closed
tk show [id]       # Show ticket details
```

### Notes
```bash
tk add-note [id] "[note text]"
```

Notes support markdown formatting.

### Listing
```bash
tk list                          # All tickets
tk list --status=open            # Filter by status
tk list --status=in_progress
tk list --status=closed
tk list -a "[assignee]"          # Filter by assignee
tk list -T [tag1,tag2]           # Filter by tags
tk closed --limit=N              # Recently closed tickets
```

## Dependencies

```bash
tk dep [ticket-id] [blocks-on-id]    # ticket-id depends on blocks-on-id
tk ready                              # Show tickets ready to work on
tk blocked                            # Show blocked tickets
tk dep tree [ticket-id]               # View dependency tree
```

## Query and Export

```bash
tk query                              # Export all tickets as JSON
tk query | jq '.[] | select(...)'    # Filter with jq
```

## Tips

**Partial ID matching:**
```bash
tk show abc-5    # Matches abc-53wp if unique
```

**Capture ticket ID:**
```bash
parent_id=$(tk create "Parent task" -t task -p 1)
tk create "Child task" --parent $parent_id
```

**Structured notes:**
```bash
tk add-note [id] "
## Finding
[What was found]

## Resolution
[What was done]
"
```

**Initialize if needed:**
```bash
mkdir -p .tickets    # If .tickets doesn't exist
```
