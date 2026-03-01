# gprmax source map: Troubleshooting

Generated from source roots:
- `gprMax`
- `tools`
- `user_libs`
- `tests`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `error`
- `GeneralError`
- `CmdInputError`
- `check_cmd_names`
- `geometry-only`
- `mpi`
- `dispersion`

## Fast source navigation
- `rg -n "GeneralError|CmdInputError|check_cmd_names|dispersion|geometry-only" gprMax tools tests`
- `rg -n "add_argument|mpi|restart|task" gprMax/gprMax.py`

## Suggested source entry points
- `gprMax/gprMax.py` | mode guards (MPI/task/restart combinations) and CLI validation (`run_main`)
- `gprMax/model_build_run.py` | input processing, geometry checks, dispersion diagnostics (`run_model`)
- `gprMax/input_cmds_file.py` | command name validation and processed input writing (`check_cmd_names`)
- `gprMax/input_cmds_singleuse.py` | essential command parsing (`process_singlecmds`)
- `gprMax/input_cmds_geometry.py` | geometry processing and overlap behavior (`process_geometrycmds`)
- `tools/inputfile_old2new.py` | legacy syntax migration
- `tools/plot_Ascan.py` | post-processing failure points (`mpl_plot`, receiver/component guards)
- `tools/outputfiles_merge.py` | merge/read failure points (`merge_files`, `get_output_data`)
