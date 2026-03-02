---
title: Supported Data Sources
weight: 5
---

PlyDB abstracts the complexity of different storage formats into a single relational view. You can query across any combination of these sources using standard SQL.

For configuration details, see [Configuring Data Sources](/docs/data-sources/).

## SQL Databases

### PostgreSQL

Full support for querying PostgreSQL databases. PlyDB connects over the network using standard credentials. Tables are referenced as `catalog.schema.table` (e.g. `analytics.public.orders`).

PostgreSQL `COMMENT` metadata is automatically extracted for [semantic context](/docs/semantic-context/), giving AI agents rich descriptions of your tables and columns.

### MySQL

Full support for querying MySQL databases with the same network-based connection model as PostgreSQL.

### SQLite

Query SQLite database files directly by path. SQLite uses `main` as its default schema, so tables are referenced as `catalog.main.table`.

### DuckDB

Query DuckDB database files directly by path. Like SQLite, DuckDB uses `main` as its default schema.

## File Formats

PlyDB can query flat files as if they were database tables. The file format is inferred from the extension, or can be set explicitly in the config.

### CSV

Comma-separated (or custom-delimited) value files. Supports configurable delimiter and header row detection.

### JSON

JSON files containing tabular data.

### Parquet

Apache Parquet columnar storage files. Commonly used for large analytical datasets and data lake exports.

### Excel (.xlsx)

Excel workbooks. You can specify which sheet tab to read via the `sheet_name` config field.

## Object Storage

### Amazon S3

Query files stored in S3 buckets. Supports all file formats listed above (CSV, JSON, Parquet, Excel). S3 URIs support glob patterns for matching multiple files:

```
s3://bucket/2026/*/sensor_*.parquet
```

Requires AWS credentials configured via the `credentials` section of the config file.

## SaaS

### Google Sheets

Query Google Sheets spreadsheets using SQL. Two authentication methods are supported:

- **Service account** — for server-side, non-interactive use.
- **Browser OAuth** — for interactive, ad-hoc querying with no credentials in the config.

When `sheet_name` is omitted from the config, the table name in your SQL query determines which tab is read, allowing you to query different tabs without changing configuration.

## Planned

The following sources are on the roadmap but not yet supported:

- **Apache Iceberg** — Open table format for large analytical datasets.
- **Delta Lake** — Storage layer for data lakehouse architectures.
