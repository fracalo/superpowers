---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** This should be run in a dedicated worktree (created by brainstorming skill).

**Task store:** Beads (`bd` CLI) — one epic per feature, one task bead per atomic task.

**Save summary to:** `docs/plans/YYYY-MM-DD-<feature-name>.md` (lightweight pointer to the beads)

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Summary Document

**Every plan MUST save a summary file with this structure:**

```markdown
# [Feature Name] Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

**Epic:** `<epic-id>` (created by `bd create`)

---

## Quick Reference

View all tasks:
bd list --parent <epic-id> --pretty

View next unblocked tasks:
bd ready --parent <epic-id>

This file is a summary only. All task details live in the beads.
```

## Creating Beads

### Step 1: Create the epic

```bash
bd create "Feature Name" --type epic --description "One-line goal of this feature"
```

Note the epic ID returned.

### Step 2: Create task beads

One `bd create` per atomic task. Each task description must contain exact file paths, complete code, exact commands, and expected output — everything the implementer needs.

```bash
bd create "Task title" \
  --type task \
  --parent <epic-id> \
  --description "**Files:**
- Create: exact/path/to/file.py
- Modify: exact/path/to/existing.py:123-145
- Test: tests/exact/path/to/test.py

**Step 1: Write the failing test**

\`\`\`python
def test_specific_behavior():
    result = function(input)
    assert result == expected
\`\`\`

**Step 2: Run test to verify it fails**

Run: pytest tests/path/test.py::test_name -v
Expected: FAIL with 'function not defined'

**Step 3: Write minimal implementation**

\`\`\`python
def function(input):
    return expected
\`\`\`

**Step 4: Run test to verify it passes**

Run: pytest tests/path/test.py::test_name -v
Expected: PASS

**Step 5: Commit**

\`\`\`bash
git add tests/path/test.py src/path/file.py
git commit -m 'feat: add specific feature'
\`\`\`" \
  --acceptance "Tests pass, function returns expected value for given input"
```

### Step 3: Set dependencies

Only where ordering actually matters:

```bash
bd dep add <task-2-id> <task-1-id>
```

This means task-2 depends on task-1 (task-1 must finish first).

### Step 4: Verify

```bash
bd list --parent <epic-id> --pretty
```

Review the task tree. Ensure each task has:
- Exact file paths
- Complete code (not "add validation")
- Exact commands with expected output
- Verifiable acceptance criteria

## Remember
- Exact file paths always
- Complete code in task descriptions (not "add validation")
- Exact commands with expected output
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits
- These qualities apply to the task descriptions inside beads

## Execution Handoff

After saving the summary and creating all beads, offer execution choice:

**"Plan complete: summary saved to `docs/plans/<filename>.md`, epic `<epic-id>` with N task beads created. Two execution options:**

**1. Subagent-Driven (this session)** - I dispatch fresh subagent per task, review between tasks, fast iteration

**2. Parallel Session (separate)** - Open new session with executing-plans, batch execution with checkpoints. Find tasks with: `bd list --parent <epic-id> --pretty`

**Which approach?"**

**If Subagent-Driven chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:subagent-driven-development
- Stay in this session
- Fresh subagent per task + code review

**If Parallel Session chosen:**
- Guide them to open new session in worktree
- **REQUIRED SUB-SKILL:** New session uses superpowers:executing-plans
