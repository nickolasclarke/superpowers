# UV Package Manager Test Scenarios

These scenarios test whether agents can correctly use uv for common Python package management tasks.

## Scenario 1: Adding a Package (Time Pressure)

**Setup:** Python project with pyproject.toml
**Prompt:** "I need to add requests and pydantic to my project right now. I'm using uv. What's the fastest way?"
**Success criteria:** Agent uses `uv add requests pydantic` (not pip install, not manual pyproject.toml editing)

## Scenario 2: Creating New Project

**Setup:** Empty directory
**Prompt:** "Create a new Python project called 'api-client' with FastAPI and httpx as dependencies. Use uv."
**Success criteria:**
- Uses `uv init` or knows to create project structure
- Uses `uv add` to install dependencies
- Doesn't use `pip` or `poetry`

## Scenario 3: Removing Package and Cleanup

**Setup:** Project with multiple dependencies, some unused
**Prompt:** "Remove the 'pandas' package from my project and clean up any dependencies that aren't needed anymore."
**Success criteria:**
- Uses `uv remove pandas`
- Knows about `uv sync` or equivalent for cleanup
- Doesn't manually edit pyproject.toml

## Scenario 4: Running Scripts

**Setup:** Project with a script defined in pyproject.toml
**Prompt:** "How do I run my 'dev' script with uv? And how do I run a one-off Python command?"
**Success criteria:**
- Knows `uv run` command
- Can execute both scripts and one-off commands
- Doesn't suggest activating venv manually

## Scenario 5: Version Constraints

**Setup:** Need specific package versions
**Prompt:** "Add redis but I need version 4.x, not 5.x. Also add pytest but only as a dev dependency."
**Success criteria:**
- Knows how to specify version constraints with uv
- Knows dev dependency syntax
- Uses correct uv commands

## Scenario 6: Update All Dependencies

**Setup:** Project with outdated dependencies
**Prompt:** "Update all my dependencies to their latest compatible versions."
**Success criteria:**
- Uses `uv lock --upgrade` or equivalent
- Understands the difference between lock file and pyproject.toml
- Doesn't suggest pip or manual editing
