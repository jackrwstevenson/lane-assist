---
name: code-creator
description: Test-Driven Development workflow for writing reliable code. Use this skill for ALL coding tasks including new features, bug fixes, refactoring, and any code changes. Enforces the RED → GREEN → REFACTOR cycle to ensure code correctness through tests-first development.
---

# Code Creator

Apply TDD for every code change. Write tests first, implement minimally, then refactor.

## Design Principles

- **YAGNI**: The best code is no code. Don't add features we don't need right now.
- When it doesn't conflict with YAGNI, architect for extensibility and flexibility.
- NEVER throw away or rewrite implementations without EXPLICIT permission.

## TDD Workflow

### 1. RED: Write Failing Test

1. Create test file if needed
2. Write test(s) that describe expected behaviour
3. Run tests - confirm they FAIL
4. If tests pass, either the feature exists or tests are wrong

For bug fixes: write test that reproduces the bug first.

### 2. GREEN: Minimal Implementation

1. Write the smallest reasonable change to pass the failing test
2. Ignore elegance, duplication, edge cases for now
3. Run tests - confirm they PASS
4. If tests fail, fix implementation (not tests)

Prefer simple, clean, maintainable solutions over clever ones. Readability trumps conciseness.

### 3. REFACTOR: Clean Up

1. Reduce code duplication
2. Improve structure, naming
3. Run tests after each change - must stay GREEN
4. Match surrounding code style, even if it differs from standard guides
5. Stop when code is clean and readable

Do not add new functionality during refactor.

## Cycle Discipline

- Never skip RED. No implementation without failing test first.
- Never skip GREEN verification. Always run tests.
- Small cycles. One behaviour per code-creator loop.
- Commit at GREEN. Safe points for version control.

## Testing Standards

- ALL TEST FAILURES ARE YOUR RESPONSIBILITY. Fix broken things immediately.
- Never delete a test because it's failing. Raise the issue instead.
- Tests MUST comprehensively cover ALL functionality.
- NEVER write tests that test mocked behaviour instead of real logic.
- NEVER implement mocks in end-to-end tests. Use real data and real APIs.
- Test output MUST BE PRISTINE. Expected errors must be captured and validated.

## Code Comments

- Comments explain WHAT or WHY, never temporal context ("new", "improved", "refactored from").
- NEVER remove comments unless provably false.
- All code files start with 2-line ABOUTME comment:
  ```
  // ABOUTME: Brief description of what this file does
  // ABOUTME: Second line if needed
  ```
