---
name: scope-and-delegate
description: Framework for establishing clear task delegation with defined goals, boundaries, success criteria, and deliverables. Use this skill whenever the user is starting a new project, delegating complex work, or wants Claude to clarify scope and expectations before beginning. Trigger on phrases like "scope this out", "clarify before starting", "what do you need to know", "let's plan this", "brief me", "set up a project", or any request involving delegation, project kickoff, requirements gathering, or multi-phase work. Also trigger when the user asks Claude to break down a large task into phases, create a project structure, or establish checkpoints for complex work. If the task is non-trivial and multi-session, this skill should be used.
---

# Task Brief Framework

Complex work benefits from **checkpoint-based briefs** — separate documents for each phase, with persistent outputs that enable resuming and deepening work.

## When to Use This Skill

This skill is for work that spans multiple steps or sessions. The idea is to write everything down so that work can be paused, resumed, handed off, or deepened without losing context. If a task can be done in one shot, you probably don't need this — just do it. But if it involves multiple phases, needs user approval at certain points, or produces many artifacts, this framework will keep things organized.

## Part 1: Scoping the Work

Before beginning, establish clarity on four dimensions:

### 1. Purpose & Rationale
What are we trying to accomplish, and why? Pin down the concrete outcome or deliverable, the underlying need, and how success serves the broader goal.

### 2. Authority Boundaries
Where are the limits of the delegated authority? Clarify what decisions can be made autonomously, what requires approval, what resources and tools are available, and any constraints on time, budget, or scope.

### 3. Definition of Done
What does "done" look like? Define specific acceptance criteria, quality standards, edge cases that must be handled, and how completion will be verified.

### 4. Checkpoints
What are the natural phases of this work? Break work into 2-5 checkpoints, each independently completable. Define what triggers moving to the next checkpoint and which ones need user approval before proceeding.

## Part 2: Brief Structure

For non-trivial work, create a **multi-file brief** rather than a single document. See `references/templates.md` for the full folder structure, overview brief template, and checkpoint brief template.

### Recommended Folder Structure

```
project/
├── briefs/                       # Source of truth for methodology
│   ├── 00_overview.md            # Mission, scope, success criteria
│   ├── 01_cp1_[phase_name].md    # Checkpoint 1 instructions
│   ├── 02_cp2_[phase_name].md    # Checkpoint 2 instructions
│   └── ...
├── results/                      # All checkpoint outputs
│   ├── cp1_[output_name].md      # CP1 deliverable
│   ├── cp2_[output_name].md      # CP2 deliverable
│   ├── [items]/                  # Subfolder if many items
│   │   ├── _index.md             # Index with status tracking
│   │   └── [item].md             # One file per item
│   └── ...
└── README.md                     # Project overview and status
```

## Part 3: Output Patterns

### Index Files
When a checkpoint produces many items, create an `_index.md` with a status table tracking total, complete, and pending counts. See `references/templates.md` for the full format.

### Item Files with YAML Frontmatter
For items that need machine-parseable metadata, use YAML frontmatter with id, name, status, priority, and created fields.

### Interim Saves
For long-running checkpoints, save progress incrementally in batches with a progress note at the bottom (e.g., "Progress: 2/4 batches complete. Next: Batch 3").

## Part 4: Working Patterns

### Resume After Interruption
1. Read `00_overview.md` to understand project status
2. Check `_index.md` files for item-level status
3. Read the current checkpoint brief
4. Continue from last saved state

### Deepening Work
To go deeper on a completed checkpoint, create a new checkpoint brief (e.g., `03_cp2b_deep_dive.md`), reference the original checkpoint's outputs as inputs, and define new, more specific outputs.

### Checkpoint Transitions
Before marking a checkpoint complete:
1. Run through the Check-Before-Finish list
2. Update checkpoint status in `00_overview.md`
3. Update any `_index.md` files
4. Summarize outputs to user
5. If next checkpoint needs approval, STOP and wait

## Part 5: Usage Instructions

### For Simple Tasks (< 1 hour of work)
1. Use `AskUserQuestion` to quickly confirm scope
2. Proactively suggest likely answers based on context
3. Summarize brief verbally, then proceed

### For Complex Tasks (multi-session work)
1. Use `AskUserQuestion` to gather requirements (one dimension at a time)
2. Proactively suggest likely answers — propose what the user probably wants
3. Propose checkpoint breakdown
4. Create folder structure and brief files
5. Get user approval on structure before starting work
6. Work checkpoint by checkpoint, saving outputs persistently

### Suggesting Answers
When asking questions, include your best guess. For example: "What does 'done' look like? Based on your request, I'm guessing: [likely criteria]. Does that match, or should we adjust?" This makes it easy to say "yes" or quickly correct course.

### Key Principles
1. **Write everything down** — Outputs should survive session boundaries
2. **Checkpoint, don't monolith** — Break work into resumable phases
3. **Index many items** — Use `_index.md` when tracking multiple things
4. **Stop before big decisions** — Get user approval at natural boundaries
5. **Check before finishing** — Every checkpoint has a verification checklist
6. **Enable deepening** — Structure outputs so they can be expanded later

## Quick Reference

| Situation | Action |
|-----------|--------|
| Starting new project | Create `briefs/00_overview.md` + checkpoint briefs |
| Many items to track | Create `results/[items]/_index.md` |
| Long-running work | Save in batches, note progress |
| Need user decision | Use explicit STOP point |
| Finishing checkpoint | Run Check-Before-Finish, update status |
| Resuming work | Read overview → index → checkpoint brief |
| Going deeper | Create new checkpoint brief referencing prior outputs |
