# Python Project Bootstrap

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

## Prerequisites

Requires [uv](https://docs.astral.sh/uv/) for package management:
```bash
uv --version || curl -LsSf https://astral.sh/uv/install.sh | sh
```
