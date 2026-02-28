---
title: Configuration
---

# Configuration

DataFlow is configured through a combination of YAML files and environment variables. This section covers all available settings.

## Configuration sources

DataFlow reads configuration from these sources, in order of precedence (highest first):

```mermaid
graph TD
    A["Command-line flags"] --> E[Final Config]
    B["Environment variables"] --> E
    C["dataflow.yml"] --> E
    D["~/.dataflow/config.yml"] --> E

    style A fill:#f87171,stroke:#dc2626,color:#fff
    style D fill:#93c5fd,stroke:#3b82f6,color:#fff
```

1. **Command-line flags** — override everything (`--batch-size 1000`)
2. **Environment variables** — per-environment settings (`DATAFLOW_BATCH_SIZE=1000`)
3. **Project config** — `dataflow.yml` in the project root
4. **User config** — `~/.dataflow/config.yml` for machine-wide defaults

## Guides

- [[configuration/config-file|Configuration File Reference]] — complete `dataflow.yml` reference
- [[configuration/environment-variables|Environment Variables]] — managing secrets and per-environment settings

> [!tip] Getting started?
> For most projects, the defaults work well. Start with a minimal `dataflow.yml` and add settings as needed.
