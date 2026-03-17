# 07 — File Structure

This document defines the standard conventions for organizing files and directories within a project. These conventions help coding agents navigate the codebase predictably and helps ensure that new files always land in the right place.

---

## 1. General Principles

- **Mirror the existing structure.** When adding files to a project, match where the project already puts similar files. The conventions below are defaults — the existing project wins.
- **Keep related things together.** Group files by feature or responsibility, not by type alone.
- **Avoid deeply nested directories.** More than 4 levels deep usually signals a need to reorganize.
- **Use clear, descriptive names.** Directory and file names should immediately communicate their purpose to a new developer.

---

## 2. Naming Conventions

| Item | Convention | Examples |
|------|-----------|---------|
| Python files | `lowercase.py` | `userService.py`, `dbUtils.py` |
| JavaScript / TypeScript files | `camelCase.ts` or `PascalCase.tsx` | `apiClient.ts`, `UserProfile.tsx` |
| CSS / SCSS files | `kebab-case.css` | `user-profile.css`, `main-layout.scss` |
| Configuration files | As defined by the tool | `.eslintrc.json`, `pyproject.toml` |
| Test files | `[name]Test.py` or `[name]Test.ts` | `userServiceTest.py`, `apiClientTest.ts` |
| Documentation files | `UPPER_CASE.md` or `Title_Case.md` | `README.md`, `CONTRIBUTING.md` |
| Directories | `PascalCase` or `` | `UserService/`, `ApiClient/` |
| Asset files (images, fonts) | `lowercase with underscores` | `hero_image.png`, `roboto_bold.woff2` |

---

## 3. Standard Python Project Layout

```
my_project/
├── README.md
├── LICENSE
├── pyproject.toml          ← Project metadata and dependencies (PEP 517/518)
├── .env.example            ← Template for environment variables
├── .gitignore
│
├── src/
│   └── my_project/         ← Source package (matches project name)
│       ├── __init__.py
│       ├── main.py         ← Entry point
│       ├── config.py       ← Configuration loading
│       ├── models/         ← Data models or ORM classes
│       ├── services/       ← Business logic
│       ├── utils/          ← Shared utility functions
│       └── api/            ← API routes or controllers (if applicable)
│
├── tests/
│   ├── __init__.py
│   ├── conftest.py         ← Shared pytest fixtures
│   ├── unit/
│   └── integration/
│
├── docs/                   ← Extended documentation
└── scripts/                ← One-off scripts, migrations, build helpers
```

---

## 4. Standard JavaScript / TypeScript Project Layout (Vite / Next.js)

```
my_project/
├── README.md
├── LICENSE
├── package.json
├── tsconfig.json
├── .env.example
├── .gitignore
│
├── src/
│   ├── main.ts             ← App entry point
│   ├── App.tsx             ← Root component
│   ├── components/         ← Reusable UI components
│   ├── pages/              ← Page-level components or routes
│   ├── hooks/              ← Custom React hooks
│   ├── services/           ← API calls and external integrations
│   ├── store/              ← State management (Redux, Zustand, etc.)
│   ├── utils/              ← Shared utility functions
│   ├── types/              ← Shared TypeScript type definitions
│   └── assets/             ← Images, fonts, and static files
│
├── tests/
│   ├── unit/
│   └── integration/
│
└── public/                 ← Publicly served static files
```

---

## 5. Configuration Files

Keep configuration at the **project root** unless the tool requires otherwise. Do not scatter config files inside `src/`.

| File | Purpose |
|------|---------|
| `.env` | Local environment variables (never commit) |
| `.env.example` | Template for environment variables (always commit) |
| `.gitignore` | Files and directories excluded from version control |
| `pyproject.toml` | Python project config (replaces `setup.py`) |
| `package.json` | Node project config and dependencies |
| `tsconfig.json` | TypeScript compiler options |
| `.eslintrc.json` | JavaScript/TypeScript linting rules |
| `pytest.ini` / `setup.cfg` | Python test configuration |

---

## 6. Test File Placement

Tests should live in a `tests/` directory at the project root, **not** inside `src/`. Mirror the source structure inside `tests/` so each source file has a corresponding test file:

```
src/
└── services/
    └── user_service.py

tests/
└── unit/
    └── services/
        └── test_user_service.py
```

---

## 7. Where Not to Put Things

| What | Where NOT to put it | Where to put it instead |
|------|--------------------|-----------------------|
| Business logic | `main.py` | `services/` |
| Shared utility functions | Inline in a specific module | `utils/` |
| Environment secrets | Source code | `.env` (and `.env.example` as template) |
| Hardcoded config values | Source code | Config file or environment variable |
| Test fixtures and helpers | The source directory | `tests/conftest.py` or `tests/fixtures/` |
| Documentation longer than a README | `README.md` | `docs/` folder |

---

## 8. When to Create a New Directory

Create a new directory when:
- There are **3 or more files** that belong together logically.
- A feature is large enough to justify its own namespace.
- The project's structure suggests it (e.g., a `handlers/` folder already exists).

**Do not** create a directory for a single file. Put it in the most relevant existing directory instead.

---

## See Also

- [Coding Style](../01_coding_style.md)
- [README Format](../02_readme_format.md)
- [Permissions](../06_permissions.md)
