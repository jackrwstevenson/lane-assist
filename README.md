# Lane Assist

Lane Assist is a modular toolkit for AI-assisted software engineering. It provides two components:

1. **Skills** — Agent skills that extend Claude's capabilities for the product lifecycle
2. **Workflow** — Best practice guides for humans and AI agents working together

## Skills

Skills are self-contained packages that give Claude specialised knowledge and workflows.

| Skill               | Purpose                                                      |
| ------------------- | ------------------------------------------------------------ |
| **spec-creator**    | Create detailed specifications through iterative questioning |
| **plan-creator**    | Break specs into step-by-step implementation plans           |
| **code-creator**    | TDD workflow (RED → GREEN → REFACTOR)                        |
| **debugger**        | Systematic debugging with root cause analysis                |
| **skill-creator**   | Create new skills for this marketplace                       |
| **frontend-design** | Create distinctive, production-grade frontend interfaces     |

### Architecture

Each skill is a self-contained directory:

```
skill-name/
├── SKILL.md (required)    # YAML frontmatter + instructions
├── scripts/               # Executable code
├── references/            # Documentation loaded as needed
└── assets/                # Templates, images (not loaded into context)
```

Context loads progressively to minimise token usage:

1. **Metadata** (~100 words) — Always loaded
2. **SKILL.md body** (<5k words) — Loaded when skill triggers
3. **Bundled resources** — Loaded only as needed

### Installation

Register this repository as a Claude Code Plugin marketplace:

```
/plugin marketplace add jackrwstevenson/lane-assist
```

Install the plugin:

```
/plugin install lane-assist@lane-assist
```

Skills activate automatically based on context, or mention them directly.

### Other Platforms

- **Claude.ai** — See [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude#h_a4222fa77b)
- **Claude API** — See [Skills API Quickstart](https://docs.claude.com/en/api/skills-guide#creating-a-skill)

## Workflow

The `workflow/` folder contains best practice guides for AI-assisted software engineering:

- **README.md** — Guide for human engineers: how to plan, implement, and review code with AI assistance
- **CLAUDE.md** — Guide for AI agents: working relationship, communication style, and workflow rules

These files are designed to be copied into your own projects to bootstrap effective human-AI collaboration.

> **Coming soon**: Automatic workflow bootstrapping as part of the plugin installation.
