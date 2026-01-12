---
name: spec-creator
description: Create detailed software specifications through iterative questioning. Use when starting a new project, feature, or when the user mentions "spec", "requirements", "design doc", or describes an idea without clear requirements.
---

# Spec Creator

## When to Use

- User describes a new project or feature idea
- Requirements are vague or incomplete
- Starting any non-trivial implementation work

## Instructions

1. **Gather requirements through questions** — Ask clarifying questions one at a time until you understand:

   - Core functionality and user goals
   - Edge cases and error conditions
   - Technical constraints (languages, frameworks, APIs)
   - Non-functional requirements (performance, security, scalability)

2. **Document in SPEC.md** — Compile findings into a structured document:

```markdown
# [Project/Feature Name] Specification

## Overview

[1-2 sentence summary]

## Requirements

### Functional

- [Requirement 1]
- [Requirement 2]

### Non-Functional

- [Performance, security, scalability needs]

## Architecture

[High-level design decisions]

## Data Models

[Key entities and relationships]

## API/Interface Design

[Endpoints, function signatures, UI flows]

## Edge Cases

[Known edge cases and how to handle them]

## Testing Strategy

- Unit tests: [what to test]
- Integration tests: [what to test]
- Acceptance criteria: [how to verify]

## Out of Scope

[Explicitly excluded items]
```

3. **Review with user** — Present the spec and iterate until approved

## Best Practices

- Don't assume—ask
- Keep questions focused (one topic at a time)
- Document decisions and rationale
- Flag ambiguities rather than guessing
