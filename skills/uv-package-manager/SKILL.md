---
name: uv-package-manager
description: Use when working with Python projects using uv (Astral's package manager) for adding/removing dependencies, initializing projects, version constraints, or dependency groups - provides command reference and idiomatic workflows
---

# UV Package Manager

## Overview

**uv** is Astral's fast Python package manager and project manager. Use `uv add`, `uv remove`, `uv sync`, and `uv run` for all package operations - never fall back to pip or manual pyproject.toml editing.

## Quick Reference

| Task | Command | Notes |
|------|---------|-------|
| Add packages | `uv add requests pydantic` | Updates pyproject.toml and lockfile |
| Add with version | `uv add "redis>=4.0,<5.0"` | Use PEP 440 version specifiers |
| Add compatible version | `uv add "redis~=4.0"` | Allows 4.x, blocks 5.x |
| Add dev dependency | `uv add --dev pytest ruff` | Adds to dev dependency group |
| Add optional group | `uv add --optional docs sphinx` | Creates optional dependency group |
| Remove package | `uv remove pandas` | Updates pyproject.toml and lockfile |
| Initialize project | `uv init my-project` | Creates project structure with pyproject.toml |
| Install all deps | `uv sync` | Installs from lockfile, removes extras |
| Update lockfile | `uv lock --upgrade` | Updates dependencies to latest compatible |
| Run script | `uv run python script.py` | Executes in project environment |
| Run one-off command | `uv run python -c "print('hello')"` | No venv activation needed |
| Show installed | `uv pip list` | List packages in environment |

## Common Workflows

### Creating New Project

```bash
# Initialize project with structure
uv init my-api

# Navigate into project
cd my-api

# Add dependencies
uv add fastapi uvicorn httpx

# Add dev dependencies
uv add --dev pytest pytest-asyncio ruff
```

### Version Constraints

```python
# Exact version
uv add "requests==2.31.0"

# Minimum version
uv add "requests>=2.31.0"

# Version range
uv add "requests>=2.31.0,<3.0.0"

# Compatible release (recommended for semver)
uv add "requests~=2.31.0"  # Allows 2.31.x, blocks 2.32+
```

### Dependency Groups

uv organizes dependencies into groups in pyproject.toml:

```toml
[project]
dependencies = [
    "requests>=2.31.0",
    "pydantic>=2.0.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "ruff>=0.1.0",
]
docs = [
    "sphinx>=7.0.0",
]
```

Add to groups:

```bash
# Dev dependencies
uv add --dev pytest ruff mypy

# Optional group
uv add --optional docs sphinx sphinx-rtd-theme

# Install with optional groups
uv sync --extra docs
```

### Lock File Management

```bash
# Update all dependencies to latest compatible versions
uv lock --upgrade

# Update specific package
uv lock --upgrade-package requests

# Sync environment with lockfile (removes unlocked packages)
uv sync

# Sync with optional groups
uv sync --extra dev --extra docs
```

## Running Code

uv handles environment activation automatically:

```bash
# Run Python script
uv run python app/main.py

# Run script with arguments
uv run python script.py --verbose --config config.json

# Run module
uv run -m pytest

# Run module with arguments
uv run -m pytest tests/ -v --cov

# One-off command
uv run python -c "import sys; print(sys.version)"

# Run project script (defined in pyproject.toml)
uv run --script dev
```

**Never manually activate virtual environments** - `uv run` handles this.

## Project Initialization

```bash
# Create new project with standard structure
uv init my-project
# Creates:
# my-project/
#   pyproject.toml
#   README.md
#   src/my_project/__init__.py

# Initialize in existing directory
cd existing-project
uv init

# Specify Python version
uv init --python 3.11 my-project
```

## Common Mistakes

| Mistake | Why Wrong | Correct Approach |
|---------|-----------|------------------|
| `uv pip install package` | Not idiomatic, doesn't update pyproject.toml | `uv add package` |
| Manually editing pyproject.toml | Easy to break dependency resolution | Use `uv add` / `uv remove` |
| `pip install` in uv project | Bypasses uv's lock file | Always use `uv add` |
| Activating venv manually | Unnecessary with uv | Use `uv run` |
| `--group dev` for dev deps | Wrong flag | Use `--dev` |
| Not using lock file | Loses reproducibility | Commit `uv.lock` to git |
| `uv install` | Wrong command | Use `uv sync` to install from lockfile |

## When NOT to Use uv

- Project explicitly uses pip, poetry, or conda
- Team hasn't adopted uv yet (discuss first)
- Legacy project with complex pip-tools setup

## Migration from Other Tools

```bash
# From requirements.txt
uv add -r requirements.txt

# From poetry (has pyproject.toml)
uv sync  # uv reads existing pyproject.toml

# From pip (manual)
uv add package1 package2 package3  # Add each dependency
```

## Environment Variables

```bash
# Use specific Python version
UV_PYTHON=3.11 uv run python script.py

# Offline mode (use only cache)
UV_NO_NETWORK=1 uv sync
```

## Real-World Example

Complete workflow for new FastAPI project:

```bash
# Create project
uv init my-api
cd my-api

# Add production dependencies
uv add fastapi "uvicorn[standard]" pydantic-settings

# Add dev tools
uv add --dev pytest pytest-asyncio httpx ruff mypy

# Create main app
cat > src/my_api/main.py << 'EOF'
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello World"}
EOF

# Run development server
uv run uvicorn my_api.main:app --reload

# Run tests
uv run -m pytest

# Update dependencies
uv lock --upgrade
uv sync
```

## Additional Resources

- Official docs: `uv --help` and `uv add --help`
- uv supports pip-compatible interface but prefer native commands
- Lock file (`uv.lock`) should be committed to version control
