---
name: subagents
description: Orchestrate subagents for complex tasks. Append "use subagents" to throw more compute at problems. Keep main context clean by offloading to specialized subagents. Based on the Claude Code team's subagent practices.
argument-hint: [task-description]
user-invocable: true
---

# Subagents: Parallel Processing Power

Append "use subagents" to any request where you want Claude to throw more compute at the problem.
Offload individual tasks to subagents to keep your main agent's context window clean and focused.
— Claude Code team tips

## Activation

When user says "use subagents" or `/subagents [task]`:

## When to Use Subagents

### Good Candidates for Subagents

- **Exploration tasks**: "Find all usages of X across the codebase"
- **Parallel analysis**: "Check each module for security issues"
- **Independent implementation**: "Implement these 5 API endpoints"
- **Research**: "Understand how the auth system works"
- **Verification**: "Test this change against all edge cases"

### Keep in Main Context

- Tasks requiring conversation history
- Decisions needing user input
- Work that builds on previous steps
- Final synthesis and presentation

## Subagent Strategies

### Strategy 1: Divide and Conquer

For large tasks, split into independent pieces:

```
Main task: "Add validation to all API endpoints"

Subagent 1: Validate user endpoints (users/*.ts)
Subagent 2: Validate auth endpoints (auth/*.ts)
Subagent 3: Validate billing endpoints (billing/*.ts)
Subagent 4: Write shared validation utilities

Main: Synthesize results, ensure consistency
```

### Strategy 2: Explore Then Execute

Use exploration subagent first:

```
Subagent (Explore): "Understand how the payment system works"
  → Returns: Architecture summary, key files, patterns

Main: Uses findings to implement changes correctly
```

### Strategy 3: Parallel Review

Multiple subagents review different aspects:

```
Subagent 1: Security review of changes
Subagent 2: Performance review of changes
Subagent 3: Test coverage analysis
Subagent 4: Documentation check

Main: Synthesizes all feedback
```

### Strategy 4: Research Army

For unfamiliar territory:

```
Subagent 1: Read and summarize module A
Subagent 2: Read and summarize module B
Subagent 3: Read and summarize module C
Subagent 4: Find all integration points

Main: Builds mental model from summaries
```

## Subagent Types

| Type | Use For |
|------|---------|
| `Explore` | Codebase exploration, finding patterns, understanding architecture |
| `Plan` | Creating implementation plans for subtasks |
| `general-purpose` | Implementing features, fixing bugs, research |

## Context Management Tips

### Keep Main Context Clean

Offload to subagents:
- Reading many files
- Searching across codebase
- Trial-and-error exploration
- Detailed analysis

Keep in main context:
- User requirements
- Key decisions made
- Final implementation
- Overall coordination

### Handoff Information

When spawning subagents, provide:
- Clear, specific task description
- Relevant constraints
- Expected output format
- Files/areas to focus on

## Example Session

```
User: Refactor the auth system to use JWT. use subagents

Claude: I'll use subagents to parallelize this work.

**Spawning exploration subagent...**
Task: Map the current auth system - find all files, understand
the session handling, identify all auth check points.

**Spawning research subagent...**
Task: Document the current auth middleware chain and how
requests are authenticated.

[Subagents return findings]

**Analysis complete.** Current auth uses:
- Session-based auth in 12 files
- 3 middleware functions
- Custom session store in Redis

**Spawning implementation subagents...**

Subagent 1: Create JWT utilities (generate, verify, refresh)
Subagent 2: Update auth middleware for JWT
Subagent 3: Create token refresh endpoint
Subagent 4: Update all auth checks (12 files)

[Subagents complete implementation]

**Synthesizing changes...**
All implementations complete. Running tests...

Summary:
- 15 files modified
- JWT auth fully implemented
- All 47 tests passing
- Session fallback maintained for migration period
```

## Auto-Approval Hook

Route permission requests to Opus 4.5 via a hook to auto-approve safe operations:

```json
// .claude/settings.json
{
  "hooks": {
    "pre_tool_call": {
      "command": "claude --model opus -p 'Is this tool call safe? Respond YES or NO: $TOOL_CALL'"
    }
  }
}
```

See: code.claude.com/docs/en/hooks
