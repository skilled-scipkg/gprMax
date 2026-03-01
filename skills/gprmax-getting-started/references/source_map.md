# gprmax source map: Getting Started

Generated from source roots:
- `gprMax`
- `tools`
- `user_libs`
- `tests`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `main`
- `cli`
- `run`
- `example`
- `plot`
- `merge`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" gprMax tools user_libs tests`
- `rg -n "add_argument|run_model|plot|merge" gprMax tools`

## Suggested source entry points
- `gprMax/__main__.py` | module entrypoint for `python -m gprMax`
- `gprMax/gprMax.py` | command-line arguments and run-mode routing (`main`, `api`, `run_main`)
- `gprMax/model_build_run.py` | processed-input, geometry build, solver loop (`run_model`)
- `tools/plot_Ascan.py` | A-scan plotting options and output checks (`mpl_plot`)
- `tools/outputfiles_merge.py` | B-scan output merge logic (`merge_files`, `get_output_data`)
- `tools/plot_Bscan.py` | B-scan rendering behavior (`mpl_plot`)
