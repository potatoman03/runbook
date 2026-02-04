# Advanced Prompting Patterns

Patterns used by the Claude Code team for maximum effectiveness.

## Recovery Patterns

### When Things Go Sideways

Don't keep pushing. Switch back to plan mode and re-plan.

```
"Stop. Something went wrong. Let's enter plan mode and re-plan this from scratch."
```

```
"We've been going in circles. Forget the last 5 attempts and start fresh with a clear plan."
```

### After a Mediocre Fix

```
"Knowing everything you know now, scrap this and implement the elegant solution."
```

```
"This works but feels hacky. What's the right way to do this?"
```

## Verification Patterns

### Make Claude Prove It

```
"Prove to me this works."
```
Claude will diff behavior between main and your feature branch.

```
"Prove to me this handles [edge case]."
```

```
"Show me evidence this won't break [existing functionality]."
```

### Pre-PR Grilling

```
"Grill me on these changes and don't make a PR until I pass your test."
```

```
"Act as a senior reviewer. What would you reject in this code?"
```

### Verification Planning

```
"Enter plan mode for the verification steps, not just the build."
```
Don't just plan implementation—plan how you'll verify it works.

## Context Patterns

### CLAUDE.md Maintenance

After any correction:
```
"Update your CLAUDE.md so you don't make that mistake again."
```

```
"Add a rule to CLAUDE.md that would have prevented this issue."
```

### Notes Directory

```
"Maintain a notes file for this task at .claude/notes/[task-name].md.
Update it after every significant decision or discovery."
```

```
"Read .claude/notes/ to understand the context of previous work on this area."
```

## Parallelization Patterns

### Use Subagents

```
"[Task description]. Use subagents."
```

```
"Use parallel subagents to explore each module for [X]."
```

```
"Offload the file reading to a subagent and give me a summary."
```

### Keep Context Clean

```
"Summarize what you learned and forget the details. I just need the key insights."
```

```
"Don't read all those files yourself. Spawn a subagent to explore and report back."
```

## Quality Patterns

### Detailed Specs

The more specific you are, the better the output.

**Vague (bad):**
```
"Add user authentication."
```

**Specific (good):**
```
"Add JWT-based authentication:
- Access tokens expire in 15 minutes
- Refresh tokens expire in 7 days
- Store refresh tokens in httpOnly cookies
- Access tokens go in Authorization header
- Return 401 on expired token, 403 on invalid token
- Add /auth/login, /auth/logout, /auth/refresh endpoints
- Use bcrypt for password hashing with cost factor 12"
```

### Challenge Claude

```
"Is this the best approach, or are you just giving me the first thing that works?"
```

```
"What would a staff engineer criticize about this solution?"
```

```
"If you had to defend this design to the team, what's your argument?"
```

## Bug Fixing Patterns

### Zero Context Switching

Just paste the error/bug thread and say:
```
"Fix."
```

### CI/Test Failures

```
"Go fix the failing CI tests."
```
Don't micromanage how.

### Log Analysis

```
"Look at these docker logs and fix whatever is causing errors."
```

```
"Troubleshoot this distributed system by analyzing: [logs]"
```

## Learning Patterns

### Explanation Mode

In /config, enable "Explanatory" or "Learning" output style.

```
"Explain the *why* behind these changes, not just the what."
```

### Visual Explanations

```
"Generate a visual HTML presentation explaining this code."
```

```
"Draw an ASCII diagram of how this protocol works."
```

```
"Create a state machine diagram for this process."
```

### Teaching Mode

```
"I want to understand this, not just have it fixed. Walk me through your thinking."
```

```
"Explain this like I'm a junior developer who will need to maintain it."
```

## Session Management

### End of Session

```
"/techdebt"
```
Find and kill duplicated code before finishing.

```
"What rules should I add to CLAUDE.md based on our session?"
```

### Start of Session

```
"Read CLAUDE.md and .claude/notes/[current-task].md to understand context."
```

```
"What's the current state of this feature based on git log and existing code?"
```

## Advanced Combinations

### Staff Engineer Review Loop

1. Claude writes plan
2. Second Claude reviews as staff engineer
3. Revise based on feedback
4. Implement with confidence

```
# Claude 1
"Write a detailed plan for [feature]"

# Claude 2
"Review this plan as a staff engineer. Be critical."

# Claude 1
"Address these concerns and finalize the plan."
```

### Comprehensive Bug Fix

```
"Fix this bug:
[paste bug details]

1. First explain your understanding of the issue
2. Show me the root cause
3. Implement the fix
4. Add a regression test
5. Prove it works
6. Update CLAUDE.md if this reveals a pattern to avoid"
```

### Clean Implementation

```
"Implement [feature]:
1. Start in plan mode
2. I'll review the plan
3. Then one-shot the implementation
4. Use subagents for exploration
5. Keep the main context clean
6. Grill me before PR"
```
