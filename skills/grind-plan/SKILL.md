---
name: grind-plan
description: Use when a design document or implementation plan exists and needs validation before execution begins - checks for gaps, ambiguity, missing file paths, unclear test strategies, or anything that would block an AI agent from executing the plan without interpretation
---

# Grind Plan

## Overview

Validate that a plan is complete enough for an AI agent to execute without interpretation or guesswork.

**Core principle:** A plan that requires the executor to make judgment calls is not a plan — it's a wish list.

**Violating the letter of this rule is violating the spirit of this rule.**

## The Iron Law

```
NO EXECUTION WITHOUT PLAN VALIDATION FIRST
```

If the plan hasn't passed grind-plan, it is not ready for execution. Period.

## The 7 Validation Dimensions

Evaluate the plan against each dimension. A single blocking issue in any dimension = FAIL.

### 1. File Paths

Every file reference must be an exact path from the repository root.

| Gap | Example |
|-----|---------|
| Vague reference | "update the config file" |
| Partial path | "in the utils folder" |
| Implied location | "add a test for this" |
| Missing extension | "create a handler module" |

**Ask:** "Which exact file? What is its path from repo root?"

### 2. Test Strategy

Every behavioral change needs a test with expected input, expected output, and the command to run it.

| Gap | Example |
|-----|---------|
| No test mentioned | Step says "implement X" with no test step |
| Test without assertion | "write a test for the new function" |
| No run command | Test exists but no `pytest`/`npm test` invocation |
| Missing expected output | "run tests" without specifying pass/fail expectation |

**Ask:** "What test proves this works? What command runs it? What output confirms success?"

### 3. Regression Prevention

Existing test suites must be identified. Impact on other code must be analyzed. Rollback must be possible.

| Gap | Example |
|-----|---------|
| No existing test mention | Plan modifies code with no mention of existing tests |
| No impact analysis | Changes a shared utility without checking callers |
| No rollback path | Multi-step change with no commit boundaries |
| Missing integration check | New feature with no end-to-end verification |

**Ask:** "What existing tests cover this area? Who else calls this code? Where are the commit checkpoints?"

### 4. Step Granularity

Each step must be a single action, completable in 2-5 minutes. No compound steps.

| Gap | Example |
|-----|---------|
| Compound step | "implement the auth module and write tests" |
| Unbounded scope | "refactor as needed" |
| Missing intermediate steps | Jumps from "create file" to "integration works" |
| No verification between steps | Three implementation steps, then one test step |

**Ask:** "Can this step be split further? What is the single action here? Where is the verification step?"

### 5. Ambiguous Language

Flag any phrase that requires interpretation. The executor should never have to decide what the plan means.

| Gap | Example |
|-----|---------|
| Vague verb | "add validation", "handle errors", "clean up" |
| Subjective qualifier | "handle appropriately", "improve performance" |
| Undefined reference | "similar to the other one", "the usual pattern" |
| Implicit knowledge | "follow best practices", "standard approach" |

**Ask:** "What specifically should happen? What does 'appropriately' mean here? Show the actual code."

### 6. Dependencies and Ordering

Task dependencies must be explicit. No step should assume prior context that isn't stated.

| Gap | Example |
|-----|---------|
| Implicit ordering | Steps that only work in a specific order but don't say so |
| Hidden dependency | Step 5 uses a function created in Step 2 without stating it |
| Parallel ambiguity | Unclear which tasks can run independently |
| Missing prerequisite | "Run the migration" without "first, create the migration" |

**Ask:** "What must be done before this step? Which tasks depend on which? What can run in parallel?"

### 7. Behavioral Specificity

Descriptions must specify exact behavior — inputs, outputs, edge cases, error handling — so a skilled agent can implement without guessing intent.

| Gap | Example |
|-----|---------|
| Undefined behavior | "validate the input" without saying what valid means |
| Missing edge cases | "parse the date" with no mention of invalid formats |
| Unclear error handling | "handle failures" without specifying what happens on each failure mode |
| Unspecified inputs/outputs | "transform the data" without stating shape in and shape out |

**Ask:** "What exactly should happen for each input? What are the edge cases? What does failure look like?"

## The Verdict

```
PASS: All 7 dimensions clear. Plan is executable.

FAIL (N blocking issues):
  - Dimension: [specific gap]
  - Dimension: [specific gap]
  ...
```

**Binary only.** PASS or FAIL. No "mostly good", no "good enough", no "minor issues."

A single blocking issue = FAIL.

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "Plan looks good enough" | "Good enough" = gaps the executor will stumble on |
| "I wrote it so I know what it means" | You won't be executing it. A fresh agent will. |
| "We can clarify during execution" | Clarification during execution = wasted context and wrong turns |
| "It's obvious what to do" | Obvious to you ≠ obvious to a fresh agent with zero context |
| "The executor is smart" | Smart executors make smart guesses. Guesses are still wrong. |
| "This is just a small change" | Small changes with ambiguous plans cause the biggest surprises |
| "We'll iterate" | Iteration without a solid plan = random walk |
| "Validation is overhead" | 5 minutes of validation saves hours of rework |

## Red Flags - STOP

- Plan has steps without exact file paths
- Steps say "add tests" without specifying which tests
- Phrases like "as needed", "if necessary", "appropriately"
- Steps that combine multiple actions
- Steps describe behavior without specifying inputs, outputs, or edge cases
- Dependencies between steps that aren't stated
- No commit boundaries between logical changes
- Thinking "the executor will figure it out"

**All of these mean: FAIL. Fix the plan before execution.**

## Workflow Position

```
brainstorming → grind-plan → writing-plans → executing-plans
                 ^^^^^^^^^^^
                 YOU ARE HERE
```

**Input:** Design document or implementation plan from brainstorming
**Output:** PASS verdict (proceed to writing-plans) or FAIL with specific gaps to fix

## The Bottom Line

**A plan that passes grind-plan can be executed by a fresh agent with zero context and zero judgment calls.**

If the executor needs to interpret, guess, or decide — the plan fails.
