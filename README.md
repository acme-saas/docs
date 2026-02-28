---
title: DataFlow Documentation
description: The modern data pipeline platform for teams that move fast.
---

# DataFlow

**DataFlow** is an open-source data pipeline platform that makes it easy to build, test, and deploy data workflows. Connect any source, transform your data, and deliver it anywhere — all with simple configuration files.

> [!tip] New to DataFlow?
> Start with the [[getting-started/quickstart|Quickstart Guide]] to build your first pipeline in under 5 minutes.

## Why DataFlow?

- **Simple by default** — YAML config files, not code
- **Powerful when needed** — custom transforms in Python or JavaScript
- **Built for teams** — version control, testing, and CI/CD friendly
- **Real-time ready** — batch and streaming in one platform

## How it works

```mermaid
graph LR
    A[Sources] -->|Extract| B[DataFlow Engine]
    B -->|Transform| C[Processing]
    C -->|Load| D[Destinations]

    style A fill:#818cf8,stroke:#4f46e5,color:#fff
    style B fill:#34d399,stroke:#059669,color:#fff
    style C fill:#fbbf24,stroke:#d97706,color:#fff
    style D fill:#f87171,stroke:#dc2626,color:#fff
```

## Quick links

| Section                                        | Description                     |
| ---------------------------------------------- | ------------------------------- |
| [[getting-started/installation\|Installation]] | Install DataFlow on your system |
| [[getting-started/quickstart\|Quickstart]]     | Build your first pipeline       |
| [[concepts/architecture\|Architecture]]        | Understand how DataFlow works   |
| [[guides/connecting-databases\|Guides]]        | Step-by-step tutorials          |
| [[api-reference/client\|API Reference]]        | Complete API documentation      |

## Example pipeline

```yaml
# dataflow.yml
name: user-analytics
version: "1.0"

sources:
  - type: postgres
    name: users_db
    connection: ${DATABASE_URL}
    query: "SELECT * FROM users WHERE updated_at > :last_run"

transforms:
  - type: filter
    condition: "status = 'active'"
  - type: map
    fields:
      full_name: "first_name || ' ' || last_name"
      signup_year: "EXTRACT(YEAR FROM created_at)"

destinations:
  - type: bigquery
    dataset: analytics
    table: active_users
    write_mode: upsert
    key: user_id
```

> [!info] Looking for the changelog?
> See [[changelog|what's new in DataFlow v2.4]].
