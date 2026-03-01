# gprmax source map: Advanced Topics

Generated from source roots:
- `gprMax`
- `tools`
- `user_libs`
- `tests`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `analytical`
- `comparison`
- `reference`
- `citations`
- `index`
- `version`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" gprMax tools user_libs tests`
- `rg -n "analytical|reference|citation|version" gprMax tests docs/source`

## Suggested source entry points
- `tests/analytical_solutions.py` | analytical reference implementations (`hertzian_dipole_fs`)
- `tests/test_models.py` | analytical-vs-simulated comparison flow for fixture families
- `tests/models_basic/hertzian_dipole_fs_analytical/hertzian_dipole_fs_analytical.in` | canonical analytical fixture input
- `gprMax/_version.py` | version metadata referenced by outputs/docs
- `gprMax/__init__.py` | package metadata entry point
