# Weekly Reflection Analysis - Execution Plan
## Version: 1.0 | Parallel Processing Enabled
## Location: /home/abhishek/Downloads/experiments/learnings/weekly-reflections/

---

## 🎯 Purpose
Standardized, parallelizable workflow for weekly self-reflection analysis.
This plan enables multi-task execution to complete analysis in ~2 minutes vs ~10 minutes sequential.

---

## 📁 Required Directory Structure

```
weekly-reflections/
├── execution-plan.md          (this file)
├── YYYY-WXX/                  (weekly folders)
│   ├── workflow.md            (user-approved methodology)
│   ├── analysis-report.md     (final output)
│   ├── data-raw/              (intermediate data)
│   └── insights.json          (structured findings)
└── templates/
    ├── report-template.md
    └── scorecard-template.md
```

---

## ⚡ Parallel Task Execution Matrix

### Phase 1: Data Collection (Parallel - 3 tasks)
**All tasks run simultaneously, no dependencies**

```yaml
Task 1.1: Commit History Extraction
  Command: git log --since="7 days ago" --format="%h|%s|%ai|%an" --author="$(git config user.name)"
  Output: commits-raw.txt
  Fields: hash, message, timestamp, author
  
Task 1.2: File Change Analysis
  Command: git log --since="7 days ago" --name-only --format="" | sort | uniq -c | sort -rn
  Output: files-changed.txt
  Format: count filename
  
Task 1.3: Activity Timeline
  Command: git log --since="7 days ago" --format="%ad|%aD" --date=format:"%Y-%m-%d %H:%M|%a"
  Output: timeline-raw.txt
  Format: timestamp|day
```

### Phase 2: Pattern Analysis (Parallel - 4 tasks)
**Depends on Phase 1 completion, internal parallel**

```yaml
Task 2.1: Daily Distribution
  Input: timeline-raw.txt
  Processing: Aggregate by date, count commits per day
  Output: daily-stats.json
  
Task 2.2: Time Block Analysis
  Input: timeline-raw.txt
  Processing: Categorize by hour blocks
    - Morning: 6-12
    - Afternoon: 12-18
    - Evening: 18-24
    - Night: 0-6
  Output: timeblocks.json
  
Task 2.3: File Categorization
  Input: files-changed.txt
  Processing: Regex-based category assignment
    - mistakes\/: "Learning/Mistakes"
    - \.obsidian/: "Tool Config"
    - Graph.*Progress: "Graph Learning"
    - worksheet: "Daily Tracking"
    - \.specstory/: "AI Conversations"
    - default: "General Notes"
  Output: file-categories.json
  
Task 2.4: Content Evolution
  Input: files-changed.txt
  Processing: Identify most-edited files (count > 2)
  Output: hot-files.json
```

### Phase 3: Quality Metrics (Parallel - 3 tasks)
**Depends on Phase 1, can run with Phase 2**

```yaml
Task 3.1: Commit Type Analysis
  Input: commits-raw.txt
  Processing: Categorize messages
    - "fix|refactor|update|backup": Maintenance
    - "add|create|feat|new": New Work
    - "vault backup": Automated
  Output: commit-types.json
  
Task 3.2: LC Progress Tracking
  Input: Graph Mastery - Progress.md, mistakes/*.md
  Processing:
    - Extract [x] and [ ] items
    - Parse LC numbers from mistakes
    - Calculate completion ratio
  Output: lc-progress.json
  
Task 3.3: Session Detection
  Input: timeline-raw.txt
  Processing:
    - Group consecutive commits within 90 min
    - Count sessions per day
    - Calculate average session length
  Output: sessions.json
```

### Phase 4: Report Generation (Sequential - depends on all above)
**Final synthesis, must wait for all JSON outputs**

```yaml
Task 4.1: Data Aggregation
  Inputs: All *.json files from Phases 2-3
  Processing: Merge into unified data structure
  Output: analysis-data.json
  
Task 4.2: Report Rendering
  Input: analysis-data.json + templates/report-template.md
  Processing: Template substitution
  Output: analysis-report.md
  
Task 4.3: Reflection Prompts
  Input: analysis-data.json
  Processing: Generate context-specific questions
  Output: reflection-prompts.md
```

---

## 🔧 Implementation Commands

### Quick Start (Copy-Paste Ready)

