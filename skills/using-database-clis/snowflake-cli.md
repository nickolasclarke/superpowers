# Snowflake CLI (snow) Reference

## SQL Documentation

Full SQL command reference: https://docs.snowflake.com/en/sql-reference/sql-all

## Query Execution

```bash
# Single query
snow sql -q "SELECT * FROM table LIMIT 10"

# From file
snow sql -f query.sql

# From stdin
cat query.sql | snow sql -i

# With variables
snow sql -q "SELECT * FROM &{table}" -D table=my_table
```

## Schema Exploration

Two approaches - CLI commands or SQL:

| Task | CLI Command | SQL Alternative |
|------|-------------|-----------------|
| List databases | `snow object list database` | `snow sql -q "SHOW DATABASES"` |
| List schemas | `snow object list schema --in database X` | `snow sql -q "SHOW SCHEMAS IN DATABASE X"` |
| List tables | `snow object list table --in schema X.Y` | `snow sql -q "SHOW TABLES IN SCHEMA X.Y"` |
| Find tables | `snow object list table -l '%CUSTOMER%'` | `snow sql -q "SHOW TABLES LIKE '%CUSTOMER%'"` |
| Describe table | `snow object describe table X.Y.Z` | `snow sql -q "DESCRIBE TABLE X.Y.Z"` |
| Sample data | - | `snow sql -q "SELECT * FROM X.Y.Z LIMIT 10"` |

## Key Flags

| Flag | Scope | Purpose |
|------|-------|---------|
| `-q "query"` | `snow sql` | Execute single query |
| `-f file.sql` | `snow sql` | Execute SQL file |
| `-i` | `snow sql` | Read from stdin |
| `-D key=value` | `snow sql` | Variable substitution |
| `--format TABLE\|JSON\|CSV` | Global | Output format (default: TABLE) |
| `--silent` | Global | Suppress progress messages |
| `-c name` | Global | Use named connection |
| `--database X` | Connection | Override default database |
| `--schema X` | Connection | Override default schema |

## Output Formats

```bash
# Human-readable table (default)
snow sql -q "SELECT * FROM t LIMIT 10" --format TABLE

# For programmatic processing
snow sql -q "SELECT * FROM t LIMIT 10" --format JSON

# For export
snow sql -q "SELECT * FROM t" --format CSV > output.csv
```

## Typical Exploration Session

```bash
# 0. Check current context (database/schema/warehouse)
snow sql -q "SELECT CURRENT_DATABASE(), CURRENT_SCHEMA(), CURRENT_WAREHOUSE()"

# 1. What databases exist?
snow object list database

# 2. What schemas in my database?
snow object list schema --in database ANALYTICS

# 3. Find customer-related tables
snow object list table --in schema ANALYTICS.PUBLIC -l '%CUSTOMER%'

# 4. What columns does it have?
snow object describe table ANALYTICS.PUBLIC.CUSTOMERS

# 5. Sample the data
snow sql -q "SELECT * FROM ANALYTICS.PUBLIC.CUSTOMERS LIMIT 10"
```

**Note:** Snowflake identifiers are case-insensitive and stored uppercase unless quoted. Pattern `'%customer%'` and `'%CUSTOMER%'` are equivalent.
