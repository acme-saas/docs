---
title: Connectors
---

# Connectors

Connectors are adapters that let DataFlow read from sources and write to destinations. Each connector handles the specifics of a particular system — authentication, data formats, pagination, etc.

## Available connectors

### Sources

| Connector | Type | Incremental | Streaming |
|-----------|------|:-----------:|:---------:|
| PostgreSQL | `postgres` | Yes | Yes (CDC) |
| MySQL | `mysql` | Yes | No |
| MongoDB | `mongodb` | Yes | Yes |
| CSV | `csv` | No | No |
| JSON | `json` | No | No |
| Parquet | `parquet` | No | No |
| REST API | `rest` | Configurable | No |
| Kafka | `kafka` | N/A | Yes |
| Redis Streams | `redis_stream` | N/A | Yes |
| S3 | `s3` | Yes | No |
| Google Sheets | `google_sheets` | No | No |

### Destinations

| Connector | Type | Write modes |
|-----------|------|-------------|
| BigQuery | `bigquery` | append, replace, upsert, merge |
| Snowflake | `snowflake` | append, replace, upsert |
| PostgreSQL | `postgres` | append, replace, upsert |
| S3 | `s3` | append, replace |
| JSON | `json` | append, replace |
| Webhook | `webhook` | append |
| Elasticsearch | `elasticsearch` | append, upsert |

## Connector configuration

Each connector requires specific configuration. Here's a typical database connector:

```yaml
sources:
  - type: postgres
    name: users_db
    connection: ${DATABASE_URL}
    query: "SELECT * FROM users WHERE updated_at > :last_run"
    timeout: 60
    batch_size: 10000
```

### Connection strings

> [!tip] Use environment variables for secrets
> Never hardcode credentials in your pipeline files. Use `${VARIABLE_NAME}` to reference environment variables. See [[configuration/environment-variables|Environment Variables]].

```yaml
# PostgreSQL
connection: postgresql://user:pass@host:5432/dbname

# MySQL
connection: mysql://user:pass@host:3306/dbname

# MongoDB
connection: mongodb://user:pass@host:27017/dbname
```

## Building custom connectors

If DataFlow doesn't support your data source, you can build a custom connector:

```python
# connectors/my_custom_source.py
from dataflow.sdk import SourceConnector, Row

class MyCustomSource(SourceConnector):
    """Read data from a custom API."""

    def setup(self, config):
        self.api_url = config["api_url"]
        self.api_key = config["api_key"]

    def extract(self, last_run=None):
        response = self.http.get(
            self.api_url,
            headers={"Authorization": f"Bearer {self.api_key}"},
            params={"since": last_run}
        )
        for item in response.json()["data"]:
            yield Row(item)

    def teardown(self):
        pass
```

Register it in your configuration:

```yaml
plugins:
  connectors:
    - path: ./connectors/my_custom_source.py
      type: my_custom_source

sources:
  - type: my_custom_source
    api_url: https://api.example.com/data
    api_key: ${MY_API_KEY}
```

> [!note]
> Custom connectors have full access to the DataFlow SDK. See the [[api-reference/connector|Connector API]] for the complete interface.

## Health checks

DataFlow periodically checks connector health. You can see the status with:

```bash
dataflow connectors status
```

```
┌─────────────┬──────────┬─────────┬──────────────────┐
│ Connector   │ Type     │ Status  │ Last Check       │
├─────────────┼──────────┼─────────┼──────────────────┤
│ users_db    │ postgres │ healthy │ 2 minutes ago    │
│ analytics   │ bigquery │ healthy │ 2 minutes ago    │
│ events      │ kafka    │ warning │ 5 minutes ago    │
└─────────────┴──────────┴─────────┴──────────────────┘
```

## Related

- [[concepts/architecture|Architecture]] — how connectors fit into the pipeline engine
- [[api-reference/connector|Connector API]] — full API reference for building connectors
- [[guides/connecting-databases|Connecting Databases Guide]] — step-by-step setup for common databases
