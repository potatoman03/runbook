# Runbook

**A Claude Code productivity system based on tips from the Claude Code team.**

This skill implements the 10 best practices shared by Boris Cherny (creator of Claude Code) and his team at Anthropic. It transforms how you work with Claude Code—from solo prompting to parallelized, systematic, compounding productivity.

---

## Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [The Philosophy](#the-philosophy)
- [Commands](#commands)
  - [/worktrees](#worktrees---parallel-work-sessions)
  - [/plan-review](#plan-review---staff-engineer-review)
  - [/update-rules](#update-rules---claudemd-maintenance)
  - [/techdebt](#techdebt---find-and-kill-technical-debt)
  - [/fix](#fix---auto-bug-fixing)
  - [/grill](#grill---pre-pr-challenge)
  - [/elegant](#elegant---scrap-and-do-it-right)
  - [/explain](#explain---visual-learning)
  - [/subagents](#subagents---parallel-compute)
- [Power Prompts](#power-prompts)
- [Workflows](#workflows)
- [File Structure](#file-structure)
- [Credits](#credits)

---

## Installation

### Personal Installation (All Your Projects)

```bash
cp -r runbook ~/.claude/skills/
```

### Project Installation (Single Project)

```bash
mkdir -p .claude/skills
cp -r runbook .claude/skills/
```

### Verify Installation

```bash
claude
> /runbook
```

You should see the skill overview with all available commands.

---

## Quick Start

```bash
# See all commands
/runbook

# Set up 3 parallel worktrees
/worktrees setup 3

# After Claude makes a mistake and you correct it
/update-rules

# End of session cleanup
/techdebt

# Before submitting a PR
/grill

# Paste a bug report, then:
/fix
```

---

## The Philosophy

The Claude Code team works differently than most users. Their core principles:

### 1. Parallelize Everything
Don't wait for one Claude to finish. Run 3-5 Claude sessions simultaneously in separate git worktrees. This is their **#1 productivity tip**.

### 2. Plan First, Code Once
Pour energy into the plan. A good plan lets Claude one-shot the implementation. When things go sideways, don't push through—go back to planning.

### 3. Compound Learning
Every mistake is an opportunity. Update CLAUDE.md after every correction. Over time, Claude gets better at *your* codebase, *your* patterns, *your* preferences.

### 4. Delegate Fully
Don't micromanage Claude. Say "fix" not "first look at file X, then check Y, then..." Trust Claude to figure out the how.

### 5. Challenge Before Shipping
Get grilled on your changes before they reach real reviewers. Prove your code works. Catch issues early.

### 6. Clean Up Daily
Run `/techdebt` at the end of every session. Prevent debt from accumulating. Keep the codebase healthy.

---

## Commands

### `/worktrees` - Parallel Work Sessions

**What it does:** Creates multiple copies of your codebase (git worktrees) so you can run separate Claude sessions simultaneously.

**Why it matters:** This is the **single biggest productivity unlock** according to the Claude Code team. While Claude works on Task A in one terminal, you start Task B in another. No waiting.

**Usage:**
```bash
/worktrees setup 3      # Create 3 worktrees
/worktrees list         # Show existing worktrees
/worktrees remove       # Clean up worktrees
/worktrees aliases      # Generate shell aliases
```

**How it works:**
```
Terminal 1 (za): Claude writing tests
Terminal 2 (zb): Claude refactoring auth
Terminal 3 (zc): You investigating a bug with Claude
```

The skill creates worktrees at `.claude/worktrees/wt-a`, `wt-b`, `wt-c` and suggests shell aliases (`za`, `zb`, `zc`) for one-keystroke navigation.

**Pro tips:**
- Name worktrees by task: `wt-auth`, `wt-api`, `wt-frontend`
- Keep a dedicated "analysis" worktree for logs and investigation
- Color-code your terminal tabs for visual distinction
- Use system notifications to know when a Claude needs input

---

### `/plan-review` - Staff Engineer Review

**What it does:** Reviews your implementation plan as a senior staff engineer would—critically examining it for flaws, edge cases, and better alternatives.

**Why it matters:** Catching problems in the plan is 10x cheaper than catching them in code. The Claude Code team has one Claude write the plan, then a second Claude review it.

**Usage:**
```bash
/plan-review              # Review plan from recent context
/plan-review plan.md      # Review specific plan file
```

**The review process:**
1. Claude reads your plan
2. Evaluates: correctness, architecture, risks, implementation quality
3. Asks probing questions: "What happens if X?" "Why not Y?"
4. Gives verdict: APPROVE, NEEDS REVISION, or MAJOR CONCERNS

**Example output:**
```markdown
## Plan Review Summary

**Overall Assessment**: NEEDS REVISION

### Strengths
- Clean separation of concerns
- Good test coverage plan

### Concerns
1. Race condition possible in token refresh - add mutex
2. No rollback strategy if migration fails

### Questions to Resolve
- What happens with concurrent requests during deployment?

### Recommendation
Address the race condition before implementing.
```

**The pattern:**
1. Write plan (or have Claude write it)
2. `/plan-review` to critique it
3. Revise based on feedback
4. Implement with confidence

---

### `/update-rules` - CLAUDE.md Maintenance

**What it does:** After Claude makes a mistake and you correct it, this adds a rule to CLAUDE.md so the mistake never happens again.

**Why it matters:** Claude is "eerily good at writing rules for itself." Your CLAUDE.md becomes a knowledge base that compounds over time. The team says to do this after *every* correction.

**Usage:**
```bash
/update-rules                           # Analyze recent corrections
/update-rules "we use tabs not spaces"  # Add specific lesson
```

**Example flow:**
```
You: "No, we use tokens not sessions here"
Claude: [fixes code]
You: /update-rules

Claude adds to CLAUDE.md:
## Authentication
- Use JWT tokens, not session-based auth
- Access tokens in Authorization header
- Refresh tokens in httpOnly cookies
```

**What makes a good rule:**
- **Specific**: Not "be careful with X" but "always do Y when X"
- **Contextual**: Includes when the rule applies
- **Actionable**: Clear what to do, not just what to avoid

**Advanced pattern:** Maintain a notes directory for each project:
```
.claude/
├── CLAUDE.md           # Points to notes
└── notes/
    ├── auth-refactor.md
    ├── api-v2.md
    └── performance-fixes.md
```

---

### `/techdebt` - Find and Kill Technical Debt

**What it does:** Scans your codebase for duplicated code, dead code, code smells, and other technical debt. Run at the end of every session.

**Why it matters:** Debt accumulates silently. The Claude Code team built this as a daily habit—find and kill debt before it compounds.

**Usage:**
```bash
/techdebt              # Scan recent changes
/techdebt src/         # Scan specific directory
```

**What it finds:**

| Category | Examples |
|----------|----------|
| **Duplication** | Copy-pasted functions, repeated logic blocks, similar components |
| **Dead code** | Unused exports, commented-out code, functions never called |
| **Code smells** | Functions >50 lines, files >500 lines, deep nesting, magic numbers |
| **Stale TODOs** | TODO/FIXME comments older than 30 days |

**Example output:**
```markdown
## Tech Debt Report

### Duplication Found
| Pattern | Occurrences | Suggested Fix |
|---------|-------------|---------------|
| Error formatting | 4 files | Extract to `formatError()` |
| User validation | 3 files | Create shared `validateUser()` |

### Dead Code
- `src/utils/legacy.ts` - entire file unused

### Quick Wins
1. Remove legacy.ts (0 impact)
2. Extract error formatting (removes 60 lines of duplication)

Want me to tackle the quick wins?
```

---

### `/fix` - Auto Bug Fixing

**What it does:** Paste a bug report, error log, or Slack thread, then say "fix." Claude figures out the rest—finds the code, identifies the root cause, implements the fix, adds tests.

**Why it matters:** Zero context switching. You don't need to read the bug, find the file, understand the code, then explain to Claude. Just paste and delegate.

**Usage:**
```bash
/fix ci                 # Fix failing CI tests
/fix bug                # Fix from bug report (paste context first)
/fix logs               # Fix from error logs
```

**Or just:**
```
[paste Slack thread or error log]
> fix
```

**Example - CI fix:**
```
> /fix ci

Claude:
Checking CI status...
Found 2 failing tests:
- auth.test.ts: "should reject expired tokens"
- api.test.ts: "should rate limit requests"

Analyzing... Token expiry using wrong timezone. Rate limit off by one.

[fixes both]

Running tests... All passing!
```

**Example - Bug fix:**
```
[paste Slack thread: "Users getting randomly logged out"]

> fix

Claude:
Root cause: Race condition in token refresh. When two API calls
happen simultaneously, both try to refresh, one invalidates the other.

Implementing mutex lock on refresh...
Added regression test for concurrent refresh.
All tests passing.
```

---

### `/grill` - Pre-PR Challenge

**What it does:** Claude acts as a tough (but fair) code reviewer. It analyzes your changes, asks probing questions, and won't approve until you demonstrate understanding.

**Why it matters:** Catches issues before real reviewers do. Forces you to think through edge cases. Builds confidence that your code is solid.

**Usage:**
```bash
/grill                  # Grill on staged changes
/grill --staged         # Explicitly staged changes
/grill src/auth/        # Grill specific files
```

**The grilling process:**
1. Claude analyzes your diff
2. Asks 5-7 probing questions
3. You answer each one
4. Claude gives verdict: PASS, NEEDS WORK, or FAIL

**Types of questions:**
- **Understanding**: "Walk me through what happens when [edge case]?"
- **Defensive**: "What if this input is null?"
- **Design**: "Why this approach over [alternative]?"
- **Security**: "Is this input validated?"

**Example:**
```
> /grill

Claude: You've modified 3 files. Let's begin.

Question 1/5: In login.ts line 34, you're storing the refresh token
in localStorage. What happens if a user has multiple tabs open?

You: The other tabs would have a stale token...

Claude: Correct. How does your implementation handle this?

[...continues grilling...]

Final Verdict: PASS
You understand these changes. Ready for PR.
```

**Bonus challenges:**
- "Prove to me this works" - Claude diffs behavior between main and your branch
- "Try to break it" - Claude attempts adversarial inputs

---

### `/elegant` - Scrap and Do It Right

**What it does:** After hacking at a problem and ending up with a messy solution, invoke this. Claude uses everything it learned from failed attempts to find the *right* approach.

**Why it matters:** First attempts accumulate patches and workarounds. Sometimes stepping back reveals a simpler approach that makes the problem trivial.

**Usage:**
```bash
/elegant                    # Reimagine current solution
/elegant src/auth/token.ts  # Reimagine specific file
```

**The magic prompt:**
> "Knowing everything you know now, scrap this and implement the elegant solution"

**What Claude does:**
1. Acknowledges what made previous solutions bad
2. Identifies the insight that leads to a better solution
3. Presents the elegant approach before implementing
4. Implements cleanly from scratch (not patching the old code)

**Example:**
```
[After messy timezone conversion attempts]

> /elegant

Claude:
## The Elegant Solution

**Previous approach problems:**
We were converting timezones at three points—input, database, output.
Each added edge cases.

**The insight:**
Store everything in UTC. Only convert at display time.

**The elegant approach:**
1. All dates stored as UTC timestamps
2. Single `formatForUser(date, user)` utility
3. All comparisons done in UTC

**Why this is better:**
- Eliminates entire class of timezone bugs
- Removes 40 lines of conversion code
- Industry standard practice

Want me to implement this?
```

**Trigger phrases:**
- "This is hacky, what's the right way?"
- "There has to be a better way"
- "Start over and do it right"
- `/elegant`

---

### `/explain` - Visual Learning

**What it does:** Generates visual explanations of code, architecture, or concepts. Creates ASCII diagrams, HTML slide presentations, and interactive visualizations.

**Why it matters:** Visuals accelerate understanding. When learning unfamiliar code or explaining to others, diagrams and slides help. Claude makes "surprisingly good slides."

**Usage:**
```bash
/explain architecture           # ASCII diagram of system
/explain --ascii auth flow      # ASCII diagram of specific flow
/explain --slides payment.ts    # HTML presentation
/explain --interactive api      # Interactive HTML explanation
```

**ASCII diagram example:**
```
┌─────────────────────────────────────────────────────────┐
│                    Load Balancer                         │
└─────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│  Web Server   │   │  Web Server   │   │  Web Server   │
└───────┬───────┘   └───────┬───────┘   └───────┬───────┘
        └───────────────────┼───────────────────┘
                            ▼
                  ┌─────────────────┐
                  │  PostgreSQL DB  │
                  └─────────────────┘
```

**HTML slides:** Generated as self-contained HTML files at `.claude/explanations/[topic].html` with keyboard navigation (arrow keys).

**Use cases:**
- Learning a new codebase on day 1
- Explaining architecture to team members
- Understanding complex data flows
- Documenting systems visually

---

### `/subagents` - Parallel Compute

**What it does:** For complex tasks, Claude spawns "subagents"—separate Claude instances that work on pieces of the problem in parallel. The main Claude coordinates them.

**Why it matters:**
1. **More compute** - Throw more processing power at hard problems
2. **Clean context** - Subagents handle messy exploration; main Claude stays focused

**Usage:**
```bash
/subagents refactor auth to use JWT
```

**Or append to any request:**
```
> Implement the new payment system. Use subagents.
```

**How it works:**
```
Main Claude receives: "Refactor auth to use JWT"

Spawns:
├── Subagent 1: Create JWT utilities (generate, verify, refresh)
├── Subagent 2: Update auth middleware
├── Subagent 3: Create token refresh endpoint
└── Subagent 4: Update all auth checks (12 files)

All work in parallel.

Main Claude: Synthesizes results, ensures consistency, runs tests.
```

**When to use subagents:**
- Exploration across large codebase
- Parallel analysis (security, performance, tests)
- Independent implementations
- Research tasks

**When NOT to use:**
- Tasks requiring conversation history
- Decisions needing your input mid-task
- Work that builds sequentially

---

## Power Prompts

Quick reference for prompts the Claude Code team uses:

### Recovery
```
"Knowing everything you know now, scrap this and implement the elegant solution"
"Stop. Let's re-plan this from scratch."
"We've been going in circles. Start fresh."
```

### Verification
```
"Prove to me this works"
"Grill me on these changes and don't make a PR until I pass"
"Try to break this"
```

### Learning
```
"Update your CLAUDE.md so you don't make that mistake again"
"Explain the why behind these changes, not just the what"
```

### Delegation
```
"Fix." (with context pasted)
"Go fix the failing CI tests"
"[task]. Use subagents."
```

### Quality
```
"Is this the best approach, or just the first thing that works?"
"What would a staff engineer criticize about this?"
```

---

## Workflows

### Daily Development Flow
```
1. /worktrees setup 3           # Start parallel sessions
2. Work across multiple tasks   # Don't wait for Claude
3. /update-rules                # After any corrections
4. /techdebt                    # End of session cleanup
5. /grill                       # Before any PR
```

### Complex Feature Flow
```
1. Enter plan mode              # Design the approach
2. /plan-review                 # Get staff engineer feedback
3. Revise plan                  # Address concerns
4. Implement with subagents     # Parallel execution
5. /grill                       # Challenge yourself
6. /elegant                     # If solution feels hacky
7. Submit PR                    # With confidence
```

### Bug Fix Flow
```
1. [Paste bug report or logs]
2. "Fix."                       # That's it. Delegate fully.
```

### Learning Flow
```
1. /explain architecture        # Get the big picture
2. /explain --slides [file]     # Deep dive on specific code
3. Ask questions                # Fill knowledge gaps
```

See `references/WORKFLOW-EXAMPLES.md` for 10 complete real-world workflows.

---

## File Structure

```
runbook/
├── SKILL.md                      # Main entry point (/runbook)
├── README.md                     # This documentation
├── skills/
│   ├── worktrees/SKILL.md        # /worktrees
│   ├── plan-review/SKILL.md      # /plan-review
│   ├── update-rules/SKILL.md     # /update-rules
│   ├── techdebt/SKILL.md         # /techdebt
│   ├── fix/SKILL.md              # /fix
│   ├── grill/SKILL.md            # /grill
│   ├── elegant/SKILL.md          # /elegant
│   ├── explain/SKILL.md          # /explain
│   └── subagents/SKILL.md        # /subagents
└── references/
    ├── PROMPTING-PATTERNS.md     # 30+ power prompts
    └── WORKFLOW-EXAMPLES.md      # 10 complete workflows
```

---

## Credits

Based on [Boris Cherny's Twitter thread](https://x.com/bcherny/status/1885103675015516287) sharing tips from the Claude Code team at Anthropic. Boris is the creator of Claude Code.

This skill implements all 10 tips:

| Tip | Implementation |
|-----|----------------|
| 1. Do more in parallel | `/worktrees` |
| 2. Start in plan mode | `/plan-review` |
| 3. Invest in CLAUDE.md | `/update-rules` |
| 4. Create your own skills | This entire skill |
| 5. Claude fixes bugs by itself | `/fix` |
| 6. Level up prompting | `/grill`, `/elegant` |
| 7. Terminal setup | `/worktrees` aliases |
| 8. Use subagents | `/subagents` |
| 9. Data & analytics | Workflow examples |
| 10. Learning with Claude | `/explain` |

---

## License

MIT

---

**Remember:** There is no one right way to use Claude Code. Experiment to see what works for you.
