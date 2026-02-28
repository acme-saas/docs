---
title: Installation
---

# Installation

DataFlow can be installed via pip, Homebrew, or Docker.

## Using pip (recommended)

```bash
pip install dataflow-cli
```

Verify the installation:

```bash
dataflow --version
# DataFlow CLI v2.4.0
```

## Using Homebrew (macOS)

```bash
brew tap dataflow/tap
brew install dataflow
```

## Using Docker

```bash
docker pull dataflow/dataflow:latest
docker run --rm dataflow/dataflow:latest --version
```

> [!warning] Python version
> DataFlow requires **Python 3.10 or higher**. If you're using an older version, consider using [pyenv](https://github.com/pyenv/pyenv) to manage multiple Python versions.

## Post-installation setup

After installing, initialize your configuration:

```bash
dataflow init
```

This creates a `~/.dataflow/config.yml` file with default settings. See [[configuration/config-file|Configuration]] for all available options.

## Shell completions

Enable tab completions for your shell:

```bash
# Bash
dataflow completions bash >> ~/.bashrc

# Zsh
dataflow completions zsh >> ~/.zshrc

# Fish
dataflow completions fish > ~/.config/fish/completions/dataflow.fish
```

## Upgrading

```bash
pip install --upgrade dataflow-cli
```

> [!note]
> After upgrading, run `dataflow migrate up` to apply any database schema changes. See the [[changelog|Changelog]] for breaking changes.

## Next steps

Once installed, head to the [[getting-started/quickstart|Quickstart]] to build your first pipeline.
