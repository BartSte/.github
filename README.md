# Reusable Python Workflows

This repository hosts two reusable GitHub Actions workflows: `.github/workflows/pytest.yml` for running tests and `.github/workflows/pypi.yml` for publishing distributions.

## Prerequisites

- Your project uses pytest as its test runner.
- Dependency management is handled by `uv`, with both `pyproject.toml` and `uv.lock` committed.
- Publishing with `.github/workflows/pypi.yml` requires a configured GitHub environment (defaults to `release`) with PyPI trusted publishing enabled.

## How to Use from Another Repository

1. Ensure your repository contains `pyproject.toml` and an up-to-date `uv.lock`.
2. Create or update a workflow (e.g., `.github/workflows/tests.yml`) in the consuming repository. The following snippets demonstrate how to call each reusable workflow:

#### Run tests with the pytest workflow

```yaml
name: Tests
on:
  push:
    branches: [main]
  pull_request:
jobs:
  pytest:
    uses: BartSte/.github/.github/workflows/pytest.yml@main
    with:
      pytest-args: "" # optional extra pytest flags
```

#### Publish distributions with the PyPI workflow

This reusable workflow requires a GitHub environment (defaulting to `release`) and can optionally target a custom package index by setting `repository-url`.

```yaml
name: Publish
on:
  release:
    types: [published]

jobs:
  pypi:
    uses: BartSte/.github/.github/workflows/pypi.yml@main
    with:
      python-version: "3.11" # optional
      environment-name: release # matches your configured environment
      repository-url: "" # optional; omit or leave blank for PyPI
```

Inputs are optional—omit them if defaults are acceptable. Callers can drop optional inputs or point to TestPyPI by setting `repository-url`.

## Workflow Steps at a Glance

### pytest.yml
- Checkout the calling repository.
- Set up `uv`, optionally with a caller-specified Python version.
- Synchronize dependencies with `uv sync --frozen`.
- Run `pytest` with debug logging (and any extra arguments provided).

### pypi.yml
- Checkout the calling repository.
- Set up `uv`, optionally with a caller-specified Python version.
- Synchronize dependencies with `uv sync --frozen`.
- Build both wheel and sdist artifacts using `uv`.
- Publish distributions to PyPI by default, or to an alternate repository when `repository-url` is provided.
