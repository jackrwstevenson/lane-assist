# README

A guide for human engineers on building software effectively with AI assistance.

## Core Principle

Treat AI as a powerful pair programmer requiring clear direction, context, and oversight—not autonomous judgment. You remain accountable for the software produced.

---

## Phase 1: Spec & Plan Creation

**Use Opus 4.5 with thinking enabled. Execute in Plan mode (shift+tab twice in Claude Code).**

1. **Brainstorm requirements** — Describe your idea and have the AI ask clarifying questions until requirements and edge cases are fleshed out.

2. **Create SPEC.md** — Compile requirements, architecture decisions, data models, and testing strategy into a single document.

3. **Create PLAN.md** — Break implementation into logical, bite-sized tasks or milestones. Iterate on this plan until it's coherent.

4. **Refine in Plan mode** — Go back and forth with Claude until you're satisfied with the plan. A good plan is critical—once solid, switch to auto-accept mode for execution.

---

## Phase 2: Implementation

### Work in Small Chunks

- Implement one function, fix one bug, add one feature at a time
- Each chunk should be small enough for the AI to handle within context and for you to understand
- Complete each step before moving to the next

### Provide Extensive Context

- Include relevant code, docs, constraints, and known pitfalls
- Use rules files (CLAUDE.md) and skills to encode style guides and preferences
- Show examples of desired output format when helpful

### Give Claude a Verification Loop

- Always provide a way for Claude to verify its work (tests, linters, type checkers)
- This feedback loop 2-3x the quality of final results
- If tests exist, instruct Claude to run them after each task and debug failures

---

## Phase 3: Quality Control

### Human Review (Non-Negotiable)

- Review every AI-generated snippet as if from a junior developer
- Never merge code you can't explain
- If something seems convoluted, ask for comments or rewrite it simpler

### Testing

- Weave testing into the workflow from the start
- Write or generate tests for each piece as you go
- Run tests after every change

### Version Control

- Commit after each successful task (save points)
- Use clear commit messages
- Use branches/worktrees to isolate experiments

---

## Key Rules

| Do                          | Don't                                   |
| --------------------------- | --------------------------------------- |
| Plan first, code second     | Dive in with vague prompts              |
| Break work into small tasks | Ask for large monolithic outputs        |
| Provide full context        | Make AI operate on partial information  |
| Test and verify everything  | Blindly trust AI output                 |
| Commit often                | Let changes pile up without checkpoints |
| Stay in the loop            | Let AI run unsupervised                 |

---

## Model Selection

- Use the best model available for each task
- Try multiple models if one gets stuck
- Switch models mid-stream when needed—each has different strengths
