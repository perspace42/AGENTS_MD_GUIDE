# 08 — Error Handling

This document defines how a coding agent should detect, handle, communicate, and recover from errors — both errors in the code it writes, and errors it encounters while working.

---

## 1. General Philosophy

- **Errors should never be silent.** Every error must either be logged, surfaced to the user, or explicitly handled. Swallowing errors makes debugging exponentially harder.
- **Fail fast, fail clearly.** When something goes wrong, stop immediately and produce a clear, specific error message. Do not continue executing in an uncertain state.
- **Recover gracefully where possible.** For expected, recoverable errors (e.g., a missing file, a failed network request), provide a fallback or a helpful error message. For unexpected errors, let the exception propagate.

---

## 2. Error Handling in Code

### The Three Categories of Errors

| Category | Description | How to Handle |
|----------|-------------|---------------|
| **Expected errors** | Errors you anticipate (e.g., file not found, invalid input, network timeout) | Catch specifically and handle or report |
| **Logic errors** | Bugs in the code that produce wrong results without raising an exception | Prevent with validation; expose with tests |
| **Unexpected errors** | Errors you did not anticipate (e.g., out-of-memory, corrupted data) | Let propagate; log at the top level |

### Catch Specific Exceptions — Not Everything

```python
# BAD — catches everything, hides bugs
try:
    result = process(data)
except Exception:
    pass

# GOOD — catches only what you expect and handles it meaningfully
try:
    result = process(data)
except FileNotFoundError as e:
    logger.error(f"Input file not found: {e}")
    raise
except ValueError as e:
    logger.warning(f"Invalid data format: {e}")
    return None
```

### Never Use Bare `except` Clauses

```python
# BAD
try:
    do_something()
except:
    pass

# GOOD
try:
    do_something()
except SpecificError as e:
    handle(e)
```

### Always Include Error Context in Messages

Error messages must answer: **What failed? Where? Why?**

```python
# BAD
raise ValueError("Invalid input")

# GOOD
raise ValueError(
    f"Invalid discount rate '{rate}' for item '{item_id}'. "
    f"Rate must be between 0.0 and 1.0."
)
```

### Use Finally for Cleanup

Always release resources (file handles, database connections, locks) in a `finally` block:

```python
file = open(path, "r")
try:
    data = file.read()
finally:
    file.close()  # Always runs, even if an exception occurs
```

Or better — use context managers:

```python
with open(path, "r") as file:
    data = file.read()  # File is automatically closed
```

---

## 3. Input Validation

Validate all external input **before** processing it. External input includes: user input, API responses, file contents, environment variables, and function arguments from untrusted callers.

```python
def set_timeout(seconds: int) -> None:
    """Set the request timeout in seconds."""
    if not isinstance(seconds, int):
        raise TypeError(f"seconds must be an int, got {type(seconds).__name__}")
    if seconds <= 0:
        raise ValueError(f"seconds must be positive, got {seconds}")
    _timeout = seconds
```

---

## 4. Logging

### What to Log

| Level | When to Use |
|-------|------------|
| `DEBUG` | Detailed internal state information useful during development |
| `INFO` | Normal operational events (service started, job completed) |
| `WARNING` | Something unexpected happened but the program can continue |
| `ERROR` | An operation failed; action is required |
| `CRITICAL` | A severe failure; the application may not be able to continue |

### What Not to Log

- Passwords, tokens, API keys, or any secrets.
- Personally identifiable information (PII) such as email addresses, full names, or government IDs.
- Raw request/response bodies from external APIs (they may contain sensitive data).

### Log Format

Include: timestamp, level, source module, and message.

```
2026-03-16 14:22:01 [ERROR] user_service: Failed to create user. Email 'x@' is invalid.
```

---

## 5. How a Coding Agent Should Handle Errors It Encounters

### When a Tool or Command Fails

1. **Read the error message carefully.** Do not retry blindly.
2. **Diagnose before acting.** Identify the most likely cause.
3. **Report to the user.** Explain what failed and what you believe caused it.
4. **Propose a fix or ask for help.** Either attempt a resolution or ask the user what to do.

**Example response when a command fails:**

> The command `pytest tests/` failed with the following error:
>
> ```
> ModuleNotFoundError: No module named 'requests'
> ```
>
> This means the `requests` package is not installed in the current environment. You can fix this by running:
>
> ```bash
> pip install requests
> ```
>
> Alternatively, if `requests` should already be installed, check that you are using the correct virtual environment.

### When You Cannot Proceed Without Information

Stop, explain exactly what is blocking you, and ask a specific question:

> I cannot continue because I don't know where the database connection string is configured. Is it in an environment variable, a config file, or hardcoded somewhere? Please point me to where this is defined.

---

## 6. Communicating Errors to the User

| Situation | What to Say |
|-----------|------------|
| A tool or command failed | Quote the exact error, diagnose it, propose a fix |
| Your code has a bug | Acknowledge it directly, explain the root cause, show the fix |
| Something is ambiguous | Ask a specific question — do not guess and produce broken code |
| An error is outside your control | Explain what happened and what the user can do about it |

Never say "it should work" or "try it and see." Be specific about what you know and what you don't.

---

## See Also

- [Restrictions](../05_restrictions.md)
- [Testing](../09_testing.md)
- [Response Format](../03_response_format.md)
