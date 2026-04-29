---
trigger: "*.md"
description: Markdown standards, linting, and stylistic requirements.
---

# Markdown Directives

You must strictly adhere to these standards at all times to ensure
consistent and high-quality Markdown documentation.

## Markdown Formatting & Style

- **Syntax / Style:** Strictly follow the
  [Google Developer Documentation Style Guide](https://developers.google.com/style)
  for textual formatting and guidelines when writing developer documentation.
- **Formatting:** Use `prettier` to format Markdown files automatically,
  enforcing consistent line wrapping, spacing, and structural standard practices.
- **Linting:** Use `markdownlint` to statically check Markdown files for common
  syntax and style issues. Strictly resolve all lint warnings before finalizing
  documentation.
- **Structure:** Clearly document any complex examples using properly fenced
  code blocks with specific language tags for syntax highlighting (e.g.,
  ` ```python `).
