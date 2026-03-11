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

By default, the workflow reads `requires-python` from your `pyproject.toml` and runs tests against every compatible Python version (from the set 3.9–3.13). To override this, pass a JSON array:

```yaml
    with:
      python-versions: '["3.12", "3.13"]'
```

## Workflow Steps at a Glance

### pytest.yml

- **setup job**: Resolves the Python versions to test. Auto-detects compatible versions from `requires-python` in `pyproject.toml`, or uses the caller-supplied `python-versions` JSON array.
- **test job (matrix)**: For each resolved Python version — checkout, set up `uv` with that version, synchronize dependencies with `uv sync --frozen`, and run `pytest` with debug logging (and any extra arguments provided).
