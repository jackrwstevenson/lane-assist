---
name: red-green-refactor
description: Test-Driven Development workflow for writing reliable code. Use this skill for ALL coding tasks including new features, bug fixes, refactoring, and any code changes. Enforces the RED → GREEN → REFACTOR cycle to ensure code correctness through tests-first development.
---

# Red Green Refactor

Apply TDD for every code change. Write tests first, implement minimally, then refactor.

## Workflow

### 1. RED: Write Failing Test

Before writing any implementation:

1. Create test file if needed
2. Write test(s) that describe expected behavior
3. Run tests - confirm they FAIL
4. If tests pass, either the feature exists or tests are wrong

For bug fixes: write test that reproduces the bug first.

### 2. GREEN: Minimal Implementation

Make tests pass with the simplest possible code:

1. Write only enough code to pass the failing test
2. Ignore elegance, duplication, edge cases
3. Run tests - confirm they PASS
4. If tests fail, fix implementation (not tests)

Resist urge to over-engineer. "Simplest" means literal minimum.

### 3. REFACTOR: Clean Up

With green tests as safety net:

1. Improve code structure, naming, duplication
2. Run tests after each change - must stay GREEN
3. If tests fail, revert last change
4. Stop when code is clean and readable

Do not add new functionality during refactor.

## Cycle Discipline

- Never skip RED. No implementation without failing test first.
- Never skip GREEN verification. Always run tests after implementation.
- Refactor is optional per cycle but required before PR/commit.
- Small cycles. One behavior per RED-GREEN-REFACTOR loop.
- Commit at GREEN. Safe points for version control.
