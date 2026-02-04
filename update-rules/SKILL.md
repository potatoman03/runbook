---
name: update-rules
description: Update CLAUDE.md with lessons learned from mistakes. After any correction, use this to ensure Claude doesn't make the same mistake again. Claude is "eerily good at writing rules for itself." Based on core advice from the Claude Code team.
argument-hint: [lesson]
user-invocable: true
allowed-tools: Read, Edit, Write, Glob
---

# Update Rules: CLAUDE.md Maintenance

After every correction, end with: "Update your CLAUDE.md so you don't make that mistake again."
— Boris Cherny, creator of Claude Code

## Activation

When user invokes `/update-rules` or says "update rules" or "add to CLAUDE.md":

## Process

### 1. Identify the Lesson

If $ARGUMENTS provided, use that as the lesson context.
Otherwise, analyze the recent conversation for:
- Corrections the user made
- Mistakes that were pointed out
- Preferences that were expressed
- Patterns that should be followed

### 2. Find or Create CLAUDE.md

Check for existing CLAUDE.md in order:
1. `./CLAUDE.md` (project root)
2. `./.claude/CLAUDE.md`
3. `~/.claude/CLAUDE.md` (global)

If none exists, create at `./CLAUDE.md`.

### 3. Draft the Rule

Write a clear, actionable rule. Good rules are:
- **Specific**: Not "be careful with X" but "always do Y when X"
- **Contextual**: Include when the rule applies
- **Actionable**: Clear what to do, not just what to avoid

**Bad rule:**
```
Be careful with database migrations.
```

**Good rule:**
```
## Database Migrations
- Always create a rollback migration alongside forward migrations
- Test migrations on a copy of production data before deploying
- Never drop columns in the same release that stops using them
```

### 4. Categorize and Place

Organize rules into sections:
- `## Code Style` - formatting, naming conventions
- `## Architecture` - patterns, structure decisions
- `## Testing` - test requirements, coverage expectations
- `## Git & PR` - commit messages, PR process
- `## [Project-Specific]` - domain-specific rules

### 5. Update the File

Add the new rule to the appropriate section. If section doesn't exist, create it.

### 6. Confirm the Update

Show the user what was added and where.

## Advanced: Notes Directory Pattern

One engineer on the team maintains a notes directory for every task/project, updated after every PR:

```
.claude/
├── CLAUDE.md           # Points to notes
└── notes/
    ├── auth-refactor.md
    ├── api-v2.md
    └── performance-fixes.md
```

In CLAUDE.md:
```markdown
## Project Notes
See `.claude/notes/` for task-specific context and lessons learned.
Always read relevant notes before starting work on that area.
```

To set this up:
```bash
mkdir -p .claude/notes
```

## Maintenance Tips

- **Ruthlessly edit** - Remove outdated or redundant rules
- **Keep iterating** - Update until Claude's mistake rate measurably drops
- **Review periodically** - Prune rules that are no longer relevant
- **Be specific** - Vague rules don't help

## Example Session

```
User: No, we use tabs not spaces in this project.

Claude: I'll update the code to use tabs.
[fixes code]

User: /update-rules

Claude: Adding to CLAUDE.md:

## Code Style
- Use tabs for indentation, not spaces
- This applies to all source files (.ts, .tsx, .js, .jsx)

Added to ./CLAUDE.md under "Code Style" section.
```
