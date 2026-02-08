---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan summary for context, then pull tasks from beads (`bd` CLI).
Execute in batches, report for review between batches.

**Core principle:** Batch execution with checkpoints for architect review.
Beads are the source of truth for tasks.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

## The Process

### Step 1: Load Plan and Review Tasks
1. Read summary file for Goal, Architecture, Tech Stack
2. Get epic ID from the `**Epic:**` field in the summary
3. `bd list --parent <epic-id> --pretty` to view all tasks
4. Review critically — identify any questions or concerns about the tasks
5. If concerns: Raise them with your human partner before starting
6. If no concerns: Proceed

### Step 2: Execute Batch
**Default: First 3 unblocked tasks**

Get next batch:
```bash
bd ready --parent <epic-id>
```

For each task:
1. `bd update <task-id> --claim` — mark as yours
2. `bd show <task-id>` — get full task details (files, steps, code, acceptance criteria)
3. Follow each step exactly (task has bite-sized steps)
4. Run verifications as specified
5. `bd close <task-id>` — mark complete

### Step 3: Report
When batch complete:
- Show what was implemented
- Show verification output
- Say: "Ready for feedback."

### Step 4: Continue
Based on feedback:
- Apply changes if needed
- `bd ready --parent <epic-id>` for next batch
- Execute next batch
- Repeat until complete

### Step 5: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker mid-batch (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan summary and `bd list` output critically first
- Follow task steps exactly (from `bd show`)
- Use `bd ready` to find next tasks — never guess task ordering
- Don't skip verifications
- Reference skills when plan says to
- Between batches: just report and wait
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** - REQUIRED: Set up isolated workspace before starting
- **superpowers:writing-plans** - Creates the plan and beads this skill executes
- **superpowers:finishing-a-development-branch** - Complete development after all tasks
