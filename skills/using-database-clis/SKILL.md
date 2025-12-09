---
name: using-database-clis
description: Use when exploring database schemas, running queries, or answering data questions via CLI tools like Snowflake's snow or BigQuery's bq - covers workflow patterns, output handling, and avoiding context overflow from large results
---

# Using Database CLIs

## Overview

Execute SQL queries and explore schemas using database CLI tools. Assumes CLI is pre-configured (auth handled). Focus on efficient exploration and safe result handling.

**Core principle:** Always limit results to avoid context overflow. Explore schema before writing complex queries.

## When to Use

- Investigating data structure for dbt development
- Answering questions about data using plain language
- Validating query logic or model output
- Exploring unfamiliar schemas

## Workflow

### 1. Schema Discovery

Start broad, narrow down:
1. List databases/schemas available
2. Find relevant tables (search by name pattern)
3. Inspect table structure (columns, types)
4. Sample data to understand content

### 2. Query Execution

**Defaults for exploration:**
- Always use `LIMIT 100` unless you need more
- Use `--format JSON` for reliable output (TABLE format mangles wide columns)
- Use `--format TABLE` only for narrow result sets (few columns, short values)
- Count rows first if result size is uncertain

**For large results:**
- Export to file with `--format CSV`
- Or aggregate/summarize instead of raw data

### 3. Execution Modes

| Mode | Command | Use For |
|------|---------|---------|
| Single query | `<cli> -q "SELECT..."` | Agent workflows, one-off queries |
| SQL file | `<cli> -f file.sql` | Multi-statement scripts |
| REPL | `<cli>` (no args) | Human interactive sessions (not for agents) |

## CLI References

| Database | CLI | Reference |
|----------|-----|-----------|
| Snowflake | `snow` | See snowflake-cli.md |
| BigQuery | `bq` | (coming soon) |

## Common Mistakes

**Forgetting LIMIT:** Query returns 100k rows, floods context
**Fix:** Default to `LIMIT 100`, increase only if needed

**Wrong output format:** TABLE mangles wide data, JSON too verbose for simple queries
**Fix:** Use `--format JSON` by default; TABLE only for narrow results (3-4 short columns)

**Querying before exploring:** Write complex query on wrong table
**Fix:** DESCRIBE table and sample data first
