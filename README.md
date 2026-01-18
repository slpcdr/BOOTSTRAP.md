# Python Project Bootstrap

A comprehensive prompt for AI coding agents to scaffold production-ready Python CLI applications.

## What You Get

- **Professional CLI** using Typer + Rich
- **Type-safe configuration** using Pydantic Settings
- **Structured logging** with structlog (JSON/console modes)
- **Strict code quality** via ruff + mypy
- **Automated checks** via pre-commit hooks
- **Conventional commits** enforcement
- **AI-friendly development** with documented workflows
- **Spec-driven and TDD workflows**

## Usage

1. Copy the contents of [PROMPT.md](./PROMPT.md)
2. Replace `[PROJECT_NAME]` with your project name
3. Replace `[DESCRIPTION]` with a brief description
4. Provide to your AI coding agent

## Prerequisites

Requires [uv](https://docs.astral.sh/uv/) for package management:
```bash
uv --version || curl -LsSf https://astral.sh/uv/install.sh | sh
```
