---
title: Project Structure
---

# Project Structure

A Acme project is a directory containing configuration files, transform definitions, and optional test fixtures.

## Default layout

```
my-project/
├── acme.yml           # Main pipeline configuration
├── pipelines/             # Additional pipeline definitions
│   ├── users.yml
│   ├── orders.yml
│   └── analytics.yml
├── transforms/            # Custom transform functions
│   ├── __init__.py
│   ├── normalize.py
│   └── enrich.py
├── tests/                 # Pipeline tests
│   ├── test_users.yml
│   └── fixtures/
│       └── sample_users.csv
├── .env                   # Environment variables (not committed)
├── .env.example           # Example environment file
└── .acme/             # Local state (auto-generated)
    ├── runs/              # Pipeline run history
    └── cache/             # Connector cache
```

## Key files

### `acme.yml`

The main configuration file. Defines sources, transforms, destinations, and scheduling.

```yaml
name: my-project
version: "1.0"

defaults:
  batch_size: 5000
  retry_count: 3
  timeout: 300

sources:
  - type: postgres
    name: main_db
    connection: ${DATABASE_URL}
```

See [[configuration/config-file|Configuration Reference]] for all available options.

### `pipelines/` directory

For projects with multiple pipelines, you can split them into separate files:

```mermaid
graph TD
    A[acme.yml] -->|defaults| B[pipelines/users.yml]
    A -->|defaults| C[pipelines/orders.yml]
    A -->|defaults| D[pipelines/analytics.yml]

    B --> E[users_db]
    C --> F[orders_db]
    D --> G[bigquery]
```

Each pipeline file inherits the `defaults` from `acme.yml` but can override any setting.

### `transforms/` directory

Custom Python functions for data transformations. See [[concepts/transforms|Transforms]] for details.

```python
# transforms/normalize.py
def normalize_email(row):
    """Lowercase and strip whitespace from email addresses."""
    row["email"] = row["email"].lower().strip()
    return row
```

### `.env` file

> [!danger] Never commit `.env` files
> The `.env` file contains secrets like database passwords and API keys. Always add it to `.gitignore`.

```bash
DATABASE_URL=postgresql://user:pass@localhost:5432/mydb
API_KEY=sk-abc123...
SLACK_WEBHOOK=https://hooks.slack.com/services/...
```

## Next steps

- [[getting-started/first-pipeline|Your First Pipeline]] — a hands-on tutorial
- [[configuration/environment-variables|Environment Variables]] — managing secrets and configuration
