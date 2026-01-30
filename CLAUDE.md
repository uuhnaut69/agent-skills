# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository manages **Agent Skills**—modular packages that extend Claude's capabilities with specialized knowledge, workflows, and tools. Skills are "onboarding guides" for specific domains that transform Claude from a general-purpose agent into a specialized one.

## Architecture

### Skill Structure
```
skill-name/
├── SKILL.md           # Required: YAML frontmatter (name, description) + markdown instructions
├── scripts/           # Optional: Executable Python/Bash for deterministic tasks
├── references/        # Optional: Documentation loaded into context as needed
└── assets/            # Optional: Files used in output (templates, images, fonts)
```

### Progressive Disclosure Design
Skills use three-level loading to manage context efficiently:
1. **Metadata** (name + description) - Always in context (~100 words)
2. **SKILL.md body** - Loaded when skill triggers (<5k words)
3. **Bundled resources** - Loaded/executed as needed

### Key Files
- `skills/spring-boot/` - Spring Boot development patterns and best practices
- `.claude-plugin/marketplace.json` - Plugin registry metadata

### Naming Conventions
- Skill names must be hyphen-case: lowercase letters, digits, and hyphens only
- Maximum 64 characters for name, 1024 for description
- Description cannot contain angle brackets
