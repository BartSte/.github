# Reusable Python Workflows

This repository hosts reusable GitHub Actions workflows.

## Prerequisites

- Your project uses pytest as its test runner.
- Dependency management is handled by `uv`, with both `pyproject.toml` and `uv.lock` committed.

## How to Use from Another Repository

1. Ensure your repository contains `pyproject.toml` and an up-to-date `uv.lock`.
2. Create or update a workflow (e.g., `.github/workflows/pytests.yml`) in the consuming repository. The following snippets demonstrate how to call each reusable workflow:

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

## Workflow Steps at a Glance

### pytest.yml
- Checkout the calling repository.
- Set up `uv`, optionally with a caller-specified Python version.
- Synchronize dependencies with `uv sync --frozen`.
- Run `pytest` with debug logging (and any extra arguments provided).

