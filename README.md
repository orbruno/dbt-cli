# dbt-cli

Minimal dbt CLI wrapper with PostgreSQL and BigQuery adapters for use in Docker environments.

## Purpose

This repository provides a uv-compatible dbt installation that includes:
- **dbt-core**: Core dbt functionality
- **dbt-postgres**: PostgreSQL adapter (for Supabase, standard PostgreSQL)
- **dbt-bigquery**: BigQuery adapter (for Google BigQuery)

## Usage

### With uv (recommended)

```bash
# Install
uv sync

# Run dbt commands
uv run dbt --version
uv run dbt init my_project
uv run dbt run --profiles-dir ./profiles --project-dir ./project
```

### In Docker (vps-stack)

When running inside the vps-stack cli-tools container:

```bash
# Check version
dbt --version

# Run transformations
dbt run --profiles-dir /data/shared/dbt --project-dir /data/shared/dbt-project

# Test models
dbt test --profiles-dir /data/shared/dbt --project-dir /data/shared/dbt-project

# Generate docs
dbt docs generate --profiles-dir /data/shared/dbt --project-dir /data/shared/dbt-project
```

## Configuration

dbt requires a `profiles.yml` file for database connections. Example:

```yaml
# profiles.yml
turri:
  target: dev
  outputs:
    dev:
      type: postgres
      host: "{{ env_var('SUPABASE_HOST') }}"
      user: postgres
      password: "{{ env_var('SUPABASE_DB_PASSWORD') }}"
      port: 5432
      dbname: postgres
      schema: mdm
      threads: 4

    bigquery:
      type: bigquery
      method: service-account
      project: "{{ env_var('BIGQUERY_PROJECT') }}"
      dataset: turri_staging
      keyfile: /data/shared/credentials/bigquery-sa.json
      threads: 4
```

## Related

- [vps-stack](https://github.com/orbruno/vps-stack) - Docker stack that includes this tool
- [dbt documentation](https://docs.getdbt.com/)

---

**Last Updated**: 2026-02-04
