---
description: Scope and delegate a task with clear goals, checkpoints, and deliverables
allowed-tools: Read, Write, AskUserQuestion
argument-hint: [task or project description]
---

Scope and delegate work based on the user's input: `$ARGUMENTS`

Load the scope-and-delegate skill for full framework details, then follow these steps:

## 1. Understand the Task

Read the user's input and determine:

- **What** they want accomplished (concrete outcome or deliverable)
- **Why** it matters (underlying need, broader goal)
- **How big** the task is (one-shot vs multi-phase)

If the task is trivial and can be done in one shot, say so and just do it — don't over-engineer.

## 2. Scope the Work

For non-trivial tasks, establish clarity on four dimensions using `AskUserQuestion`. Proactively suggest likely answers based on context:

- **Purpose & Rationale** — what and why
- **Authority Boundaries** — what decisions can be made autonomously vs need approval
- **Definition of Done** — specific acceptance criteria
- **Checkpoints** — natural phases (2-5), which need approval before proceeding

## 3. Create the Brief

For multi-session work, create the folder structure and brief files:

- `briefs/00_overview.md` — mission, scope, success criteria, checkpoint summary
- `briefs/01_cp1_[phase].md` through `briefs/0N_cpN_[phase].md` — one per checkpoint
- `results/` — directory for checkpoint outputs

Get user approval on the structure before starting work.

## 4. Begin Work

Start on the first checkpoint. Save outputs persistently. Stop at natural decision points for user approval. If a checkpoint produces many items, create an `_index.md` to track them.
