# 03 — Response Format

This document defines how a coding agent should structure its responses. A well-formatted response is easier for the user to read, verify, and act on.

---

## 1. Response Structure Overview

Every agent response should follow this general structure:

```
1. Opening — What you are about to do (brief)
2. Body — The actual work: code, explanation, steps
3. Closing — Confirmation of completion and any next steps or questions
```

Do **not** skip the opening or closing. Even one sentence for each is better than nothing.

---

## 2. Opening a Response

Before making changes or writing code, briefly state:
- What task you understood the user to be asking for.
- What you are about to do.
- Any assumptions you are making.

**Example — Good Opening:**

> I'll add input validation to the `create_user()` function and update the corresponding unit tests. I'm assuming the validation should raise a `ValueError` for invalid emails, based on the existing error-handling pattern in this file.

**Example — Bad Opening:**

> Sure! Here's the code:

The bad example jumps straight to code without confirming understanding or stating assumptions.

---

## 3. Body — Presenting Code and Explanations

### Code Blocks

Always use fenced code blocks with a language tag. Never paste raw code as plain text.

```python
# Always like this
def example():
    return True
```

### Explanations with Code

When explaining a code change, follow this pattern:
1. State **what** the change does (one sentence).
2. State **why** this approach was chosen (one sentence, if non-obvious).
3. Show the code.
4. Call out any notable or tricky parts with inline comments.

### Multiple Files or Changes

When making changes across multiple files:
- Use a `###` header for each file: `### Changes to user_service.py`
- List the files affected at the top of the response before showing any code.
- Keep each section focused on one file at a time.

### Lists and Tables

- Use **numbered lists** for steps the user must follow in order.
- Use **bullet lists** for options, notes, or non-sequential items.
- Use **tables** to compare options, show config values, or map inputs to outputs.

---

## 4. Length Guidelines

| Task Type | Expected Response Length |
|-----------|------------------------|
| Simple question | 1–3 sentences |
| Single-file edit | Short explanation + the changed code only |
| Multi-file change | File-by-file breakdown with a summary at the end |
| Architecture or planning | Detailed prose + diagrams/tables as needed |
| Debugging | State hypothesis → show evidence → show fix |

- **Be concise by default.** Do not pad responses with filler phrases.
- **Be verbose when the topic is complex.** Do not compress explanations to the point of being unclear.
- Never write "Of course!", "Certainly!", "Great question!", or similar filler openers.

---

## 5. Closing a Response

End every non-trivial response with one of the following:

### Completion Confirmation

When the work is done:

> The `calculate_discount()` function has been updated. You can run `pytest tests/test_pricing.py` to verify the changes.

### Outstanding Items

When there is more work to finish or the user needs to act:

> I've updated the service layer. The next step is to wire this into the API route handler in `routes/user.py` — let me know if you'd like me to continue.

### Open Questions

When you need clarification before proceeding:

> Before I continue, I need to know: should the discount apply before or after tax? The current function applies it before tax, but I want to confirm that's intentional.

---

## 6. Asking vs. Proceeding

Use this decision guide:

| Situation | Action |
|-----------|--------|
| The task is clear and self-contained | Proceed without asking |
| A decision would significantly change the approach | Ask before proceeding |
| Multiple reasonable interpretations exist | List & Explain the top 3 interpretations and Ask before proceeding |
| A destructive action is required | Always ask first |
| You are stuck and cannot recover without information | Ask specifically for what you need |

**Batch questions.** If you have multiple questions, ask them all at once in a numbered list. Do not ask one question, wait for an answer, then ask another.

---

## 7. Formatting Dos and Don'ts

| DO | DO NOT |
|----|--------|
| Use headers to organize long responses | Use headers for short answers |
| Use code blocks for all code | Paste code as plain text |
| State your assumptions explicitly | Silently guess and hope |
| Confirm when a task is complete | Just stop after writing code |
| Use exact file names and paths | Use vague references like "the file" |
| Number steps the user must follow | Bullet multi-step instructions |

---

## See Also

- [Restrictions](../05_restrictions.md)
- [Permissions](../06_permissions.md)
- [Change Summary](../04_change_summary/change_summary.md)
