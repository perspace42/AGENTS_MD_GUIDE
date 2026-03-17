# 01 — Coding Style Conventions

This document defines the coding style conventions that a coding agent must follow when writing, editing, or reviewing code. These rules apply across all languages unless a language-specific section overrides them.

---

## 1. General Philosophy

- **Clarity over cleverness.** Code should be immediately understandable to a human reader. Avoid one-liners, obscure tricks, or over-compressed logic.
- **Consistency over preference.** When a project already has a style in place, match it — even if it differs from these defaults.
- **Explicit over implicit.** Name things clearly. Avoid abbreviations unless they are universally understood (e.g., `id`, `url`, `html`).

---

## 2. Naming Conventions

### Variables

| Convention | Rule | Example |
|-----------|------|---------|
| Casing | `camelCase` | `userProfile`, `retryCount` |
| Boolean prefix | Start with `is`, `has`, `can`, or `should` | `isValid`, `hasPermission`, `canRetry` |
| Avoid vague names | Use specific, meaningful names | `pendingOrderCount` not `data` |
| Avoid single letters | Only use single character variable names in defined math equations | `A = pi*r^2` `#A == Area, r == radius` |
| Units in name | Include unit when the type alone is ambiguous | `connectionTimeoutMs`, `fileSizeBytes` |

- Names should communicate **what the variable holds**, not how it is stored or implemented.
- Avoid generic names like `data`, `info`, `obj`, `temp`, or `val` — be specific about what the value represents.
- Boolean variables should always read like a statement that is true or false: `isLoading`, `hasWriteAccess`.

---

### Functions

| Convention | Rule | Example |
|-----------|------|---------|
| Casing | `camelCase` | `calculateDiscount()`, `sendNotification()` |
| Pattern | Verb + noun | `getUser()`, `validateEmail()`, `parseResponse()` |
| Boolean return | Read like a question | `isExpired()`, `hasChildren()`, `canDelete()` |
| Event handlers | `on` + event name | `onClick()`, `onSubmit()`, `onConnectionLost()` |
| Avoid vague verbs | Use specific action words | `archiveUser()` not `processUser()` |

- Name functions after **what they do** — a reader should understand the outcome without reading the body.
- Avoid vague verbs like `do`, `process`, or `execute`. Use specific verbs:

| Vague | Specific Alternatives |
|-------|-----------------------|
| `processUser` | `validateUser`, `saveUser`, `archiveUser` |
| `handleData` | `parseResponse`, `filterResults`, `transformPayload` |
| `executeSession` | `startSession`, `expireSession`, `refreshSession` |

---

### Classes

| Kind | Convention | Example |
|------|-----------|--------|
| Classes | `PascalCase`, noun or noun phrase | `UserProfile`, `OrderProcessor` |
| Enums | `PascalCase`, noun | `OrderStatus`, `PaymentMethod`, `LogLevel` |
| Interfaces | `PascalCase`; prefix `I` only if project requires it | `UserRepository` or `IUserService` |
| Abstract classes | `PascalCase`; prefix `Base` or `Abstract` if helpful | `BaseRepository`, `AbstractValidator` |
| Type aliases | `PascalCase` | `ApiResponse`, `UserId` |

- Class names must be **nouns or noun phrases** — never verbs or adjective-only names.
- A class name should describe **what the object is**, not what it does: `OrderProcessor` not `ProcessOrder`.

---

### Constants

- Use `SCREAMING_SNAKE_CASE` for module-level or global constants: `MAX_RETRY_COUNT`, `DEFAULT_TIMEOUT_MS`, `API_BASE_URL`.
- Constants should be named after **what they represent**, not their value — `MAX_CONNECTIONS` not `FIFTY`.
- Group related constants in an enum or a dedicated constants file rather than scattering them across modules.

---

### Files and Directories

- Use `camelCase` for Python files: `userUtils.py`, `dbConnection.py`
- Use `kebab-case` for web assets and stylesheets: `user-profile.css`, `hero-image.png`
- Use `camelCase` for utility and helper modules: `apiClient.ts`, `formatDate.ts`
- See [File Structure](../07_file_structure.md) for directory naming rules.

---

## 3. Type Annotations

All variables and functions must have explicitly defined types. Do not rely on type inference alone for public-facing code — types serve as documentation and catch errors before runtime.

### Variables

Every variable declaration must include a type annotation.

| Situation | Requirement | Example |
|-----------|------------|---------|
| Primitive value | Annotate the type | `count: int = 0` |
| Collection | Annotate at least the container(only annotate element type if list only contains one type) | `names: list[str] = []` |
| Optional value | Use `Optional` or union with `None` | `userId: str \| None = None` |
| Boolean flag | Annotate as `bool` | `isActive: bool = True` |
| Constants | Annotate the type | `MAX_RETRIES: int = 3` |

```python
# REQUIRED
userName: str = "Scott"
retryCount: int = 0
isActive: bool = True
allowedRoles: list[str] = ["admin", "editor"]
currentUser: User | None = None
```

```typescript
const userName: string = "Scott";
const retryCount: number = 0;
const isActive: boolean = true;
const allowedRoles: string[] = ["admin", "editor"];
let currentUser: User | null = null;
```

### Functions

Every function must declare a type for each parameter and an explicit return type.

| Situation | Requirement | Example |
|-----------|------------|---------|
| Standard function | Annotate all params and return type | `def getUser(userId: str) -> User` |
| No return value | Annotate return as `None` / `void` | `def logEvent(msg: str) -> None` |
| Optional parameter | Use `Optional` or default + union | `def find(id: str, active: bool = True) -> User \| None` |
| Multiple return types | Use a union type | `-> str \| None` |
| Generator | Annotate as `Generator` or `Iterator` | `-> Generator[str, None, None]` |

