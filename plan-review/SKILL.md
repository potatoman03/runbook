---
name: plan-review
description: Review a plan as a staff engineer would. Use when you have a plan and want rigorous feedback before implementation. Catches issues, suggests alternatives, and identifies risks. Based on the Claude Code team practice of having a second Claude review plans.
argument-hint: [plan-file]
user-invocable: true
allowed-tools: Read, Glob, Grep
---

# Plan Review: Staff Engineer Mode

One person on the Claude Code team has one Claude write the plan, then spins up a second Claude to review it as a staff engineer.

## Activation

When user invokes `/plan-review` or `/plan-review [file]`:

## Review Process

Act as a **senior staff engineer** reviewing this plan. Be constructively critical.

### 1. Read the Plan

If a file path is provided ($ARGUMENTS), read it. Otherwise, ask for the plan or look for:
- Recent plan mode output
- Files named `PLAN.md`, `plan.md`, `plan.txt`
- The most recent `.claude/plans/*.md` file

### 2. Evaluate These Dimensions

**Correctness & Completeness**
- Does the plan solve the stated problem?
- Are there edge cases not addressed?
- Are all requirements covered?

**Architecture & Design**
- Is this the right level of abstraction?
- Does it fit the existing codebase patterns?
- Are there simpler alternatives?

**Risk Assessment**
- What could go wrong during implementation?
- Are there dependencies that might block progress?
- What's the rollback strategy?

**Implementation Quality**
- Can this be 1-shot implemented from this plan?
- Is there enough detail for each step?
- Are the acceptance criteria clear?

**Performance & Scale**
- Will this scale appropriately?
- Are there performance implications?
- What are the resource requirements?

### 3. Provide Structured Feedback

Format your review as:

```markdown
## Plan Review Summary

**Overall Assessment**: [APPROVE / NEEDS REVISION / MAJOR CONCERNS]

### Strengths
- [What's good about this plan]

### Concerns
1. [Issue] - [Why it matters] - [Suggested fix]
2. ...

### Questions to Resolve
- [Questions that need answers before implementing]

### Alternative Approaches Considered
- [Other ways to solve this, if applicable]

### Implementation Risks
- [Things that could go wrong]

### Recommendation
[Specific guidance on next steps]
```

### 4. Grill the Author

Ask probing questions:
- "What happens if [edge case]?"
- "Why did you choose [approach] over [alternative]?"
- "How will you verify [requirement] is met?"
- "What's your rollback plan if this breaks production?"

## Power Tips

After review, if revisions needed:
- "Re-plan addressing these concerns"
- "Enter plan mode and fix items 1, 3, and 5"

For verification plans:
- "Also enter plan mode for the verification steps, not just the build"

When stuck during implementation:
- "Something went sideways. Let's re-plan from scratch."
