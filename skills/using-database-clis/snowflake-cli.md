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

Two approaches - CLI commands or SQL.

**Note:** Prefer `snow sql` over `snow object` commands. The `snow object list` TABLE output is often unreadable with wide columns. SQL queries with `--format JSON` are more reliable.

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
# Recommended default - reliable for any schema
snow sql -q "SELECT * FROM t LIMIT 10" --format JSON

# Human-readable - ONLY for narrow results (few short columns)
snow sql -q "SELECT day_date, value FROM t LIMIT 10" --format TABLE

# For export
snow sql -q "SELECT * FROM t" --format CSV > output.csv
```

**TABLE format limitations:** Columns wrap and mangle when data is wide. Use JSON for tables with many columns or long string values.

## Typical Exploration Session

```bash
# 0. Check current context (database/schema/warehouse)
snow sql -q "SELECT CURRENT_DATABASE(), CURRENT_SCHEMA(), CURRENT_WAREHOUSE()"

# 1. What databases exist?
snow sql -q "SHOW DATABASES" --format JSON

# 2. What schemas in my database?
snow sql -q "SHOW SCHEMAS IN DATABASE ANALYTICS" --format JSON

# 3. Find customer-related tables
snow sql -q "SHOW TABLES IN SCHEMA ANALYTICS.PUBLIC LIKE '%CUSTOMER%'" --format JSON

# 4. What columns does it have?
snow sql -q "DESCRIBE TABLE ANALYTICS.PUBLIC.CUSTOMERS" --format JSON

# 5. Sample the data
snow sql -q "SELECT * FROM ANALYTICS.PUBLIC.CUSTOMERS LIMIT 10" --format JSON
```

**Note:** Snowflake identifiers are case-insensitive and stored uppercase unless quoted. Pattern `'%customer%'` and `'%CUSTOMER%'` are equivalent.
