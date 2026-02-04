# Workflow Examples

Real-world workflows using the runbook skills.

## Workflow 1: Feature Development (Parallel)

**Setup: 3 worktrees for a new feature**

```bash
# Terminal 1 - Analysis
za  # Jump to worktree A
> "Read the codebase and explain how the current auth system works"
> "Draw an ASCII diagram of the data flow"

# Terminal 2 - Planning
zb  # Jump to worktree B
> "Enter plan mode. Design JWT authentication to replace sessions"
> [Review plan]
> "/plan-review"  # Get staff engineer feedback
> [Revise plan]

# Terminal 3 - Implementation
zc  # Jump to worktree C
> [After plan approved in B]
> "Implement the JWT auth plan. Use subagents for parallel work"
```

**Benefits:**
- Analysis doesn't pollute implementation context
- Plan can be reviewed while analysis continues
- Implementation starts with clean context

---

## Workflow 2: Bug Fix (Zero Context Switching)

**Scenario: Bug reported in Slack**

```
# In Claude
[Paste Slack bug thread]

> "Fix."

Claude:
- Parses bug details
- Finds relevant code
- Identifies root cause
- Implements fix
- Adds regression test
- Verifies fix
```

**Or for CI failures:**

```
> "Go fix the failing CI tests"

Claude:
- Runs `gh run view --log-failed`
- Parses failures
- Fixes each one
- Verifies all pass
```

---

## Workflow 3: End-of-Session Cleanup

**Before finishing for the day:**

```
> "/techdebt"

Claude reports:
- 3 duplicated patterns found
- 2 unused functions
- 1 file over 500 lines

> "Fix the quick wins"

Claude removes dead code, extracts duplicated pattern

> "What should we add to CLAUDE.md from today?"

Claude suggests rules based on corrections made

> "/update-rules"

Claude adds rules to CLAUDE.md
```

---

## Workflow 4: Learning New Codebase

**Day 1 on a new project:**

```
> "/explain architecture"

Claude:
- Scans codebase structure
- Generates ASCII diagrams
- Explains each major component

> "/explain --slides src/core/engine.ts"

Claude generates HTML presentation

> "Ask me questions to test my understanding"

Claude quizzes you on the codebase
```

---

## Workflow 5: Code Review Prep

**Before submitting PR:**

```
> "/grill"

Claude:
- Analyzes your changes
- Asks probing questions
- Challenges your decisions

[Answer questions]

Claude: "PASS - You understand these changes. Ready for PR."

> "Prove to me this works"

Claude:
- Runs tests on main
- Runs tests on branch
- Compares behavior
- Shows evidence it works

> "/commit"
> "/pr"
```

---

## Workflow 6: Refactoring Session

**Improving code quality:**

```
> "/techdebt src/services/"

Claude identifies:
- 4 functions doing similar things
- Inconsistent error handling
- Missing abstractions

> "Refactor the error handling to be consistent. Use subagents."

Claude spawns subagents to:
- Subagent 1: Define standard error types
- Subagent 2: Create error utilities
- Subagent 3: Update service A
- Subagent 4: Update service B

> [After implementation feels hacky]
> "/elegant"

Claude:
- Steps back
- Finds better abstraction
- Reimplements cleanly
```

---

## Workflow 7: Complex Feature with Review Loop

**High-stakes feature:**

```
# Session 1: Planning
> "Enter plan mode. Design the payment processing system."

[Claude writes detailed plan]

# Session 2: Review
> "/plan-review"

Claude (as staff engineer):
- Questions design decisions
- Identifies risks
- Suggests alternatives

> "Address the concerns about idempotency and retry logic"

[Revise plan]

# Session 3: Implementation
> "Implement the payment plan. Use subagents for parallel work."

Claude coordinates:
- Subagent 1: Payment models and database
- Subagent 2: Stripe integration
- Subagent 3: Webhook handlers
- Subagent 4: Tests

> "/grill"

[Pass the grilling]

> "/pr"
```

---

## Workflow 8: Data Analysis Session

**Using dedicated analysis worktree:**

```bash
# Jump to analysis worktree
cd .claude/worktrees/analysis && claude
```

```
> "Query the last 7 days of signups by source using bq CLI"

Claude:
```bash
bq query --use_legacy_sql=false '
SELECT
  source,
  COUNT(*) as signups,
  DATE(created_at) as date
FROM `project.dataset.users`
WHERE created_at > TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 7 DAY)
GROUP BY source, date
ORDER BY date DESC, signups DESC
'
```

> "Visualize this as an ASCII chart"

Claude generates ASCII visualization

> "What's causing the spike on Tuesday?"

Claude investigates correlated events
```

---

## Workflow 9: Onboarding New Team Member

**Create understanding materials:**

```
> "/explain --slides architecture"
> "/explain --slides auth"
> "/explain --slides database"

[Generate HTML presentations for each system]

> "Create an ASCII map of the entire system with labels"

> "Generate a glossary of domain terms used in this codebase"

> "What are the 10 most important files a new developer should understand?"
```

---

## Workflow 10: Emergency Production Fix

**Urgent bug in production:**

```
> "We have a production incident. Here are the logs:
[paste logs]

Fix this as fast as possible."

Claude:
- Identifies error pattern
- Finds root cause
- Implements minimal fix
- Adds logging for future debugging
- Creates PR with "URGENT" label

> "Is this safe to deploy? Any risks?"

Claude reviews for regressions

> "/grill"  # Quick sanity check

> "Merge and deploy"
```

---

## Tips from the Team

1. **Voice dictation** - Hit fn twice on macOS. You speak 3x faster than you type.

2. **Color-code tabs** - Visual distinction between worktrees/tasks.

3. **Use tmux** - One pane per worktree for easy switching.

4. **System notifications** - Know when Claude needs input across tabs.

5. **Name worktrees by task** - `wt-auth`, `wt-api`, not `wt-1`, `wt-2`.

6. **Keep one analysis worktree** - For logs, queries, investigation only.

7. **Run /techdebt daily** - Prevent debt accumulation.

8. **Update CLAUDE.md after every correction** - Compound improvement over time.
