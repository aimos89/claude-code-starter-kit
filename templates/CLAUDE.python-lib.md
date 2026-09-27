# <PROJECT NAME>

<One-sentence description of what this library does and who uses it.>

## Commands
- Install (dev): `pip install -e ".[dev]"`
- Test: `pytest` — single test: `pytest tests/test_x.py::test_y -q`
- Lint/format: `ruff check . && ruff format .`
- Type-check: `mypy src/`
- Build: `python -m build`

## Layout
- `src/<pkg>/` — library code (import as `<pkg>`)
- `tests/` — mirrors `src/` structure; every public function has a test
- `pyproject.toml` — single source of truth for deps + tool config

## Conventions
- Public API is only what's exported in `__init__.py`. Everything else is private (`_name`).
- Type hints on all public functions. Docstrings in <numpy|google> style.
- No new runtime dependencies without discussion — this is a library, keep it lean.
- Line length 100. Ruff rules: E, F, W, I, UP. Format with `ruff format`.

## Testing rules
- New behavior needs a test in the same PR. Bug fix = a test that fails before, passes after.
- Don't mock what you can construct cheaply. Prefer real objects.

## Gotchas
- <e.g. "The C extension in `src/<pkg>/_ext` is prebuilt; don't edit the generated `.c`.">
- <e.g. "Public API is frozen for 1.x — additions only, no signature changes.">

## Never
- Never edit files under `<generated/>`. Never bump the version manually (release script does it).
