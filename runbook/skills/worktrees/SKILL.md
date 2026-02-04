---
name: worktrees
description: Set up and manage git worktrees for running 3-5 parallel Claude sessions. The single biggest productivity unlock from the Claude Code team. Use when you want to parallelize work, set up worktrees, or manage multiple Claude sessions.
argument-hint: [setup|list|remove|aliases] [count]
user-invocable: true
allowed-tools: Read, Bash, Glob
---

# Worktrees: Parallel Claude Sessions

Running 3-5 git worktrees with separate Claude sessions is the **single biggest productivity unlock** according to the Claude Code team.

## Commands

### Setup Worktrees

When user says `/worktrees setup [count]`:

1. First, check if we're in a git repository:
   ```bash
   git rev-parse --is-inside-work-tree
   ```

2. Create the worktrees directory structure:
   ```bash
   mkdir -p .claude/worktrees
   ```

3. For each worktree (default 3, or $1 if specified):
   ```bash
   git worktree add .claude/worktrees/wt-a origin/main
   git worktree add .claude/worktrees/wt-b origin/main
   git worktree add .claude/worktrees/wt-c origin/main
   ```
   Use letters a, b, c, d, e for naming (up to 5).

4. Suggest shell aliases for quick navigation:
   ```bash
   # Add to ~/.zshrc or ~/.bashrc:
   alias za='cd $(git rev-parse --show-toplevel)/.claude/worktrees/wt-a && claude'
   alias zb='cd $(git rev-parse --show-toplevel)/.claude/worktrees/wt-b && claude'
   alias zc='cd $(git rev-parse --show-toplevel)/.claude/worktrees/wt-c && claude'
   ```

5. Optionally create a dedicated "analysis" worktree for logs and queries:
   ```bash
   git worktree add .claude/worktrees/analysis origin/main
   ```

### List Worktrees

When user says `/worktrees list`:
```bash
git worktree list
```

### Remove Worktrees

When user says `/worktrees remove [name]`:
```bash
git worktree remove .claude/worktrees/wt-$1
# Or remove all:
git worktree remove .claude/worktrees/wt-a
git worktree remove .claude/worktrees/wt-b
# etc.
```

### Generate Aliases

When user says `/worktrees aliases`:
Generate shell aliases for all existing worktrees that user can add to their shell config.

## Workflow Tips

1. **Name your worktrees by task** - wt-auth, wt-api, wt-frontend
2. **Use one worktree per feature/bug** - keeps context clean
3. **Have a dedicated analysis worktree** - for reading logs, running BigQuery, investigating
4. **Color-code terminal tabs** - visual distinction between worktrees
5. **Use tmux** - one pane per worktree for easy switching

## Example Session

```
$ /worktrees setup 3
Creating 3 worktrees...
  .claude/worktrees/wt-a (from origin/main)
  .claude/worktrees/wt-b (from origin/main)
  .claude/worktrees/wt-c (from origin/main)

Add these aliases to your shell:
  alias za='cd /path/to/repo/.claude/worktrees/wt-a && claude'
  alias zb='cd /path/to/repo/.claude/worktrees/wt-b && claude'
  alias zc='cd /path/to/repo/.claude/worktrees/wt-c && claude'

Then: za, zb, zc to jump into each worktree with Claude running!
```
