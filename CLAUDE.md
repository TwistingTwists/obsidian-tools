# CLAUDE.md - Graph Mastery Project Cfg

## Legend
| Symbol | Meaning | | Abbrev | Meaning |
|--------|---------|---|--------|---------|
| → | leads to | | cfg | configuration |
| & | and/with | | docs | documentation |
| > | greater than | | ops | operations |

---

## Project Context

**Focus:** Graph problems mastery (DFS, BFS, Dijkstra, Union-Find, Topological Sort)
**Goal:** 50+ LeetCode problems + interview readiness
**Learning Mode:** Mistake documentation → Pattern extraction → Problem progression

---

## Progress File Handling

### Large File Strategy
```yaml
Rule: Progress files may be large (>3KB) → use section-specific reads
Pattern: "Graph Mastery - Progress.md" | "Daily worksheet for graphs.md"
Action:
  1. Use `md-overview` first to get document structure
  2. Extract target section line numbers
  3. Read specific section using Read(offset, limit)
```

### Markdown Overview Tool
```yaml
Purpose: Fast structure extraction without full file read
Usage: md-overview data_dir/file.md → returns [H1:L1], [H2:L3], etc.
Benefits:
  - Identifies all headings & positions
  - Enables targeted section reads
  - Reduces token usage on large docs
  - Maps progress sections precisely
```

**Example Workflow:**
```
User: "What's my LC 797 status?"
→ md-overview "Graph Mastery - Progress.md"
→ Find [H2:L42] "LC 797 - All Paths" section
→ Read(file, offset=42, limit=8) for just that section
→ Respond with specific progress
```

---

## File Structure Reference

```
learnings/
├─ Graph Mastery - Question Path.md       [Master problem list & patterns]
├─ Graph Mastery - Progress.md            [Your current problem status]
├─ Daily worksheet for graphs.md          [Daily tracking]
├─ mistakes/                              [Documented learning gaps]
│  └─ 797_dfs_path_backtracking.md        [DFS backtracking pitfall]
│  └─ [pattern]_[topic].md
└─ .claude/
   └─ [local project configs]
```

---

## Reading Preferences

```yaml
Large Docs (>5KB):
  - First: md-overview to map structure
  - Then: Read target sections by offset/limit
  - Avoid: Full file reads unless comprehensive analysis needed

Progress Files:
  - Question Path: Full read (reference/patterns)
  - Progress Updates: Section-specific (current status only)
  - Mistakes: Full read (learning insights)
```

---

## Mistake Documentation

```yaml
Location: mistakes/[problem_id]_[topic].md
Structure: Tags | Problem | Mistake | What Went Wrong | Why | Fix | Key Lesson
Format: Frontmatter + 6 sections | Bullets>prose | ~150 words
```

**Recent entries:**
- `797_dfs_path_backtracking.md` - DAG visited set, backtracking position, reference copying

---

*Graph Mastery Project Config | md-overview for large docs | Section-specific reads*
