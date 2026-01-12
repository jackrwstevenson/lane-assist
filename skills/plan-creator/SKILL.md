---
name: plan-creator
description: Break specifications into step-by-step implementation plans. Use when a spec exists and you need to plan implementation, or when the user mentions "plan", "breakdown", "tasks", "milestones", or wants to implement a feature.
---

# Plan Creator

## When to Use

- After SPEC.md is complete
- Before writing any implementation code
- When preparing a Pull Request approach

## Prerequisites

- A completed SPEC.md or clear requirements
- Use Plan mode in Claude Code (shift+tab twice)

## Instructions

1. **Read the spec** — Understand all requirements before planning

2. **Break into small tasks** — Each task should be:

   - Completable in one focused session
   - Testable independently
   - Small enough to review easily

3. **Create PLAN.md** — Structure as ordered steps:

```markdown
# Implementation Plan: [Feature Name]

## Overview

[Brief summary of what we're building]

## Prerequisites

- [ ] [Any setup needed before starting]

## Implementation Steps

### Step 1: [Task Name]

**Goal**: [What this accomplishes]
**Files**: [Files to create/modify]
**Verification**: [How to test this step]

### Step 2: [Task Name]

**Goal**: [What this accomplishes]
**Files**: [Files to create/modify]  
**Verification**: [How to test this step]

[Continue for all steps...]

## Testing Plan

- [ ] [Test 1]
- [ ] [Test 2]

## Definition of Done

- [ ] All tests pass
- [ ] Code reviewed
- [ ] Documentation updated
```

4. **Iterate on the plan** — Review with the user:

   - Is each step clear and actionable?
   - Are dependencies between steps correct?
   - Is the verification approach solid?

5. **Get approval** — Present the plan and iterate until approved

## Best Practices

- Order steps by dependency (what must come first)
- Include verification for every step
- Keep tasks small—if a step feels big, split it
- A good plan enables 1-shot implementation
