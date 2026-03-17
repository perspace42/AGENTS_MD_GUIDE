# AGENTS_MD_GUIDE

A standard set of instructions for Coding Agents. Ensuring that the output conforms to the written standard regardless of which agent is used.

> **How to use this guide:** Each section below links to a dedicated Markdown file covering one topic. Read each file before configuring or prompting a coding agent. You can copy, edit, or extend any individual file to fit your project's needs without breaking the rest of the guide.

---

## Guide Index

| # | File | Description |
|---|------|-------------|
| 1 | [Coding Style](Claude%20Sonnet%204.6/01_Coding_Style.md) | Comment style, function naming conventions, and docstring format |
| 2 | [README Format](Claude%20Sonnet%204.6/02_ReadMe_Format.md) | Required README sections, headers, fields, and a copy-paste template |
| 3 | [Response Format](Claude%20Sonnet%204.6/03_Response_Format.md) | How agents should open, structure, and close their responses |
| 4 | [Change Summary](Claude%20Sonnet%204.6/04_Summary/ImplementationLog.md) | How to describe and log code changes made during a session |
| 5 | [Restrictions](Claude%20Sonnet%204.6/05_Restrictions.md) | Hard limits — actions an agent must never take |
| 6 | [Permissions](Claude%20Sonnet%204.6/06_Permissions.md) | Explicit DO THIS / NOT THIS rules organized by category |
| 7 | [File Structure](Claude%20Sonnet%204.6/07_File_Structure.md) | Project folder layout, file naming, and organization conventions |
| 8 | [Error Handling](Claude%20Sonnet%204.6/08_Errors.md) | How to detect, handle, report, and recover from errors |
| 9 | [Testing](Claude%20Sonnet%204.6/09_Testing.md) | Testing expectations, verification steps, and how to report results |

---

## Folder Layout

```
AGENTS_MD_GUIDE/
├── README.md                          ← You are here (index lives here)
└── Claude Sonnet 4.6/
    ├── 01_coding_style.md
    ├── 02_readme_format.md
    ├── 03_response_format.md
    ├── 04_change_summary/
    │   └── change_summary.md
    ├── 05_restrictions.md
    ├── 06_permissions.md
    ├── 07_file_structure.md
    ├── 08_error_handling.md
    └── 09_testing.md
```

---

## Quick Start

1. Clone or download this repository.
2. Open `README.md` (this file) to get an overview of all guide sections.
3. Navigate to the section most relevant to your current task.
4. Copy any template sections into your project's agent configuration or system prompt.
5. Modify individual files to match your project's conventions — the structure is intentionally flexible.

---

## License

See [LICENSE](LICENSE) for details.
