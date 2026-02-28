---
title: Client API
---

# Client API

The DataFlow client is the entry point for all SDK interactions. Initialize it once and use it across your application.

## Installation

```bash
pip install dataflow-sdk
```

## Initialization

```python
from dataflow import DataFlowClient

# Using an API key
client = DataFlowClient(
    base_url="https://dataflow.example.com",
    api_key="df_key_abc123..."
)

# Using OAuth token
client = DataFlowClient(
    base_url="https://dataflow.example.com",
    token="eyJhbGciOiJSUzI1NiIs..."
)

# Using environment variables
# Reads DATAFLOW_URL and DATAFLOW_API_KEY
client = DataFlowClient.from_env()
```

## Configuration options

```python
client = DataFlowClient(
    base_url="https://dataflow.example.com",
    api_key="df_key_abc123...",
    timeout=30,           # Request timeout in seconds
    max_retries=3,        # Auto-retry failed requests
    retry_delay=1.0,      # Delay between retries
    verify_ssl=True,      # SSL certificate verification
)
```

## Basic usage

### List pipelines

```python
pipelines = client.pipelines.list()

for p in pipelines:
    print(f"{p.name} — {p.status} — {p.last_run_at}")
```

### Trigger a run

```python
run = client.pipelines.run("user-analytics")
print(f"Run started: {run.id}")

# Wait for completion
result = run.wait()
print(f"Status: {result.status}")
print(f"Rows processed: {result.rows_loaded}")
```

### Check pipeline status

```python
status = client.pipelines.get("user-analytics")
print(f"Status: {status.status}")
print(f"Last run: {status.last_run_at}")
print(f"Next run: {status.next_run_at}")
```

## JavaScript/TypeScript

```typescript
import { DataFlowClient } from "@dataflow/sdk";

const client = new DataFlowClient({
  baseUrl: "https://dataflow.example.com",
  apiKey: process.env.DATAFLOW_API_KEY,
});

// List pipelines
const pipelines = await client.pipelines.list();

// Trigger a run
const run = await client.pipelines.run("user-analytics");
const result = await run.wait();

console.log(`Processed ${result.rowsLoaded} rows`);
```

## Error handling

```python
from dataflow.exceptions import (
    DataFlowError,
    AuthenticationError,
    PipelineNotFoundError,
    RateLimitError,
)

try:
    result = client.pipelines.run("my-pipeline")
except AuthenticationError:
    print("Invalid API key")
except PipelineNotFoundError:
    print("Pipeline does not exist")
except RateLimitError as e:
    print(f"Rate limited. Retry after {e.retry_after}s")
except DataFlowError as e:
    print(f"Unexpected error: {e}")
```

> [!tip] Auto-retry
> The client automatically retries transient errors (5xx, timeouts) up to `max_retries` times. You only need to handle business logic errors.

## Related

- [[api-reference/pipeline|Pipeline API]] — pipeline operations
- [[api-reference/events|Events API]] — subscribe to pipeline events
- [[guides/authentication|Authentication]] — auth setup
