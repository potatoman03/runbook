---
name: elegant
description: Scrap a mediocre solution and implement the elegant one. Use after a fix feels hacky or over-complicated. Claude uses all accumulated context to find the right solution. Based on the prompting pattern from the Claude Code team.
argument-hint: [file|description]
user-invocable: true
allowed-tools: Read, Glob, Grep, Edit, Write, Bash
---

# Elegant: The Better Solution

After a mediocre fix, say: "Knowing everything you know now, scrap this and implement the elegant solution"
— Claude Code team tip

## Activation

When user invokes `/elegant` or says variations of "implement the elegant solution":

## The Elegant Solution Process

### 1. Acknowledge Context

You now have full context:
- The original problem
- Failed or mediocre attempts
- What didn't work and why
- Edge cases discovered
- The codebase patterns and constraints

This context is valuable. Use it.

### 2. Step Back

Before coding, answer:
- What is the **actual** problem being solved?
- What made previous solutions mediocre?
- What would the ideal solution look like?
- What's the simplest thing that could work?

### 3. Find the Elegant Solution

An elegant solution is:
- **Simple**: Minimal moving parts
- **Clear**: Intent is obvious
- **Robust**: Handles edge cases naturally, not through patches
- **Aligned**: Fits existing patterns in the codebase
- **Maintainable**: Future developers will understand it

Look for:
- A different abstraction that makes the problem trivial
- An existing utility/pattern that solves this
- A way to eliminate the problem instead of solving it
- The approach an experienced maintainer would take

### 4. Present the Solution

Before implementing, explain:

```markdown
## The Elegant Solution

**Previous approach problems:**
- [What was wrong with the mediocre solution]

**The insight:**
[What realization leads to a better solution]

**The elegant approach:**
[High-level description]

**Why this is better:**
- [Benefit 1]
- [Benefit 2]

**Trade-offs:**
- [Any downsides to acknowledge]
```

### 5. Implement Cleanly

When implementing:
- Start fresh, don't patch the mediocre solution
- Write the code you wish you'd written first
- Add appropriate tests
- Remove the old mediocre code entirely

### 6. Verify Improvement

- Run tests
- Compare code size and complexity
- Ensure all original requirements still met

## Triggering Phrases

All of these activate elegant mode:
- "Knowing everything you know now, scrap this and implement the elegant solution"
- "This is hacky, what's the right way?"
- "There has to be a better way"
- "This feels overcomplicated"
- "/elegant"
- "Start over and do it right"

## Anti-Patterns to Avoid

The elegant solution is NOT:
- Adding more special cases
- Wrapping bad code in a nice interface
- Over-engineering for hypothetical futures
- Premature optimization
- Showing off clever tricks

## Example Session

```
User: [After several attempts at handling timezone conversions]
This is getting messy. /elegant

Claude: Looking at everything we've tried...

## The Elegant Solution

**Previous approach problems:**
We were converting timezones at multiple points - on input,
in the database, and on output. Each conversion added edge
cases and bugs.

**The insight:**
Store everything in UTC. Only convert at the boundary - when
displaying to users. One conversion point, not three.

**The elegant approach:**
1. All dates stored as UTC timestamps
2. User's timezone stored in their profile
3. Single `formatForUser(date, user)` utility for display
4. All comparisons done in UTC (no conversion needed)

**Why this is better:**
- Eliminates entire class of timezone bugs
- Simpler mental model
- Standard practice in the industry
- Removes 40 lines of conversion code

**Trade-offs:**
- Need to migrate existing data to UTC
- May need to update a few tests

Want me to implement this?

User: Yes

Claude: [Implements clean UTC-based solution, removes old code,
updates tests, verifies everything works]
```
