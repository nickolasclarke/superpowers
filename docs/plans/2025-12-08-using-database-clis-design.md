# Design: using-database-clis Skill

## Purpose

Guide agents through effective use of database CLI tools for schema exploration and query execution. Assumes CLI is pre-configured (auth handled), focuses on workflow patterns and practical CLI usage.

## Use Cases

1. **Schema exploration for dbt development** - Investigating data structure, understanding tables/columns
2. **Answering questions about data** - Running queries to answer plain-language questions

## Structure

```
skills/using-database-clis/
  SKILL.md              # Core workflow patterns, when to use, best practices
  snowflake-cli.md      # snow CLI command reference, flags, examples
  (future: bigquery-cli.md)
```

## SKILL.md Content (~300 words)

### Overview
What this skill is for - exploring schemas, running queries via CLI

### When to Use
- Investigating data structure for dbt development
- Answering questions about data
- Validating query logic

### Core Workflow Patterns
1. Schema discovery (what databases/schemas/tables exist)
2. Structure inspection (columns, types, sample data)
3. Query execution (single queries, multi-statement)
4. Result handling (formats, limiting output)

### Best Practices
- Always LIMIT results to avoid context overflow
- Use appropriate output format for the task (table for human review, JSON/CSV for processing)
- Explore schema before writing complex queries

### Quick Reference Table
Maps common tasks to CLI-specific reference files

## snowflake-cli.md Content (~200 words)

### SQL Documentation
- Link to https://docs.snowflake.com/en/sql-reference/sql-all (master SQL command reference)

### Two Approaches for Schema Exploration

| Task | CLI Command | SQL via `snow sql` |
|------|-------------|-------------------|
| List databases | `snow object list database` | `snow sql -q "SHOW DATABASES"` |
| List schemas | `snow object list schema --in database X` | `snow sql -q "SHOW SCHEMAS IN DATABASE X"` |
| List tables | `snow object list table --in schema X.Y` | `snow sql -q "SHOW TABLES IN SCHEMA X.Y"` |
| Describe table | `snow object describe table X.Y.Z` | `snow sql -q "DESCRIBE TABLE X.Y.Z"` |

### Key Flags

| Flag | Scope | Purpose |
|------|-------|---------|
| `-q "query"` | `snow sql` | Execute single query |
| `-f file.sql` | `snow sql` | Execute SQL file |
| `-i` | `snow sql` | Read query from stdin |
| `--format TABLE\|JSON\|CSV` | Global | Output format |
| `--silent` | Global | Suppress intermediate output |
| `-c connection` | Global | Use named connection |
| `--database`, `--schema` | Connection | Override default db/schema |

### Output Management
- Default to LIMIT 100 for exploration queries
- Use `--format JSON` when processing results programmatically
- Use `--format TABLE` for human review
