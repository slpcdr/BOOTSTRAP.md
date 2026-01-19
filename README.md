# Python Project Bootstrap

[![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)](https://github.com/slpcdr/BOOTSTRAP.md)

A comprehensive prompt for AI coding agents to scaffold production-ready Python CLI applications.

## What You Get

- **Professional CLI** using Typer + Rich
- **Type-safe configuration** using Pydantic Settings
- **Structured logging** with structlog (JSON/console modes)
- **Strict code quality** via ruff + mypy
- **Automated checks** via pre-commit hooks
- **Conventional commits** enforcement
- **License compliance** checks for proprietary projects (pip-licenses)
- **AI-friendly development** with documented workflows
- **Spec-driven and TDD workflows**

## Usage

Prompt your AI coding agent:

```
Execute https://github.com/slpcdr/BOOTSTRAP.md/blob/main/BOOTSTRAP.md
```

The agent will fetch the bootstrap instructions and ask for:
- **Project name** — Name for your CLI application
- **Description** — What the project does
- **License type** — Open-source (MIT) or Proprietary

Then it executes the complete bootstrap sequence.

If your coding agent cannot do this, try another one.

### Keeping Projects Updated

Storing a copy of `BOOTSTRAP.md` in your project's Git repository allows you to keep your projects up to date. When the bootstrap template evolves, you can prompt your agent:

```
The BOOTSTRAP.md that created this project has a new version at
https://github.com/slpcdr/BOOTSTRAP.md/blob/main/BOOTSTRAP.md
— please update this project accordingly.
```

The agent will fetch the latest version, compare it with your local copy, and apply relevant updates to your project structure, tooling, or workflows.

> [!CAUTION]
> **Security note:** The user should always review agent-proposed changes before committing. Do not allow automatic merging of updates from external sources without thorough review.

**Tip:** Use an agent manager to automate this across multiple repositories.

## Prerequisites

Requires [uv](https://docs.astral.sh/uv/) for package management:
```bash
uv --version || curl -LsSf https://astral.sh/uv/install.sh | sh
```
