# Lane Assist

Modular toolkit for AI-assisted software engineering. Two components:

1. **Skills** (`skills/`) — Agent skills extending Claude's capabilities
2. **Workflow** (`workflow/`) — Best practice templates for human-AI collaboration

## Project Structure

```
lane-assist/
├── skills/                    # Agent skills
│   ├── spec-creator/          # Create specifications
│   ├── plan-creator/          # Create implementation plans
│   ├── code-creator/          # TDD workflow
│   ├── debugger/              # Root cause debugging
│   └── skill-creator/         # Create new skills
├── workflow/                  # Best practice templates
│   ├── README.md              # Guide for human engineers
│   └── CLAUDE.md              # Guide for AI agents
└── .claude-plugin/            # Plugin marketplace config
```

## Skills Architecture

Each skill is a self-contained directory:

- **SKILL.md** (required): YAML frontmatter (`name`, `description`) + Markdown instructions
- **scripts/**: Executable code for deterministic operations
- **references/**: Documentation loaded into context as needed
- **assets/**: Templates, images (NOT loaded into context)

### Progressive Disclosure

Context loads in three levels to minimise token usage:

1. **Metadata** (~100 words): Always loaded — skill name + description
2. **SKILL.md body** (<5k words): Loaded when skill triggers
3. **Bundled resources**: Loaded only as needed

### Plugin Configuration

`.claude-plugin/marketplace.json` registers skills. Update the `skills` array when adding/removing skills.

## Workflow Templates

The `workflow/` folder contains templates for other projects to adopt:

- **workflow/README.md** — Human engineer guide for AI-assisted development
- **workflow/CLAUDE.md** — AI agent guide for working relationships and process

These are designed to be copied into target projects.
