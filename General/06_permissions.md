# 06 — Permissions

This document defines the explicit rules for what a coding agent should and should not do, organized by category. Unlike [Restrictions](../05_restrictions.md) (which are hard limits), permissions describe preferred behaviors, encouraged practices, and things to avoid under normal conditions.

> Think of Restrictions as **"never do this"** and Permissions as **"prefer this / avoid that."**

---

## 1. File Operations

| ✅ DO THIS | ❌ NOT THIS |
|-----------|------------|
| Create new files only when explicitly asked | Create files speculatively "just in case" |
| Write to files within the current project directory | Write to files outside the project root |
| Back up files before making substantial overwrites | Silently overwrite existing content |
| Use relative paths in code and config | Hardcode absolute paths that won't work on other machines |
| Name new files following the project's existing conventions | Invent new naming conventions mid-project |
| Delete files only after explicit user confirmation | Remove files as part of "cleanup" without asking |

---

## 2. Code Writing

| ✅ DO THIS | ❌ NOT THIS |
|-----------|------------|
| Follow the existing code style in the file you are editing | Reformat the entire file when only changing a few lines |
| Match the indentation, naming, and comment style already in the file | Apply personal style preferences over the project's conventions |
| Add error handling to new code paths | Leave new code without any error handling |
| Use typed function signatures | Write untyped functions in typed codebases |
| Write the minimum code necessary to solve the problem | Add extra features, refactors, or "improvements" that were not asked for |
| Reuse existing helper functions and utilities | Duplicate logic that already exists in the codebase |

---

## 3. Dependencies and Packages

| ✅ DO THIS | ❌ NOT THIS |
|-----------|------------|
| Ask before adding any new library or package | Silently add `import` statements for packages not yet installed |
| Use packages that are already in the project's dependency file | Introduce competing libraries (e.g., adding `axios` to a project that uses `fetch`) |
| Specify version constraints when adding new dependencies | Add dependencies with no version pinning |
| Prefer standard library solutions when sufficient | Reach for third-party packages for problems the standard library can solve |

---

## 4. Testing

| ✅ DO THIS | ❌ NOT THIS |
|-----------|------------|
| Write tests for new functions and bug fixes | Skip tests because "it's a small change" |
| Run existing tests after making changes and report the results | Assume tests still pass after a change |
| Test edge cases: empty input, null/None, boundary values | Only test the "happy path" |
| Place test files where the project already puts them | Create a new test folder structure when one already exists |
| Name tests clearly: `aTaskNameTest` | Name tests `test1`, `test2`, `test_misc` |

---

## 5. Version Control

| ✅ DO THIS | ❌ NOT THIS |
|-----------|------------|
| Suggest a commit message after completing a set of changes | Create commits automatically without asking |
| Describe what scope the commit covers (e.g., `feat:`, `fix:`, `docs:`) | Write vague commit messages like "updates" |
| Keep commits focused on one logical change | Bundle unrelated changes into a single commit |
| Leave pushing/merging to the user | Push branches or merge pull requests automatically |

---

## 6. Communication

| ✅ DO THIS | ❌ NOT THIS |
|-----------|------------|
| Ask for clarification before starting a task with multiple valid interpretations | Guess and build the wrong thing |
| Batch all open questions into one message | Ask one question, start work, then ask another |
| State assumptions explicitly when proceeding without asking | proceed on guesses without flagging them |
| Give short, direct answers to short, direct questions | Write long explanations for trivial responses |
| Admit when you are unsure about something | Confidently present uncertain information as fact |
| Tell the user when you cannot complete a task | Silently produce incomplete output |

---

## 7. Refactoring

| ✅ DO THIS | ❌ NOT THIS |
|-----------|------------|
| Refactor only when asked, or when the refactor is necessary for correctness | Refactor code that is not related to the current task |
| Alert the user before making large structural changes | Silently restructure entire modules |
| Keep refactors in a separate commit from feature/bug changes | Mix refactoring and feature work in one diff |
| Preserve existing behavior when refactoring | Change behavior while claiming to only refactor |

---

## 8. Security

| ✅ DO THIS | ❌ NOT THIS |
|-----------|------------|
| Use environment variables for secrets and credentials | Hardcode API keys, passwords, or tokens |
| Sanitize and validate all external inputs | Trust external input without validation |
| Use parameterized queries for database operations | Concatenate user input into SQL strings |
| Follow HTTPS/TLS for all external HTTP calls | Make plaintext HTTP calls to external services |
| Flag potential security concerns to the user | Silently implement insecure patterns |

---

## See Also

- [Restrictions](../05_restrictions.md)
- [Coding Style](../01_coding_style.md)
- [Error Handling](../08_error_handling.md)
