# 09 — Testing

This document defines the testing expectations for a coding agent. All code produced or modified by an agent should be verified before being reported as complete.

---

## 1. Core Testing Principle

> **Never claim a task is done until you have verified the changes work.**

Verification means running existing tests, writing new tests, or performing explicit manual checks — not just reading the code and deciding it looks correct.

---

## 2. Types of Tests

| Type | What It Tests | When to Use |
|------|--------------|-------------|
| **Unit test** | A single function or method in isolation | Every new or modified function |
| **Integration test** | How multiple components work together | When changing interfaces between modules |
| **End-to-end test** | The full user-facing flow | When modifying critical user paths |
| **Manual test** | Visually or interactively checking behavior | When automated tests cannot cover the scenario |
| **Regression test** | That a previous bug does not return | Every bug fix |

---

## 3. When to Write Tests

| Situation | Action |
|-----------|--------|
| Adding a new function or class | Write a unit test |
| Fixing a bug | Write a regression test that would have caught the bug |
| Changing a function's behavior | Update the existing test and add new cases |
| Adding a new API endpoint | Write an integration test |
| Refactoring (no behavior change) | Ensure all existing tests still pass; add none for the refactor itself |

---

## 4. Test Naming Convention

Use the pattern: `test_[function_name]_[scenario]_[expected_result]`

| Good Test Name | What It Communicates |
|---------------|---------------------|
| `test_calculate_discount_valid_rate_returns_reduced_price` | Tests the happy path |
| `test_calculate_discount_zero_rate_returns_original_price` | Tests a boundary value |
| `test_calculate_discount_negative_rate_raises_value_error` | Tests invalid input |
| `test_calculate_discount_rate_above_one_raises_value_error` | Tests upper boundary |

---

## 5. Test Structure — Arrange / Act / Assert

Every test should follow this three-part structure:

```python
def test_calculate_discount_valid_rate_returns_reduced_price():
    # Arrange — set up inputs and expected values
    price = 100.0
    rate = 0.25
    expected = 75.0

    # Act — call the function under test
    result = calculate_discount(price, rate)

    # Assert — verify the result
    assert result == expected
```

Do not combine multiple unrelated assertions in a single test. One test → one behavior.

---

## 6. Edge Cases to Always Test

For any function that accepts input, test the following:

| Scenario | Description |
|----------|-------------|
| **Empty input** | Empty string, empty list, empty dict |
| **None / null** | What happens if `None` is passed |
| **Boundary values** | Minimum and maximum of a valid range |
| **Just outside boundaries** | One step below minimum, one step above maximum |
| **Type errors** | Wrong type passed (e.g., string instead of int) |
| **Large input** | Verify performance doesn't degrade unexpectedly |

---

## 7. Running Tests

### Before Reporting a Task Complete

Run the full test suite and report the results to the user:

```bash
# Python (pytest)
pytest tests/ -v

# JavaScript / TypeScript (Jest or Vitest)
npm test

# Go
go test ./...
```

### Reporting Test Results

After running tests, always tell the user:

- How many tests ran.
- How many passed / failed / were skipped.
- The exact error output for any failing test.

**Example:**

> All 42 tests pass. Here's the output:
>
> ```
> ============================= test session results ==============================
> 42 passed in 1.23s
> ```

If a test fails:

> 1 test is failing after my change. Here's the error:
>
> ```
> FAILED tests/unit/test_pricing.py::test_calculate_discount_valid_rate - AssertionError: assert 80.0 == 75.0
> ```
>
> I'll investigate and fix this before continuing.

---

## 8. Manual Test Steps

When automated tests cannot cover something (e.g., a UI flow or a CLI command), describe the manual test steps clearly so the user can verify the behavior themselves.

Use this format:

```
### Manual Test: [What you are testing]

**Preconditions:** [Any setup required before testing]

1. [Action the user takes]
2. [Next action]
3. ...

**Expected Result:** [Exactly what should happen]
**Actual Result:** [Leave blank for the user to fill in, or fill in if you ran it yourself]
```

**Example:**

```
### Manual Test: Login with invalid credentials

**Preconditions:** The development server is running on http://localhost:3000

1. Navigate to http://localhost:3000/login
2. Enter the username `testuser` and the password `wrongpassword`
3. Click the "Log In" button

**Expected Result:** An error message reading "Invalid username or password" appears below the form. The user is not redirected.
```

---

## 9. What Not to Do

| ❌ DO NOT | Reason |
|----------|--------|
| Skip tests because "the change is small" | Small changes can break large things |
| Write tests that always pass regardless of logic | Tests should actually catch failures |
| Delete failing tests instead of fixing them | Failing tests are information |
| Test only the happy path | Edge cases are where bugs live |
| Write a test that only checks the return type | Check the actual value |
| Report a task complete before running tests | Untested code may be broken code |

---

## See Also

- [Error Handling](../08_error_handling.md)
- [Change Summary](../04_change_summary/change_summary.md)
- [Permissions](../06_permissions.md)