```bash
#!/bin/bash
# weekly-reflection.sh - Parallel execution

WEEK_DIR="weekly-reflections/$(date +%Y-W%V)"
mkdir -p "$WEEK_DIR/data-raw"

cd /home/abhishek/Downloads/experiments/learnings

# Phase 1: Parallel data collection
{
  git log --since="7 days ago" --format="%h|%s|%ai|%an" --author="$(git config user.name)" > "$WEEK_DIR/data-raw/commits-raw.txt"
} &

{
  git log --since="7 days ago" --name-only --format="" | sort | uniq -c | sort -rn > "$WEEK_DIR/data-raw/files-changed.txt"
} &

{
  git log --since="7 days ago" --format="%ad|%aD" --date=format:"%Y-%m-%d %H:%M|%a" > "$WEEK_DIR/data-raw/timeline-raw.txt"
} &

wait

echo "Phase 1 complete. Data collected in $WEEK_DIR/data-raw/"
```

### Phase 2-3: Analysis (Python Script)

```python
# analyze.py - Process raw data into JSON insights
import json
import re
from collections import defaultdict
from datetime import datetime

def analyze_daily_distribution(timeline_file):
    daily = defaultdict(int)
    with open(timeline_file) as f:
        for line in f:
            date = line.split('|')[0].split()[0]
            daily[date] += 1
    return dict(daily)

def analyze_time_blocks(timeline_file):
    blocks = {"Morning (6-12)": 0, "Afternoon (12-18)": 0, "Evening (18-24)": 0, "Night (0-6)": 0}
    with open(timeline_file) as f:
        for line in f:
            hour = int(line.split()[1].split(':')[0])
            if 6 <= hour < 12:
                blocks["Morning (6-12)"] += 1
            elif 12 <= hour < 18:
                blocks["Afternoon (12-18)"] += 1
            elif 18 <= hour < 24:
                blocks["Evening (18-24)"] += 1
            else:
                blocks["Night (0-6)"] += 1
    return blocks

def categorize_files(files_file):
    categories = defaultdict(lambda: {"count": 0, "files": 0})
    with open(files_file) as f:
        for line in f:
            parts = line.strip().split(maxsplit=1)
            if len(parts) != 2:
                continue
            count, filename = parts
            count = int(count)
            
            if 'mistakes/' in filename:
                cat = "Mistakes"
            elif '.obsidian/' in filename:
                cat = "Tool Config"
            elif 'Graph' in filename and 'Progress' in filename:
                cat = "Graph Learning"
            elif 'worksheet' in filename:
                cat = "Daily Tracking"
            elif '.specstory/' in filename:
                cat = "AI Conversations"
            else:
                cat = "General Notes"
            
            categories[cat]["count"] += count
            categories[cat]["files"] += 1
    
    return dict(categories)

def analyze_commit_types(commits_file):
    types = {"Maintenance": 0, "New Work": 0, "Automated": 0, "Other": 0}
    with open(commits_file) as f:
        for line in f:
            message = line.split('|')[1]
            if 'vault backup' in message.lower():
                types["Automated"] += 1
            elif any(word in message.lower() for word in ['fix', 'refactor', 'update', 'backup']):
                types["Maintenance"] += 1
            elif any(word in message.lower() for word in ['add', 'create', 'feat', 'new', 'implement']):
                types["New Work"] += 1
            else:
                types["Other"] += 1
    return types

def detect_sessions(timeline_file, gap_minutes=90):
    sessions = defaultdict(int)
    with open(timeline_file) as f:
        lines = f.readlines()
        if not lines:
            return dict(sessions)
        
        prev_date = None
        prev_mins = 0
        
        for line in lines:
            parts = line.split('|')[0].split()
            date = parts[0]
            time = parts[1]
            h, m = map(int, time.split(':'))
            mins = h * 60 + m
            
            if date != prev_date or (mins - prev_mins) > gap_minutes:
                sessions[date] += 1
            
            prev_date = date
            prev_mins = mins
    
    return dict(sessions)

# Main execution
if __name__ == "__main__":
    import sys
    data_dir = sys.argv[1] if len(sys.argv) > 1 else "."
    
    results = {
        "daily_distribution": analyze_daily_distribution(f"{data_dir}/timeline-raw.txt"),
        "time_blocks": analyze_time_blocks(f"{data_dir}/timeline-raw.txt"),
        "file_categories": categorize_files(f"{data_dir}/files-changed.txt"),
        "commit_types": analyze_commit_types(f"{data_dir}/commits-raw.txt"),
        "sessions": detect_sessions(f"{data_dir}/timeline-raw.txt")
    }
    
    with open(f"{data_dir}/analysis-data.json", 'w') as f:
        json.dump(results, f, indent=2)
    
    print(f"Analysis complete. Output: {data_dir}/analysis-data.json")
```

### Phase 4: Report Generation (Template Engine)

