# Commands

Simple, explicit workflows triggered with `/` in Cursor chat.

## Available Commands

### [Code Review](code-review.md)
Conduct thorough, multi-round code review using systematic analysis and issue tracking.

**Usage:** `/code-review`

**Prerequisites:**
- `pr-review` skill enabled
- `agent-issue-tracking` skill enabled

**Features:**
- Multiple focused rounds of review (default: 5)
- Dynamic scheduling of additional rounds based on findings
- Systematic issue tracking for all findings
- Comprehensive summary with prioritized recommendations

---

## Using Commands

Commands are automatically discovered from:
- `.cursor/commands/` - Project-level
- `~/.cursor/commands/` - User-level global

**To activate commands:**

```bash
# Option 1: Symlink entire commands directory (recommended)
ln -s /path/to/ai_agent_rules/commands ~/.cursor/commands

# Option 2: Symlink individual commands
ln -s /path/to/ai_agent_rules/commands/code-review.md ~/.cursor/commands/code-review.md

# Option 3: Copy commands to discovery location
cp -r /path/to/ai_agent_rules/commands/* ~/.cursor/commands/
```

**Note:** This repository uses `commands/` as a development/documentation location. Commands must be in one of the discovery locations above to be used.

## Creating Commands

See the [Command Writing Skill](../skills/command-writing/SKILL.md) for guidance on creating effective commands.

## Additional Resources

- **[Cursor Commands Documentation](https://cursor.com/docs/context/commands)** - Official documentation
- **[Command Writing Skill](../skills/command-writing/SKILL.md)** - Guide for creating commands
