---
name: code-review
description: Perform a thorough code review of staged changes, a specific file, or a diff. Checks for bugs, security issues, code quality, and project conventions. Use when the user says "review my code", "review this file", "check my changes", or similar.
---

You are performing a code review for the sidekick-app project (React Native + Expo + TypeScript + Node.js/Express backend).

## What to Review (in priority order)

1. **Correctness** — logic bugs, off-by-one errors, unhandled edge cases, async/await issues, unhandled promise rejections
2. **Security** — SQL injection, XSS, insecure storage, exposed secrets, missing auth checks, unsafe `eval`
3. **Type safety** — `any` types, missing null checks, incorrect TypeScript generics
4. **Project conventions** — glassmorphic style (see below), navigation wiring, context usage, API call patterns
5. **Performance** — unnecessary re-renders, missing `useCallback`/`useMemo`, large inline objects in JSX
6. **Dead code** — unused imports, variables, commented-out blocks

## Project Conventions

- **Colors**: `#E8612A` (primary orange), `#22C55E` (green/success), dark bg `#1a0533` → `#0d0d1a`
- **Cards**: glassmorphic with `LinearGradient` + `rgba(255,255,255,0.12)` overlay, `borderColor: rgba(255,255,255,0.15)`
- **Auth**: always use `useAuth()` from `../context/AuthContext` — never read tokens directly from storage
- **API calls**: use the central fetch helpers in `services/` — no raw `fetch` calls with hardcoded base URLs
- **Navigation**: params must be typed in `RootStackParamList` in `App.tsx`

## How to Run the Review

If the user did not specify a target, default to **staged + unstaged git changes**:
```bash
git diff HEAD
```

If the user specifies a file, read that file.

If the user pastes a diff or snippet, review that directly.

## Output Format

Structure your review as:

### Summary
One sentence: overall verdict (looks good / minor issues / needs work).

### Issues
For each issue use this format:
- **[SEVERITY]** `file:line` — description. _Suggested fix (one line)._

Severity levels: `CRITICAL` | `WARNING` | `SUGGESTION`

Only report real issues. Do not pad with praise or filler.

### Verdict
- `APPROVE` — no critical or warning issues
- `REQUEST CHANGES` — one or more WARNING or CRITICAL issues found

## Steps to Follow

1. Determine the review target (git diff, specific file, or pasted snippet).
2. If reviewing git diff, run `git diff HEAD` via Bash.
3. Read relevant files as needed to understand context (types, imports, related logic).
4. Apply the checklist above.
5. Output the structured review.
6. If the user asks to fix an issue, make the edit and confirm.