```python
# generate_report.py - Render final markdown
import json
from string import Template

def generate_report(data_file, output_file):
    with open(data_file) as f:
        data = json.load(f)
    
    # Calculate metrics
    total_commits = sum(data['commit_types'].values())
    auto_ratio = data['commit_types'].get('Automated', 0) / total_commits * 100 if total_commits else 0
    peak_day = max(data['daily_distribution'].items(), key=lambda x: x[1])
    morning_pct = data['time_blocks'].get('Morning (6-12)', 0) / total_commits * 100 if total_commits else 0
    
    report = f"""# Weekly Self-Reflection Analysis

## 📊 Activity Summary
- **Total Commits**: {total_commits}
- **Peak Day**: {peak_day[0]} ({peak_day[1]} commits)
- **Morning Productivity**: {morning_pct:.1f}%
- **Automation Ratio**: {auto_ratio:.1f}%

## ⏰ Time Distribution
"""
    
    for block, count in data['time_blocks'].items():
        pct = count / total_commits * 100 if total_commits else 0
        bar = "█" * int(pct / 5)
        report += f"- {block}: {count} ({pct:.1f}%) {bar}\n"
    
    report += "\n## 📁 Work Categories\n"
    for cat, stats in data['file_categories'].items():
        report += f"- **{cat}**: {stats['count']} changes across {stats['files']} files\n"
    
    report += "\n## 💡 Insights\n"
    
    # Generate insights based on data
    if morning_pct > 40:
        report += "- 🌟 Strong morning productivity pattern detected\n"
    if auto_ratio > 80:
        report += "- ⚠️ High automation ratio - check for intentional commits\n"
    if data['daily_distribution'].get('Sun', 0) > 5:
        report += "- ⚠️ Sunday work detected - review work-life balance\n"
    
    report += "\n## 🎯 Self-Reflection Questions\n"
    report += f"1. _\"I had {data['sessions'].get(peak_day[0], 1)} work sessions on my peak day. What made that day effective?\"_\n"
    report += f"2. _\"{auto_ratio:.0f}% of my commits are automated. Am I tracking progress or just activity?\"_\n"
    
    with open(output_file, 'w') as f:
        f.write(report)
    
    print(f"Report generated: {output_file}")

if __name__ == "__main__":
    import sys
    generate_report(sys.argv[1], sys.argv[2])
```

---

## 🚀 Execution Checklist

### For Next Week Analysis:

**Step 1: Prepare Environment** (10 seconds)
- [ ] Ensure git repo is current
- [ ] Check `reflections-2026/` folder exists
- [ ] Verify write permissions

**Step 2: Execute in Parallel** (30 seconds)
- [ ] Run 3 data collection commands simultaneously
- [ ] Wait for all to complete
- [ ] Verify 3 raw data files exist

**Step 3: Process Data** (20 seconds)
- [ ] Run `python analyze.py data-raw/`
- [ ] Verify `analysis-data.json` created
- [ ] Check JSON structure

**Step 4: Generate Reports** (15 seconds)
- [ ] Run `python generate_report.py data-raw/analysis-data.json analysis-report.md`
- [ ] Review generated markdown
- [ ] Move to `YYYY-WXX/` folder

**Step 5: Deliver** (5 seconds)
- [ ] Present summary to user
- [ ] Include top 3 insights
- [ ] Attach reflection prompts

**Total Time**: ~80 seconds (vs 10+ minutes manual)

---

## 📋 User Customization Points

### Adjustable Parameters:

| Parameter | Default | Location | Description |
|-----------|---------|----------|-------------|
| `since` | "7 days ago" | git commands | Analysis window |
| `session_gap` | 90 minutes | detect_sessions() | Session boundary |
| `author` | git config user.name | git --author | Filter commits |
| `morning_start` | 6 | time block logic | Morning definition |
| `afternoon_start` | 12 | time block logic | Afternoon definition |
| `evening_start` | 18 | time block logic | Evening definition |

### File Category Rules:
Edit regex patterns in `categorize_files()`:
```python
patterns = {
    r'mistakes/': 'Learning/Mistakes',
    r'\.obsidian/': 'Tool Config',
    r'Graph.*Progress': 'Graph Learning',
    # Add custom patterns here
}
```

---

## 🔄 Next Week Trigger

When user says: _"Analyze my week"_ or _"Weekly reflection"_

**Auto-execute**:
1. Read this plan
2. Run Phase 1 in parallel (3 bash commands)
3. Run Phase 2-3 in parallel (Python script)
4. Run Phase 4 (Python template render)
5. Present summary + link to full report

**No user approval needed** - this is a pre-approved workflow.

---

## 📊 Success Metrics

Track improvement each week:
- [ ] Time to generate report: Target < 2 minutes
- [ ] Manual intervention required: Target 0
- [ ] Data accuracy: 100% (verifiable from git)
- [ ] User satisfaction: Insights are actionable

---

*Plan Version: 1.0 | Created: Jan 30, 2026 | Parallel Tasks: 10 | Sequential: 1*