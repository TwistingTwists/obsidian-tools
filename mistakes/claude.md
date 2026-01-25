# claude.md - Mistakes Folder Cfg

## Purpose
Track & highlight coding mistakes for learning | Short crisp notes | Problem → Error → Fix → Lesson

## Tool Preferences
```yaml
Search: ripgrep (rg) > grep | fd -t d/f > find
Find files: fd [pattern] | Find dirs: fd -t d [pattern]
Batch ops: ripgrep for content | fd for structure
```

## Key References
```yaml
LeetCode Practice: ../leetcode-2026-01/
Structure: problem_id_name.md
Fields: Mistake | What Went Wrong | Why | Fix | Key Lesson
```

## File Format
```yaml
Header: # Problem [ID]: [Name]
Sections:
  - Mistake (1-2 lines)
  - What Went Wrong (code example)
  - Why (root cause)
  - Fix (corrected code)
  - Key Lesson (takeaway)
Tags: Front matter → tags: [leetcode|pattern|data-structure]
Length: ~150 words per entry | Bullets > prose
```

## When to Document
- After solving problem & discovering error
- Found bug during testing
- Misunderstood concept → realized during implementation
- Edge case not handled

---
*Mistakes Notebook | Learn from errors | Short format | Ripgrep/fd preferred*
