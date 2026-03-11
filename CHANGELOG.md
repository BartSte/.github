# Changelog

## Unreleased

### Changed

- `pytest.yml`: replaced single `python-version` input with `python-versions` (JSON array). The workflow now runs a matrix across all Python versions compatible with `requires-python` in the caller's `pyproject.toml`. Active versions are fetched dynamically from [endoflife.date](https://endoflife.date/python) so no manual updates are needed as new releases land or old ones reach EOL.
- `pytest.yml`: callers can override auto-detection by passing a JSON array, e.g. `python-versions: '["3.12", "3.13"]'`.
- `README.md`: updated usage example and workflow description to reflect the new input and two-job structure.

### Migration

If you were passing `python-version` (singular) to `pytest.yml`, rename it to `python-versions` and wrap the value in a JSON array:

```yaml
# before
with:
  python-version: "3.12"

# after
with:
  python-versions: '["3.12"]'
```
