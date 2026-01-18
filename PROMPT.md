Create a new Python CLI application called **[PROJECT_NAME]** with the following foundational structure. Focus on setting up the development infrastructure first—the business logic will come later.

## Project Structure

```
[PROJECT_NAME]/
├── src/
│   ├── __init__.py
│   ├── cli.py              # Typer CLI entry point
│   ├── config.py           # Pydantic Settings for configuration
│   ├── logging.py          # Structured logging configuration
│   └── database/
│       └── models.py       # SQLAlchemy models (if needed)
├── tests/
│   ├── conftest.py         # Pytest fixtures
│   └── test_example.py     # Example test file
├── docs/
│   └── specs/              # Feature specifications
├── .agent/
│   ├── AI_README.md        # Context for AI agents
│   ├── memory.md           # Active context & lessons learned
│   └── workflows/
│       ├── spec-driven-development.md
│       └── test-driven-development.md
├── .env.example            # Environment variable template
├── .gitignore              # Python gitignore + secrets
├── .pre-commit-config.yaml # Pre-commit hooks
├── pyproject.toml          # Project config and dependencies
├── PLAN.md                 # Technical roadmap
├── PROGRESS.md             # Implementation status tracker
└── README.md               # User-facing documentation
```

## 1. pyproject.toml

Use `hatchling` as the build backend. Include:

**Core dependencies:**
- `typer>=0.9.0` - CLI framework
- `rich>=13.0.0` - Beautiful terminal output
- `structlog>=23.1.0` - Structured JSON logging
- `pydantic>=2.0.0` - Data validation
- `pydantic-settings>=2.0.0` - Configuration management
- `python-dotenv>=1.0.0` - Environment file loading

**Dev dependencies (optional group):**
- `pytest>=7.0.0`
- `pytest-asyncio>=0.21.0`
- `pytest-cov>=4.0.0`
- `ruff>=0.8.0`
- `mypy>=1.0.0`
- `pre-commit>=3.6.0`

**Tool configuration:**

```toml
[project.scripts]
[PROJECT_NAME] = "src.cli:app"

[tool.ruff]
line-length = 100
target-version = "py311"

[tool.ruff.lint]
select = [
    "E", "F", "I", "N", "W", "UP", "B", "C90",
    "S", "ANN", "ASYNC", "PTH", "PL", "RUF"
]
ignore = ["S101", "PLR0913", "ANN401", "PLR2004"]

[tool.ruff.lint.per-file-ignores]
"tests/*" = ["S101", "PLR2004", "ANN"]

[tool.mypy]
python_version = "3.11"
check_untyped_defs = true
ignore_missing_imports = true

[tool.pytest.ini_options]
asyncio_mode = "auto"
testpaths = ["tests"]
markers = [
    "integration: tests requiring real API keys",
    "slow: slow running tests",
]
```

## 2. Pre-commit Hooks (.pre-commit-config.yaml)

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.8.0
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
      - id: ruff-format

  - repo: local
    hooks:
      - id: mypy
        name: mypy
        entry: uv run mypy src/ --ignore-missing-imports
        language: system
        types: [python]
        pass_filenames: false

      - id: pytest
        name: pytest
        entry: uv run pytest tests/ -m "not integration" -x -q
        language: system
        pass_filenames: false
        always_run: true

  - repo: https://github.com/compilerla/conventional-pre-commit
    rev: v3.6.0
    hooks:
      - id: conventional-pre-commit
        stages: [commit-msg]
```

## 3. Configuration (src/config.py)

```python
"""Application settings loaded from environment variables."""

from functools import lru_cache
from pydantic_settings import BaseSettings, SettingsConfigDict


class Settings(BaseSettings):
    """Application configuration with env var support."""

    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        extra="ignore",
    )

    # Add your settings here with defaults
    database_url: str = "sqlite:///./app.db"
    debug: bool = False
    json_logs: bool = False


@lru_cache
def get_settings() -> Settings:
    """Get cached settings instance."""
    return Settings()
```

## 4. Structured Logging (src/logging.py)

```python
"""Configure structured logging (JSON for prod, colored for dev)."""
import structlog
import sys
import logging
from src.config import get_settings

def setup_logging():
    """Configure structlog processors and formatting."""
    settings = get_settings()
    
    processors = [
        structlog.contextvars.merge_contextvars,
        structlog.processors.add_log_level,
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.StackInfoRenderer(),
        structlog.processors.format_exc_info,
    ]
    
    if settings.json_logs:
        processors.append(structlog.processors.JSONRenderer())
    else:
        processors.append(structlog.dev.ConsoleRenderer())

    structlog.configure(
        processors=processors,
        logger_factory=structlog.PrintLoggerFactory(),
        wrapper_class=structlog.make_filtering_bound_logger(logging.INFO),
        cache_logger_on_first_use=True,
    )
```

## 5. CLI Entry Point (src/cli.py)

```python
"""CLI interface for [PROJECT_NAME]."""

import typer
from rich.console import Console
from src.logging import setup_logging
import structlog

setup_logging()
logger = structlog.get_logger()

app = typer.Typer(
    name="[PROJECT_NAME]",
    help="[DESCRIPTION]",
    no_args_is_help=True,
)
console = Console()


