# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Concision

In all interactions, be extremely concise and sacrifice grammar for the sake of concision.

## Project Overview

Lane Assist is a modular toolkit for creating Agent Skills - self-contained packages that extend Claude's capabilities. It's designed as a Claude Code Plugin marketplace, enabling teams to contribute and adopt skills for the product lifecycle.

## Architecture

### Skills Structure

Each skill is a self-contained directory with:

- **SKILL.md** (required): YAML frontmatter (name, description) + Markdown documentation
- **scripts/**: Executable code for deterministic operations
- **references/**: Documentation loaded into context as needed
- **assets/**: Template files, images (NOT loaded into context)

### Progressive Disclosure

Context is loaded in three levels to minimize token usage:

1. **Metadata** (~100 words): Always loaded - skill name + description from YAML frontmatter
2. **SKILL.md body** (<5k words): Loaded when skill triggers
3. **Bundled resources**: Loaded only as needed

### Plugin Configuration

The `.claude-plugin/marketplace.json` registers this repository as a Claude Code Plugin marketplace. Skills are referenced by path in the `skills` array.
