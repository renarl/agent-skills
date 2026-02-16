# Agent Brief Templates

This file contains copy-paste-ready templates for creating briefs, checkpoints, and output files.

## Table of Contents
1. [Overview Brief Template](#overview-brief-template)
2. [Checkpoint Brief Template](#checkpoint-brief-template)
3. [Index File Template](#index-file-template)
4. [Item File Template](#item-file-template)
5. [Interim Save Template](#interim-save-template)

---

## Overview Brief Template

Use this for `briefs/00_overview.md`:

```markdown
# [Project Name]

## Mission
[One sentence: what we're trying to accomplish]

## Success Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Checkpoints

| CP | Name | Status | Output |
|----|------|--------|--------|
| 1 | [Phase name] | NOT STARTED | `results/cp1_xxx.md` |
| 2 | [Phase name] | NOT STARTED | `results/cp2_xxx.md` |

## Constraints
- [Time, scope, or resource limits]

## Out of Scope
- [What this project explicitly does NOT cover]
```

---

## Checkpoint Brief Template

Use this for each `briefs/0N_cpN_[phase_name].md`:

```markdown
# CP[N]: [Phase Name]

**Status:** NOT STARTED | IN PROGRESS | COMPLETE

## Goal
[What this checkpoint accomplishes]

## Inputs
- [What's needed from previous checkpoints or user]

## Process
[Step-by-step instructions for completing this checkpoint]

## Outputs (Required)
- `results/cpN_xxx.md` — [description]
- [Other artifacts]

## Stop Points
**After [specific step]:** STOP and wait for user approval before proceeding.

## Check-Before-Finish
- [ ] [Verification item 1]
- [ ] [Verification item 2]
- [ ] Output file(s) exist and are complete
- [ ] _index.md updated (if applicable)
```

---

## Index File Template

Use this for `results/[items]/_index.md` when a checkpoint produces many items:

```markdown
# [Items] Index

**Updated:** [date]
**Total:** N | **Complete:** X | **Pending:** Y

## Status

| # | Name | Status | [Key Metric] |
|---|------|--------|--------------|
| 1 | [Item name] | complete | [value] |
| 2 | [Item name] | pending | — |
```

---

## Item File Template

Use this for individual items that need machine-parseable metadata:

```yaml
---
id: 1
name: "Item Name"
status: complete
priority: HIGH
created: 2026-01-30
[other fields as needed]
---

# Item Name

[Content]
```

---

## Interim Save Template

Use this within a checkpoint's output file when work is long-running:

```markdown
## Batch 1: [Theme]
[Results]

## Batch 2: [Theme]
[Results]

---
**Progress:** 2/4 batches complete
**Next:** Batch 3
```
