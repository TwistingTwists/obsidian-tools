---
name: obsidian-bases-new-view
description: Add a new filtered view to an Obsidian Bases .base file | Use when user asks to add/create a view for filtering notes
---

# Create Obsidian Bases View Skill

**Note:** Changes are made to the `.base` file in the vault root, not in the skill directory.

## Workflow

### 1. Read Official Documentation
**Required before writing any YAML**

Read: https://help.obsidian.md/bases/syntax

Extract key rules:
- `filters:` can have ONLY ONE of `and`, `or`, or `not` at each level
- To combine logic, nest inside list items
- ✅ Valid: `and:` with `- or:` nested
- ❌ Invalid: `and:` AND `or:` at same level

### 2. Read Existing Base File
- Read current `.base` file structure
- Note indentation style (2 spaces)
- Observe existing filter patterns

### 3. Design the View
- **Name**: Descriptive, use hyphens (e.g., `sync-engines`)
- **Filters**: Tags, dates, folders, or combinations
- **Sort**: Property + direction (ASC/DESC)
- **Order**: Column display order (optional)

### 4. Write New View
Append to `views:` list with proper structure:

```yaml
  - type: table
    name: [view-name]
    filters:
      [and|or|not]:
        - [filter-expression-1]
        - [filter-expression-2]
    sort:
      - property: [file.name|file.mtime|file.ctime]
        direction: [ASC|DESC]
```

**Common filter expressions:**
- `file.hasTag("tag-name")`
- `file.mtime >= "YYYY-MM-DD"`
- `file.ctime >= "YYYY-MM-DD"`
- `file.name.startsWith("prefix")`
- `file.inFolder("folder/path")`
- `file.ext == "md"`

### 5. Validate Syntax
- [ ] Check indentation (2 spaces)
- [ ] Verify no duplicate keys at same level
- [ ] Ensure proper YAML list syntax with `-`
- [ ] Only one filter key (and/or/not) at each level

### 6. User Verification
**ASK USER:** "Please open the `.base` file in Obsidian and verify the view works. Check for parse errors."

Common errors:
- "Unable to parse your base file" → Fix YAML syntax
- "filters may only have one of and, or, not keys" → Restructure nested logic
- View appears but empty → Check filter expressions match actual tags

## Example: Adding a Tag-Based View

```yaml
views:
  # ... existing views ...
  - type: table
    name: my-new-view
    filters:
      or:
        - file.hasTag("tag-a")
        - file.hasTag("tag-b")
    sort:
      - property: file.mtime
        direction: DESC
```

## Common Errors to Avoid

**Mixed and/or at same level** ❌
```yaml
filters:
  and:
    - condition1
  or:  # ERROR!
    - condition2
```

**Wrong indentation** ❌
```yaml
  - type: table  # Should be 2 spaces from views:
```

**Missing list dashes** ❌
```yaml
filters:
  and:
    file.hasTag("x")  # Missing `- `
```

## Filter Expression Reference

| Expression | Purpose |
|------------|---------|
| `file.hasTag("tag")` | Has specific tag |
| `file.mtime >= "2025-01-01"` | Modified after date |
| `file.ctime >= "2025-01-01"` | Created after date |
| `file.name.startsWith("2025")` | Name prefix match |
| `file.inFolder("folder")` | In specific folder |
| `file.ext == "md"` | File extension |

---
*Skill: Obsidian Bases New View | Filtered table views | YAML syntax*
