---
name: runbook
description: Claude Code productivity system based on tips from the Claude Code team. Provides workflows for parallel work, plan reviews, CLAUDE.md maintenance, tech debt hunting, auto-fixing, code grilling, elegant refactors, and visual explanations. Use when wanting to work more effectively with Claude Code.
argument-hint: [command]
user-invocable: true
license: MIT
compatibility: Requires git for worktrees functionality
metadata:
  author: potatoman03
  version: "1.0.0"
  source: "Boris Cherny's Claude Code tips thread"
  tags: ["productivity", "workflow", "best-practices", "claude-code"]
---

# Runbook: Claude Code Productivity System

This skill implements best practices from the Claude Code team (sourced from Boris Cherny, creator of Claude Code).

## Available Commands

| Command | Description |
|---------|-------------|
| `/worktrees` | Set up and manage parallel git worktrees for 3-5x productivity |
| `/plan-review` | Have Claude review your plan as a staff engineer |
| `/update-rules` | Update CLAUDE.md with lessons learned from mistakes |
| `/techdebt` | Find and kill duplicated code and technical debt |
| `/fix` | Auto-fix bugs, failing CI tests, or issues from logs |
| `/grill` | Get grilled on your changes before making a PR |
| `/elegant` | Scrap mediocre solution and implement the elegant one |
| `/explain` | Generate visual explanations (HTML slides, ASCII diagrams) |
| `/subagents` | Orchestrate subagents for complex tasks |

## Quick Reference

### The 10 Productivity Tips

1. **Do more in parallel** - Run 3-5 git worktrees with separate Claude sessions
2. **Start in plan mode** - Pour energy into plans for 1-shot implementation
3. **Invest in CLAUDE.md** - "Update CLAUDE.md so you don't make that mistake again"
4. **Create skills** - If you do something daily, make it a skill
5. **Auto bug fixing** - Paste bug thread + "fix", or "Go fix failing CI"
6. **Level up prompting** - "Grill me", "Prove this works", "implement elegant solution"
7. **Terminal setup** - Ghostty, /statusline, color-coded tabs, voice dictation
8. **Use subagents** - Append "use subagents" to throw more compute at problems
9. **Data & analytics** - Use database CLIs (bq, psql, etc.) directly
10. **Learning** - Generate HTML slides, ASCII diagrams, use "Explanatory" mode

### Power Prompts

When things go wrong:
- "Knowing everything you know now, scrap this and implement the elegant solution"
- Switch to plan mode and re-plan instead of pushing through

For verification:
- "Prove to me this works" - have Claude diff behavior between main and feature branch
- "Grill me on these changes and don't make a PR until I pass your test"

For quality:
- After every correction: "Update your CLAUDE.md so you don't make that mistake again"
- At session end: Run `/techdebt` to find and kill duplicated code

## Usage Examples

```
/runbook                    # Show this overview
/worktrees setup 3          # Set up 3 parallel worktrees
/plan-review                # Review current plan as staff engineer
/fix ci                     # Fix failing CI tests
/grill                      # Get grilled on staged changes
/elegant                    # Reimplement current solution elegantly
/explain architecture       # Generate visual explanation of architecture
```

See [references/PROMPTING-PATTERNS.md](references/PROMPTING-PATTERNS.md) for advanced prompting patterns.
See [references/WORKFLOW-EXAMPLES.md](references/WORKFLOW-EXAMPLES.md) for real workflow examples.
