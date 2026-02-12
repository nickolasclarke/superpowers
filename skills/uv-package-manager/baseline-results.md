# Baseline Test Results (Without UV Skill)

## Summary
Agents generally know the basic `uv add` command but lack confidence and specific syntax knowledge. They express uncertainties and suggest non-idiomatic fallbacks.

## Scenario 1: Adding Packages
**What agent got right:**
- Core command: `uv add requests pydantic`

**Gaps and uncertainties:**
- Lock file format and location
- Whether `uv add` automatically syncs
- Virtual environment handling
- Suggested fallback to `uv pip install` (not idiomatic)

## Scenario 2: Creating New Project
**What agent got right:**
- `uv init api-client`
- `uv add fastapi httpx`

**Gaps and uncertainties:**
- Exact project structure created
- Virtual environment creation/activation
- Python version specification
- Asked for permission instead of searching docs

## Scenario 3: Version Constraints & Dev Dependencies
**What agent got right:**
- Version constraint syntax: `"redis>=4.0,<5.0"`

**Gaps and uncertainties:**
- Dev dependency flag: guessed `--dev` but only 60% confident
- Could be `--group dev`, `-D`, `--optional dev`, or something else
- Doesn't know pyproject.toml structure uv uses
- Expressed significant uncertainty (60% confidence)

## Common Patterns Across All Scenarios

### Uncertainty Symptoms
1. Agents express multiple uncertainties per scenario
2. Suggest "fallback" approaches that aren't idiomatic
3. Ask for permission/clarification instead of being confident
4. Mention searching docs but don't know what's actually correct

### Missing Knowledge
1. **Specific flags**: `--dev` vs `--group` for dependency groups
2. **Lock file management**: `uv lock`, `uv sync` workflows
3. **Running commands**: `uv run` for scripts and one-off execution
4. **Version constraint shortcuts**: `~=` for compatible release
5. **Project structure**: What `uv init` actually creates
6. **Virtual environment**: When/if activation is needed

## What the Skill Needs to Provide

1. **Quick reference table** of common commands with exact flags
2. **Version constraint syntax** with examples
3. **Dependency groups** (dev, optional, etc.) with correct syntax
4. **Lock file management** commands
5. **Running scripts** and one-off commands
6. **Project initialization** workflow
7. **Removal and cleanup** commands
8. **Update workflows** for dependencies

## Success Criteria for Skill

After reading the skill, agents should:
- Use correct flags without expressing uncertainty
- Know the difference between `uv add`, `uv sync`, `uv lock`, `uv run`
- Handle version constraints confidently
- Not suggest `uv pip install` as a fallback
- Know when/how to use dependency groups
