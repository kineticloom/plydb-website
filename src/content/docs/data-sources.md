---
title: Configuring Data Sources
weight: 2
---

Data sources are configured via a JSON config file that you pass to PlyDB with the `--config` flag. You can configure multiple data sources in a single file and query across all of them.

{{< callout type="tip" >}}
If you have the [PlyDB Agent Skill](/docs/agent-integration/#cli-agent-skill) installed, you can ask your agent to help you with data source configuration instead of writing a config file manually.
{{< /callout >}}

## Config file structure

The config file has three top-level objects:

- **`credentials`** — Shared authentication profiles for cloud providers (S3, Google Sheets).
- **`databases`** — A map of data source configurations. Each key is a unique identifier that becomes the catalog name in SQL queries.
- **`semantic_context`** — (Optional) Semantic context overlay configuration.

Database keys must consist of lowercase letters, digits, and underscores only.

## PostgreSQL

```json
{
  "databases": {
    "analytics": {
      "metadata": {
        "name": "Production Analytics",
        "description": "Primary read-replica for data warehousing."
      },
      "type": "postgresql",
      "host": "db-prod-01.example.com",
      "port": 5432,
      "database_name": "analytics_main",
      "username": "bi_user",
      "password_env_var": "DB_PROD_PASSWORD"
    }
  }
}
```

| Field              | Type    | Description                                     |
| :----------------- | :------ | :---------------------------------------------- |
| `host`             | String  | Server address                                  |
| `port`             | Integer | Network port                                    |
| `database_name`    | String  | Target database name                            |
| `username`         | String  | Login identity                                  |
| `password_env_var` | String  | Name of the environment variable holding the password |

Tables are referenced as `catalog.schema.table`, e.g. `analytics.public.orders`.

## MySQL

MySQL uses the same fields as PostgreSQL:

```json
{
  "databases": {
    "app": {
      "metadata": {
        "name": "Application Database",
        "description": "Main application MySQL instance."
      },
      "type": "mysql",
      "host": "mysql.example.com",
      "port": 3306,
      "database_name": "myapp",
      "username": "readonly",
      "password_env_var": "MYSQL_PASSWORD"
    }
  }
}
```

## SQLite

```json
{
  "databases": {
    "shop": {
      "metadata": {
        "name": "Shop Database",
        "description": "SQLite database backing the app."
      },
      "type": "sqlite",
      "path": "/data/shop.sqlite"
    }
  }
}
```

| Field  | Type   | Description                          |
| :----- | :----- | :----------------------------------- |
| `path` | String | Path to the SQLite database file     |

SQLite uses `main` as its default schema, so tables are referenced as `shop.main.customers`.

## DuckDB

```json
{
  "databases": {
    "analytics": {
      "metadata": {
        "name": "Analytics Database",
        "description": "DuckDB with pre-aggregated metrics."
      },
      "type": "duckdb",
      "path": "/data/analytics.duckdb"
    }
  }
}
```

| Field  | Type   | Description                          |
| :----- | :----- | :----------------------------------- |
| `path` | String | Path to the DuckDB database file     |

Like SQLite, DuckDB uses `main` as its default schema.

## Local files (CSV, JSON, Parquet, Excel)

```json
{
  "databases": {
    "budget": {
      "metadata": {
        "name": "FY2026 Budget",
        "description": "Department budget allocations."
      },
      "type": "file",
      "path": "/Documents/Finance/budget_2026.xlsx",
      "sheet_name": "Final_Approval",
      "header_row": true
    }
  }
}
```

| Field        | Type    | Description                                         |
| :----------- | :------ | :-------------------------------------------------- |
| `path`       | String  | Path to the file                                    |
| `format`     | String  | (Optional) `csv`, `xlsx`, `parquet`, or `json`. Inferred from extension if omitted. |
| `delimiter`  | String  | (CSV only) Separator character                      |
| `header_row` | Boolean | (CSV/XLSX) Whether row 1 is the header              |
| `sheet_name` | String  | (XLSX only) Tab name to read                        |

## S3

```json
{
  "credentials": {
    "aws_user": {
      "access_key_env": "AWS_ACCESS_KEY_ID",
      "secret_key_env": "AWS_SECRET_ACCESS_KEY"
    }
  },
  "databases": {
    "sensor_data": {
      "metadata": {
        "name": "IoT Sensor Data",
        "description": "Partitioned sensor data in S3."
      },
      "type": "s3",
      "credential_profile": "aws_user",
      "uri": "s3://iot-bucket/2026/*/sensor_*.parquet",
      "format": "parquet",
      "region": "us-west-2"
    }
  }
}
```

| Field                | Type    | Description                                         |
| :------------------- | :------ | :-------------------------------------------------- |
| `uri`                | String  | S3 URI. Supports glob patterns (`*`, `?`, `[]`).    |
| `credential_profile` | String  | Key matching an entry in the `credentials` map      |
| `region`             | String  | AWS region (e.g. `us-east-1`)                       |
| `format`             | String  | **Required.** File format (`csv`, `parquet`, etc.)   |
| `delimiter`          | String  | (CSV only) Separator character                      |
| `header_row`         | Boolean | (CSV/XLSX) Whether row 1 is the header              |

Credentials are defined in the top-level `credentials` object and referenced by name via `credential_profile`.

## Google Sheets

PlyDB supports two authentication methods for Google Sheets:

- **Service account** — for server-side, non-interactive use.
- **Browser OAuth** — for interactive, ad-hoc use. PlyDB opens a browser for Google login with no credentials needed in the config.

### Service account

```json
{
  "credentials": {
    "gsheet_sa": {
      "key_file": "/etc/secrets/gsheet-sa-key.json"
    }
  },
  "databases": {
    "sales": {
      "metadata": {
        "name": "Sales Forecast",
        "description": "Q3 sales forecast."
      },
      "type": "gsheet",
      "spreadsheet_id": "1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms",
      "credential_profile": "gsheet_sa",
      "sheet_name": "Forecast",
      "header_row": true
    }
  }
}
```

### Browser OAuth

```json
{
  "databases": {
    "sales": {
      "metadata": {
        "name": "Personal Tracker",
        "description": "Ad-hoc sheet accessed via browser OAuth."
      },
      "type": "gsheet",
      "spreadsheet_id": "1AbCdEfGhIjKlMnOpQrStUvWxYz0123456789abcdefg"
    }
  }
}
```

On first query, PlyDB opens your browser for Google login. The OAuth token is cached at `~/.duckdb/stored_secrets/` and reused automatically. To force re-authentication, run `plydb auth --config /path/to/config.json`.

| Field                | Type    | Description                                         |
| :------------------- | :------ | :-------------------------------------------------- |
| `spreadsheet_id`     | String  | The spreadsheet ID from the Google Sheets URL        |
| `credential_profile` | String  | (Optional) Key referencing a credential with `key_file`. Omit for browser OAuth. |
| `sheet_name`         | String  | (Optional) Tab name. If omitted, PlyDB uses the table name from the SQL query.  |
| `header_row`         | Boolean | (Optional) Whether row 1 is the header. Defaults to `true`. |

### Dynamic sheet names

When `sheet_name` is omitted, the table name in your SQL query determines which tab is read:

```sh
# Read from the "January" tab
plydb query --config config.json \
  "SELECT * FROM sales.default.\"January\""

# Read from the "February" tab
plydb query --config config.json \
  "SELECT * FROM sales.default.\"February\""
```

{{< callout type="warning" >}}
SQL lowercases unquoted identifiers. If your sheet tab name contains uppercase letters, either quote the identifier with double quotes (e.g. `"Revenue"`) or set `sheet_name` in the config to bypass SQL identifier parsing.
{{< /callout >}}

## Cross-source queries

You can join across any combination of data sources in a single query. Just add all sources to the same config file:

```sh
plydb query --config config.json \
  "SELECT c.name, o.product, o.amount
   FROM store.public.orders o
   JOIN products.default.\"table\" p ON o.product_id = p.id"
```

## Semantic context

PlyDB can automatically scan your data sources and provide AI agents with structured semantic context. See the dedicated [Semantic Context](/docs/semantic-context/) page for details on automatic discovery, overlays, and how agents build institutional knowledge over time.

## Full reference example

Here is a complete config file demonstrating multiple source types:

```json
{
  "credentials": {
    "aws_user": {
      "access_key_env": "AWS_ACCESS_KEY_ID",
      "secret_key_env": "AWS_SECRET_ACCESS_KEY"
    },
    "gsheet_sa": {
      "key_file": "/etc/secrets/gsheet-sa-key.json"
    }
  },
  "databases": {
    "analytics": {
      "metadata": {
        "name": "Production Analytics",
        "description": "Primary read-replica."
      },
      "type": "postgresql",
      "host": "db-prod-01.example.com",
      "port": 5432,
      "database_name": "analytics_main",
      "username": "bi_user",
      "password_env_var": "DB_PROD_PASSWORD"
    },
    "budget": {
      "metadata": {
        "name": "FY2026 Budget",
        "description": "Department budget allocations."
      },
      "type": "file",
      "path": "/Documents/Finance/budget_2026.xlsx",
      "sheet_name": "Final_Approval",
      "header_row": true
    },
    "sensor_data": {
      "metadata": {
        "name": "IoT Sensor Data",
        "description": "Partitioned sensor data."
      },
      "type": "s3",
      "credential_profile": "aws_user",
      "uri": "s3://iot-bucket/2026/*/sensor_*.parquet",
      "format": "parquet",
      "region": "us-west-2"
    },
    "sales": {
      "metadata": {
        "name": "Sales Forecast",
        "description": "Q3 sales forecast."
      },
      "type": "gsheet",
      "spreadsheet_id": "1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms",
      "credential_profile": "gsheet_sa",
      "sheet_name": "Forecast",
      "header_row": true
    }
  },
  "semantic_context": {
    "overlays": [
      "/path/to/business_glossary.yaml"
    ]
  }
}
```
