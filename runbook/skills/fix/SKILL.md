---
name: fix
description: Auto-fix bugs, failing CI tests, or issues from error logs. Just paste context and say "fix." Based on the Claude Code team practice of zero-context-switching bug fixing. Works with Slack threads, CI output, docker logs, error traces.
argument-hint: [ci|bug|logs] [context]
user-invocable: true
allowed-tools: Read, Glob, Grep, Bash, Edit, Write
---

# Fix: Auto Bug Fixing

Enable the Slack MCP, then paste a Slack bug thread into Claude and just say "fix." Zero context switching required.
Or, just say "Go fix the failing CI tests." Don't micromanage how.
— Claude Code team tips

## Activation

When user invokes `/fix` with context, or pastes error/bug info and says "fix":

## Fix Modes

### Mode 1: Fix CI (`/fix ci`)

When user says `/fix ci` or "fix failing CI" or "fix the tests":

1. **Get CI Status**
   ```bash
   # GitHub Actions
   gh run list --limit 5
   gh run view [run-id] --log-failed

   # Or check local test output
   npm test 2>&1 | tail -100
   ```

2. **Identify Failures**
   Parse the output for:
   - Failed test names and files
   - Error messages and stack traces
   - Assertion failures

3. **Fix Each Failure**
   For each failing test:
   - Read the test file
   - Understand what it's testing
   - Read the implementation being tested
   - Determine if bug is in test or implementation
   - Fix the appropriate code

4. **Verify**
   ```bash
   npm test
   ```

5. **Report**
   "Fixed N failing tests. Changes made to: [files]"

### Mode 2: Fix Bug (`/fix bug` or paste bug report)

When user pastes a bug thread, error report, or says `/fix bug`:

1. **Parse the Bug Context**
   Extract:
   - Error messages
   - Stack traces
   - Steps to reproduce
   - Expected vs actual behavior
   - Affected files/components

2. **Investigate**
   - Read the relevant code
   - Understand the flow
   - Identify root cause

3. **Implement Fix**
   - Make minimal changes to fix the issue
   - Don't refactor unrelated code
   - Add regression test if appropriate

4. **Verify**
   - Run relevant tests
   - Manually verify if possible

### Mode 3: Fix from Logs (`/fix logs`)

When user says `/fix logs` or points to log output:

1. **Analyze Logs**
   ```bash
   # Docker logs
   docker logs <container> --tail 500 2>&1 | grep -i error

   # Application logs
   tail -500 /var/log/app.log | grep -i "error\|exception\|failed"
   ```

2. **Identify Patterns**
   - Recurring errors
   - Error timestamps and frequency
   - Correlation with events

3. **Trace to Code**
   - Find the code generating each error
   - Understand the conditions causing it

4. **Fix Systematically**
   - Address root causes, not symptoms
   - Consider if error handling needs improvement

## Slack MCP Integration

If Slack MCP is enabled, you can:

1. User pastes Slack bug thread URL
2. Fetch thread content via MCP
3. Parse bug details from messages
4. Fix without any manual context gathering

## Power Prompts

- "Fix" - Just the word, with context already pasted
- "Go fix the failing CI tests" - Claude figures out the rest
- "Fix whatever is in these docker logs: [logs]"
- "The build is red, fix it"

## Example Sessions

### CI Fix
```
User: /fix ci

Claude: Checking CI status...
Found 2 failing tests:
- auth.test.ts: "should reject expired tokens"
- api.test.ts: "should rate limit requests"

Analyzing failures...

auth.test.ts failure: Token expiry check using wrong timezone.
[Shows fix]

api.test.ts failure: Rate limit window calculation off by one.
[Shows fix]

Running tests... All passing!

Fixed 2 tests:
- src/auth/token.ts:45 - Fixed timezone handling
- src/api/rateLimit.ts:23 - Fixed window calculation
```

### Bug Thread Fix
```
User: [Pastes Slack thread about users getting logged out randomly]

/fix

Claude: Analyzing bug report...

Root cause identified: Session refresh race condition.
When two API calls happen simultaneously, both try to refresh
the session token, causing one to invalidate the other's session.

Implementing fix with mutex lock on token refresh...
[Shows changes]

Added regression test for concurrent refresh scenario.
All tests passing.
```
