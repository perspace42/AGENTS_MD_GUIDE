# 02 — README Format

This document defines the standard format for `README.md` files produced or modified by a coding agent. A README is the front door of a project — it should be clear, well-structured, and immediately useful to a new reader.

---

## 1. Required Sections

Every README must include these sections, in this order:

| Order | Section Header | Purpose |
|-------|---------------|---------|
| 1 | `# Project Name` | The name of the project — matches the repo/folder name |
| 2 | `## Description` | What the project does and why it exists |
| 3 | `## Requirements` | Software, tools, and versions needed to run the project |
| 4 | `## Config` | Any environment variables, config files, or settings |
| 5 | `## Installation` | Step-by-step setup instructions |
| 6 | `## Usage` | How to run the project, with at least one concrete example |
| 7 | `## License` | The license type and a link to the LICENSE file |

---

## 2. Optional Sections

Include these sections when they apply:

| Section Header | When to Include |
|---------------|----------------|
| `## Features` | When the project has multiple distinct capabilities worth listing |
| `## Screenshots` | When the project has a UI or visual output |
| `## API Reference` | When the project exposes a public API |
| `## Contributing` | When the project accepts external contributions |
| `## Changelog` | When version history should be visible in the README |
| `## Roadmap` | When planned features add important context |
| `## FAQ` | When common questions are likely and worth pre-answering |
| `## Acknowledgements` | When crediting libraries, contributors, or inspiration |

---

## 3. Formatting Rules

- Use a **single `#` H1 header** for the project name only. All other top-level sections are `##`.
- Sub-sections within a section use `###`.
- Use **fenced code blocks** (triple backticks with a language tag) for all commands and code.
- Use **numbered lists** for sequential steps (installation, usage).
- Use **bullet lists** for non-sequential items (features, requirements).
- Add a blank line between each section for readability.
- Do **not** use raw HTML unless absolutely necessary (e.g., for image sizing).
- Keep line length soft-wrapped — do not hard-wrap at 80 characters in READMEs.

---

## 4. Badges (Optional but Encouraged)

Place badges immediately below the project name if relevant:

```markdown
# My Project

![Build Status](https://img.shields.io/github/actions/workflow/status/user/repo/ci.yml)
![License](https://img.shields.io/github/license/user/repo)
![Version](https://img.shields.io/github/v/release/user/repo)
```

---

## 5. Code Block Language Tags

Always specify the language in fenced code blocks:

```markdown
```bash
npm install
```

```python
python main.py --verbose
```

```json
{
  "port": 8080
}
```
```

---

## 6. Copy-Paste README Template

Use the following template as a starting point. Replace all `[placeholder]` values.

```markdown
# [Project Name]

[One-sentence description of what the project does and who it is for.]

---

## Description

[Two to four sentences expanding on what the project does. Explain the problem it solves
and why someone would use it. Avoid technical jargon in this section.]

---

## Requirements

- [Runtime or language, e.g., Python 3.10+]
- [Package manager, e.g., pip or npm]
- [Any external service or tool, e.g., PostgreSQL 14+]

---

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/[username]/[repo-name].git
   cd [repo-name]
   ```

2. Install dependencies:
   ```bash
   [install command, e.g., pip install -r requirements.txt]
   ```

3. [Any additional setup steps, e.g., database migration, env file setup]

---

## Usage

[Describe how to start or use the project. Be specific.]

```bash
[example command]
```

**Example output:**

```
[expected output or result]
```

---

## Configuration

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `[VAR_NAME]` | Yes / No | `[default]` | [What this controls] |

Copy `.env.example` to `.env` and fill in the required values:

```bash
cp .env.example .env
```

---

## License

This project is licensed under the [LICENSE TYPE] License. See [LICENSE](LICENSE) for details.
```

---

## 7. Common Mistakes to Avoid

- **Do not** start the README with a badge block before the project name.
- **Do not** use vague descriptions like "This is a cool project." Be specific.
- **Do not** skip the Installation section. Even simple projects need setup instructions.
- **Do not** put a wall of text in the Description. Use short descriptive paragraphs.
- **Do not** leave placeholder text (e.g., `[Project Name]`) in the final file.
- **Do not** add the word "README" as a section header — the file name makes it obvious.

---

## See Also

- [Coding Style](../01_coding_style.md)
- [File Structure](../07_file_structure.md)
- [Change Summary](../04_change_summary/change_summary.md)