```python
# REQUIRED
def calculateDiscount(price: float, rate: float) -> float:
    return price * (1 - rate)

def logEvent(message: str, level: str = "INFO") -> None:
    print(f"[{level}] {message}")

def findUser(userId: str) -> User | None:
    return userStore.get(userId)
```

```typescript
function calculateDiscount(price: number, rate: number): number {
    return price * (1 - rate);
}

function logEvent(message: string, level: string = "INFO"): void {
    console.log(`[${level}] ${message}`);
}

function findUser(userId: string): User | null {
    return userStore.get(userId) ?? null;
}
```

### Rules

- **Never use `Any` / `any` / `object` as a type** unless there is no alternative and the reason is documented with a comment.
- **Never leave a function return type implicit.** Even if the language can infer it, declare it explicitly.
- **Never leave a function parameter untyped.** Every parameter must have a declared type.
- Type annotations are not optional for private or internal functions — they are required everywhere.

```python
# BAD — no types
def getUser(id):
    return db.find(id)

# GOOD — fully typed
def getUser(userId: str) -> User | None:
    return db.find(userId)
```

---

## 4. Comment Style

### When to Write a Comment

Write a comment when:
- The **why** behind a decision is not obvious from the code itself.
- A workaround or hack is present that future developers need to understand.
- A complex algorithm or formula is used.
- An external dependency or API has unexpected behavior.
- A comment is needed to explain the purpose of a function or class.

Do **not** write a comment when:
- The code already reads like plain English (e.g., `user.save()` does not need `# save the user`).
- The comment would just repeat the function or variable name.

### Types of Comments

Single Line Comment: Use a single line comment to explain a variable, or to denote a section of code.
```python
#Constants (single line comment)
SITE_URL: str ="https://my.site.com"
```
Multi Line Comment: Use a multi line comment to explain a block of code.

```python
collection: list[str] = ["item1", "item2", "item3"]
'''
(multi line comment)
Print all items in the collection 
'''
for item in collection:
    print(item)
```

### Block / Section Comments

Use a section comment to separate logical groups within a long file or function:
- section comment format:```python #<Section Name> Section```

```python
#<Database Initialization> Section
```

### TODO and FIXME Comments

Always include your name/handle and a brief description:
- TODO comments are only used for temporary notes and should be removed once the task is completed.
- FIXME comments are used for known bugs that need to be fixed, only remove once bug is fixed.

```python
# TODO(Scott): Replace with async version once the API supports it
# FIXME(Scott): This breaks on empty input — needs guard clause
```

---

## 4. Docstrings

Docstrings should be placed at the beginning of the file immediately after the import statements.

### Required Docstring Fields

| Field | Required | Notes |
|-------|----------|-------|
| Author | Yes | Your name/handle |
| Date | Yes | Date of last revision |
| Purpose | Yes | Multiple sentences. What is the purpose of this file? |
| Variable List | No | Name, type, and description for each importantvariable |


### Python (Google Style)

```python
'''
Author: Scott
Date: 2026-03-17
Purpose: This file is used to calculate the discounted price after applying a percentage rate.
Variable List:
    PRICE: The original price of the item, in dollars.
    RATE: The discount rate as a decimal between 0.0 and 1.0.
'''
PRICE: float = 100.0
RATE: float = 0.2
def calculate_discount() -> float:
    if not 0.0 <= RATE <= 1.0:
        raise ValueError(f"Rate must be between 0 and 1, got {RATE}")
    return PRICE * (1 - RATE)
    else:
        return PRICE
```

## 5. Function Design

- **One function, one responsibility.** If a function does two things, split it.
- **Keep functions short.** Aim for fewer than 30 lines. If longer, extract helper functions.
- **Avoid deep nesting.** Use early returns (guard clauses) to reduce indentation levels.
- **Avoid magic numbers.** Extract numbers into named constants with clear names.

```python
# BAD
if user.age > 18 and user.score > 750:
    ...

# GOOD
MIN_AGE = 18
MIN_CREDIT_SCORE = 750

if user.age > MIN_AGE and user.score > MIN_CREDIT_SCORE:
    ...
```

---

## 6. Formatting Rules

- **Indentation:** 4 spaces for Python; 2 spaces for JavaScript/TypeScript/JSON/YAML; tabs for Go.
- **Line length:** Maximum 100 characters. Break earlier if it aids readability.
- **Trailing whitespace:** Never leave trailing spaces at the end of a line.
- **End of file:** Every file must end with a single newline character.
- **Blank lines:** Use one blank line between methods; two blank lines between top-level functions or classes in Python.

---

## 7. Language-Specific Notes

### Python
- Do not split function parameters across multiple lines.
- Use type hints for all public functions.
- Prefer f-strings over `.format()` or `%` formatting.

### JavaScript / TypeScript
- Prefer `const` over `let`; never use `var`.
- Use `===` (strict equality) instead of `==`.
- Use `async/await` over raw Promises for async flows.
- Enable TypeScript strict mode in `tsconfig.json`.
- Use TypeScript instead of JavaScript wherever possible.

### SQL
- Use uppercase for SQL keywords: `SELECT`, `FROM`, `WHERE`.
- Alias tables with meaningful short names: `u` for `users`, `o` for `orders`.
- Comment SQL queries with a brief description of what the query does.

---

## See Also

- [README Format](../02_readme_format.md)
- [File Structure](../07_file_structure.md)
- [Permissions](../06_permissions.md)
