# 04 — Change Summary

This document defines how a coding agent should describe and log the changes it makes during a session. A good change summary helps the user understand exactly what was changed, why, and what the impact is — without having to read every line of diff themselves.

---

## 1. When to Write a Change Summary

Write a change summary:
- After completing any set of code changes.
- After modifying more than one file.
- At the end of a session involving multiple tasks.
- When asked explicitly by the user.

A change summary is **not** the same as a commit message — it is a human-readable explanation written for the user reviewing the work, not for version control.

---

## 2. Standard Change Summary Format

Use the following structure for all change summaries:

```
## Summary of Changes

### What Changed
[A short description of the change — one to three sentences. Focus on the outcome, not the implementation details.]

### Why
[The reason or motivation for the change. Reference the user's request, a bug, or a constraint.]

### Files Affected
- `path/to/file.py` — [what changed in this file]
- `path/to/other_file.ts` — [what changed in this file]

### Impact
[Describe the effect of this change. Does it break anything? Does it require migration steps? Are there known limitations?]

### Next Steps (Optional)
[Any follow-on work that the user or agent should do. Leave blank if none.]
```

---

## 3. Filled-In Example

```
## Summary of Changes

### What Changed
Added input validation to the `create_user()` function in `user_service.py`.
The function now raises a `ValueError` for invalid email addresses and empty usernames.

### Why
The API was accepting malformed user data that later caused database constraint violations.
This validation prevents invalid records from being written at the service layer.

### Files Affected
- `services/user_service.py` — Added `_validate_user_input()` helper and integrated it into `create_user()`
- `tests/test_user_service.py` — Added 4 new test cases covering invalid email, empty name, None input, and valid input

### Impact
No breaking changes for callers that already pass valid data.
Callers passing invalid data will now receive a `ValueError` instead of a database error — this is intentional and should be handled by the API layer.

### Next Steps
The API route handler in `routes/user.py` should be updated to catch `ValueError` and return a proper 400 response.
```

---

## 4. Multi-Session Change Log

When multiple sessions have each produced their own summaries, collect them in a log file within this folder. Name the file using the date and a short description:

```
04_change_summary/
├── change_summary.md        ← This guide file
├── 2026-03-16_add-validation.md
├── 2026-03-17_refactor-auth.md
└── 2026-03-20_fix-timeout-bug.md
```

Each log file uses the same format as the template above.

---

## 5. Dos and Don'ts

| DO | DO NOT |
|----|--------|
| Name every file that was modified | Say "several files were updated" |
| Explain the reason for the change | Just list what changed with no context |
| Note any breaking changes explicitly | Bury breaking changes in the middle of a paragraph |
| Keep the summary focused on this session's work | Re-summarize previous sessions |
| Write in plain, direct language | Use jargon or overly technical descriptions |
| State "No breaking changes" if none | Leave the Impact section blank |

---

## 6. Minimal Change Summary (Single File, Simple Change)

For small, isolated changes, a shorter format is acceptable:

```
Changed `MAX_RETRY_COUNT` from 3 to 5 in `config/settings.py`.
Reason: The previous value caused timeouts on slow networks during load tests.
No other files affected. No breaking changes.
```

Do not over-engineer the summary for trivial changes — match the depth of the summary to the complexity of the change.

---

## See Also

- [Response Format](../03_response_format.md)
- [Restrictions](../05_restrictions.md)
- [Testing](../09_testing.md)
