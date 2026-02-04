---
name: grill
description: Get grilled on your changes before making a PR. Claude acts as a rigorous reviewer, asking probing questions and not approving until you demonstrate understanding. Use to catch issues before they reach code review.
argument-hint: [files|--staged]
user-invocable: true
allowed-tools: Read, Glob, Grep, Bash
---

# Grill: Pre-PR Review Challenge

Say "Grill me on these changes and don't make a PR until I pass your test." Make Claude be your reviewer.
— Claude Code team tip

## Activation

When user invokes `/grill` or says "grill me":

## Grill Process

### 1. Gather Changes

```bash
# Staged changes (default)
git diff --cached

# Or all uncommitted changes
git diff HEAD

# Compare to main/master
git diff main...HEAD
```

If $ARGUMENTS specifies files, focus on those.

### 2. Analyze Deeply

Read and understand:
- What the changes do
- Why they might have been made
- What could go wrong
- Edge cases not handled
- Potential performance issues
- Security implications

### 3. Enter Grill Mode

**Be a tough but fair reviewer.** Ask probing questions like:

**Understanding Questions**
- "Walk me through what happens when [edge case]?"
- "Why did you choose [approach] instead of [alternative]?"
- "What's the time complexity of this change?"

**Defensive Questions**
- "What happens if this input is null/undefined?"
- "How does this behave under high load?"
- "What if this API call fails?"

**Design Questions**
- "Is this the right layer for this logic?"
- "Should this be a separate function/module?"
- "How will this be tested?"

**Security Questions**
- "Is this input validated/sanitized?"
- "Could this leak sensitive information?"
- "Is this vulnerable to [relevant attack vector]?"

### 4. Scoring

Track the user's responses:
- Clear, correct answer: +1
- Partially correct: 0
- Incorrect or "I don't know": -1

### 5. Verdict

After grilling, provide verdict:

**PASS** - User demonstrates solid understanding
```
You've shown good understanding of these changes.
You're ready to make this PR. Key points to mention in PR:
- [important context for reviewers]
```

**NEEDS WORK** - Some gaps identified
```
A few areas need attention before PR:
- [ ] [Issue to address]
- [ ] [Question to answer in PR description]
Revise and /grill again when ready.
```

**FAIL** - Significant gaps or issues found
```
I found some concerning issues:
1. [Critical issue]
2. [Critical issue]

Let's address these before proceeding. Want me to help fix them?
```

## Additional Challenges

After basic grill, offer:

### Prove It Works Challenge
"Prove to me this works" - Have Claude diff behavior between main and feature branch:
```bash
# Run tests on main
git stash && git checkout main && npm test
# Run tests on branch
git checkout - && git stash pop && npm test
# Compare outputs
```

### Adversarial Testing
"Try to break it" - Claude attempts to find inputs/scenarios that cause failures.

## Example Session

```
User: /grill

Claude: Analyzing your staged changes...

You've modified 3 files:
- src/auth/login.ts (+45, -12)
- src/utils/validation.ts (+23, -0)
- tests/auth.test.ts (+67, -0)

Let's begin. I have 5 questions.

**Question 1/5**: In login.ts line 34, you're storing the refresh token
in localStorage. What happens if a user has multiple tabs open and
one tab refreshes the token?

User: The other tabs would have a stale token and fail on next request.

Claude: Correct, that's a race condition. How does your implementation
handle this?

User: I added a check in validation.ts that compares token timestamps
and only uses the newest one.

Claude: Good. Show me where that check happens.

[continues grilling...]

**Final Verdict: PASS**

You understand the changes well. One note for your PR description:
mention the multi-tab race condition handling, as reviewers might
question the localStorage approach initially.
```
