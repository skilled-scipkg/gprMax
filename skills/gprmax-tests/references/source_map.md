# gprmax source map: Tests

Generated from source roots:
- `gprMax`
- `tools`
- `user_libs`
- `tests`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `test_models`
- `experimental`
- `analytical`
- `benchmark`
- `diff`
- `reference`

## Fast source navigation
- `rg -n "Max diff|analytical|_ref\.out|NaN|benchmark" tests`
- `rg -n "benchmark|api\(|run_main" gprMax/gprMax.py`

## Suggested source entry points
- `tests/test_models.py` | regression and analytical comparison driver script (model loop, max-diff calculation)
- `tests/test_experimental.py` | model-vs-experimental comparison script (normalization and time alignment)
- `tests/analytical_solutions.py` | analytical waveform/reference calculations (`hertzian_dipole_fs`)
- `tests/benchmarking/plot_benchmark.py` | benchmark result plotting (`autolabel` plus plotting pipeline)
- `gprMax/gprMax.py` | benchmark mode CLI path and mode constraints (`run_benchmark_sim`)
