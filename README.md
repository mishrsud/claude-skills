# claude-skills

A reusable collection of Claude Code skills and personas for development projects.

## Purpose

This repo provides a shared library of Claude Code configuration — personas, skills, and workflow guides — that can be dropped into any project to establish consistent AI-assisted development practices.

## Structure

```
claude-skills/
├── docs/                        # Guides and reference material
│   └── claude-code-workflow-guide.md
├── personas/                    # Role-specific instruction sets
└── skills/                      # Reusable slash command definitions
```

## Usage

### Personas

Persona files define how Claude should approach a task — its priorities, output style, and workflow. Copy the relevant persona files into your project's `.claude/personas/` directory and invoke them as needed.

### Skills

Skill files define reusable slash commands. Place them in `.claude/commands/` in your project or globally in `~/.claude/commands/` to make them available across all projects.

### Docs

The `docs/` directory contains guides on how to use Claude Code effectively, including workflow patterns and configuration best practices. Start with [`claude-code-workflow-guide.md`](docs/claude-code-workflow-guide.md).

## Related

- [Claude Code documentation](https://docs.anthropic.com/claude/claude-code)