@app.callback()
def callback() -> None:
    """Initialize on startup."""
    logger.info("application_startup")


@app.command()
def health() -> None:
    """Check health of all services."""
    logger.info("health_check_initiated")
    console.print("[green]✓[/green] All systems operational")


if __name__ == "__main__":
    app()
```

## 6. Test Fixtures (tests/conftest.py)

Create fixtures for:
- Temporary database for isolated tests
- Database session management
- Temporary directories for file operations
- Sample test data

## 7. AI Agent Context

### .agent/AI_README.md

```markdown
# AI Agent README

## Project Overview
**[PROJECT_NAME]** - [Brief description]

| Component | Location | Purpose |
|-----------|----------|---------|
| CLI | `src/cli.py` | User interface |
| Config | `src/config.py` | Settings management |
| Logs | `src/logging.py` | Structured logging setup |

## Development Methodology

### Use Spec-Driven Development For:
- New features
- Significant refactors
- Complex logic with edge cases

**Workflow**: `/spec-driven-development`

### Use Test-Driven Development For:
- Bug fixes
- New functions/methods
- Code with clear input/output behavior

**Workflow**: `/test-driven-development`

## Code Conventions

- **Formatter**: ruff format
- **Linter**: ruff with strict rules
- **Type hints**: Required on all functions
- **Docstrings**: Required on public functions
- **Logging**: Use `structlog` for logic, `rich` ONLY for user output

## Completion Criteria

A task is **DONE** when:
1. ✅ Code passes linting (`uv run ruff check`)
2. ✅ Code passes type checking (`uv run mypy`)
3. ✅ Unit tests cover the functionality
4. ✅ E2E test proves the command works
```

### .agent/memory.md

```markdown
# Active Context & Lessons Learned
*Maintain a list of architectural decisions and 'gotchas' encountered.*

- [Decision]: We use `structlog` for logic and `rich` ONLY for direct user output.
- [Gotcha]: Typer commands must explicitly return `None` or an `Exit` code.
- [Tooling]: We use `uv` for dependency management. Always run commands with `uv run`.
```

## 8. Development Workflows

### .agent/workflows/spec-driven-development.md

```markdown
---
description: Spec-driven development - write specification before implementation
---

# Spec-Driven Development

1. **Create specification** in `docs/specs/<feature>-spec.md`
   - Requirements, interfaces, edge cases, acceptance criteria

2. **Review specification** before implementation

// turbo
3. **Run baseline tests**
   ```bash
   uv run pytest tests/ -v
   ```

4. **Implement following the spec**

// turbo
5. **Verify implementation**
   ```bash
   uv run ruff check src/
   uv run mypy src/
   uv run pytest tests/ -v
   ```
```

### .agent/workflows/test-driven-development.md

```markdown
---
description: Test-driven development - write tests before implementation
---

# TDD Workflow

### 1. RED - Write Failing Test
// turbo
```bash
uv run pytest tests/test_<module>.py::<test_name> -v
```

### 2. GREEN - Minimal Code to Pass
// turbo
```bash
uv run pytest tests/test_<module>.py::<test_name> -v
```

### 3. REFACTOR - Improve with Tests Green
// turbo
```bash
uv run pytest tests/ -v
```
```

## 9. Progress Tracking (PROGRESS.md)

```markdown
# [PROJECT_NAME] - Progress Tracker

A task is **DONE** when verified with tests passing.

---

## Phase 1: [Current Phase Name]

### [Component 1]
- [ ] Feature 1
- [ ] Feature 2

### [Component 2]
- [ ] Feature 3

---

## Future Phases

### Phase 2: [Name]
- [ ] Planned item
```

## 10. Technical Plan (PLAN.md)

Include:
- Project overview and goals
- System architecture diagram (ASCII art)
- Component breakdown with file locations
- Implementation phases with completion criteria
- Technology stack table
- Configuration reference

## 11. Standard Files

**.gitignore** - Include:
```
__pycache__/
*.py[cod]
*.egg-info/
dist/
venv/
.venv/
.env
*.db
.pytest_cache/
.coverage
.mypy_cache/
.ruff_cache/
```

**.env.example** - Template for all required env vars with comments

**README.md** - Include:
- Features list
- Prerequisites
- Installation steps
- Usage examples
- Configuration table
- Development commands
- Project structure

## Setup Instructions (Using uv)

Do not use standard `pip` or `venv`. Configure the project using `uv` for 10x speed and strict locking.

1. **Ensure uv is installed:**
   ```bash
   uv --version || curl -LsSf https://astral.sh/uv/install.sh | sh
   ```
2. **Initialize project:**
   ```bash
   uv init
   ```
3. **Add dependencies:**
   ```bash
   uv add typer rich pydantic pydantic-settings python-dotenv structlog
   uv add --dev pytest pytest-asyncio pytest-cov ruff mypy pre-commit
   ```
4. **Install pre-commit hooks:**
   ```bash
   uv run pre-commit install
   uv run pre-commit install --hook-type commit-msg
   ```
5. **Copy Environment:**
   ```bash
   cp .env.example .env
   ```
6. **Verify setup:**
   ```bash
   uv run [PROJECT_NAME] health
   uv run pytest tests/ -v
   ```
