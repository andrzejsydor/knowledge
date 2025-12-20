# Markdown Template Standard

This document defines the standard template for all markdown files in this repository.

## Standard Template Structure

```markdown
---
tags:
  - Tag1
  - Tag2
  - Tag3
---

# Topic Name

[Optional: Table of Contents for longer files]

## Section 1

Content here...

## Section 2

Content here...

## Tools

[If applicable - tools related to the topic]

## Links

[Links section at the end]
```

## Formatting Rules

### 1. Frontmatter
- **Required**: All files should have YAML frontmatter with tags
- **Format**: Use YAML list format with proper indentation
- **Example**:
  ```yaml
  ---
  tags:
    - Tag1
    - Tag2
  ---
  ```

### 2. Title
- **Format**: Use single `#` for main title, not `# #`
- **Example**: `# Kafka` not `# #Kafka`

### 3. Table of Contents
- **Optional**: Add for files with 3+ main sections
- **Format**: Use markdown links to sections
- **Placement**: After the main title, before content

### 4. Sections
- **Format**: Use `##` for main sections, `###` for subsections
- **Spacing**: One blank line before and after sections

### 5. Links Section
- **Format**: Always use `# Links` (not `# #Links` or `#Link`)
- **Placement**: At the end of the document
- **Organization**: Group related links when possible

### 6. Spacing
- **Between sections**: One blank line
- **Remove**: Excessive blank lines (more than 2 consecutive)
- **Code blocks**: One blank line before and after

### 7. Tags
- Use consistent tag naming (PascalCase or lowercase)
- Include relevant tags for discoverability
- Avoid redundant tags

## Improvements Applied

The following improvements have been standardized across all markdown files:
- ✅ Consistent frontmatter format
- ✅ Standardized title format
- ✅ Consistent links section naming
- ✅ Improved spacing and readability
- ✅ Added table of contents where appropriate

