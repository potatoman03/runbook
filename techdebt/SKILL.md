---
name: techdebt
description: Find and kill duplicated code and technical debt. Run at the end of every session to clean up. Based on the Claude Code team practice of building a /techdebt slash command.
argument-hint: [path]
user-invocable: true
allowed-tools: Read, Glob, Grep, Bash
agent: Explore
context: fork
---

# Tech Debt: Find and Kill

Build a /techdebt slash command and run it at the end of every session to find and kill duplicated code.
— Claude Code team tip

## Activation

When user invokes `/techdebt` or `/techdebt [path]`:

## Analysis Process

### 1. Scope the Search

If $ARGUMENTS provided, focus on that path. Otherwise, analyze:
- Recently modified files (from git)
- The entire `src/` or main source directory

```bash
# Get recently modified files
git diff --name-only HEAD~10
```

### 2. Hunt for Duplication

**Code Duplication**
Look for:
- Copy-pasted functions with minor variations
- Repeated logic blocks (3+ lines appearing 2+ times)
- Similar components/classes that could be unified
- Repeated error handling patterns
- Duplicated validation logic

**Structural Duplication**
Look for:
- Multiple files with nearly identical structure
- Repeated import patterns
- Similar test setups

### 3. Identify Other Tech Debt

**Dead Code**
- Unused exports
- Commented-out code blocks
- Functions never called
- Unused variables/imports

**Code Smells**
- Functions over 50 lines
- Files over 500 lines
- Deep nesting (4+ levels)
- Magic numbers/strings
- Missing error handling
- TODO/FIXME comments older than 30 days

**Dependency Issues**
- Outdated packages with security vulnerabilities
- Unused dependencies
- Circular dependencies

### 4. Generate Report

Format findings as:

```markdown
## Tech Debt Report

### Critical (Fix Now)
- [Security/correctness issues]

### Duplication Found
| Pattern | Occurrences | Files | Suggested Fix |
|---------|-------------|-------|---------------|
| [desc]  | N           | [files] | [action]    |

### Dead Code
- `path/file.ts:123` - unused function `foo()`
- ...

### Code Smells
- `path/file.ts` - 847 lines, consider splitting
- ...

### Quick Wins
- [Easy fixes with high impact]

### Refactoring Opportunities
- [Larger improvements to consider]
```

### 5. Offer to Fix

After presenting the report:
- "Want me to fix the critical issues?"
- "Should I deduplicate [specific pattern]?"
- "I can remove the dead code if you'd like"

## Automated Checks

For comprehensive analysis, run these commands:

```bash
# Find similar code blocks (if jscpd installed)
npx jscpd ./src --min-lines 5 --min-tokens 50

# Find unused exports (if ts-prune installed)
npx ts-prune

# Check for outdated deps
npm outdated

# Find TODO/FIXME comments
grep -rn "TODO\|FIXME\|HACK\|XXX" --include="*.ts" --include="*.js" src/
```

## Example Session

```
User: /techdebt

Claude: Analyzing codebase for tech debt...

## Tech Debt Report

### Duplication Found
| Pattern | Occurrences | Files | Suggested Fix |
|---------|-------------|-------|---------------|
| Error response formatting | 4 | api/*.ts | Extract to `formatError()` utility |
| User validation logic | 3 | auth/, users/ | Create shared `validateUser()` |

### Dead Code
- `src/utils/legacy.ts` - entire file unused (0 imports)
- `src/components/OldButton.tsx:45` - unused export `ButtonVariant`

### Code Smells
- `src/services/auth.ts` - 523 lines, consider splitting
- 12 TODO comments older than 30 days

### Quick Wins
1. Remove `legacy.ts` - 0 impact, cleaner codebase
2. Extract error formatting - 15 min, removes 60 lines of duplication

Want me to tackle the quick wins?
```
