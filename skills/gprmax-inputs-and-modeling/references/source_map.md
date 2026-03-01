# gprmax source map: Inputs and Modeling

Generated from source roots:
- `gprMax`
- `tools`
- `user_libs`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `input`
- `geometry`
- `material`
- `waveform`
- `rx`
- `src_steps`
- `dispersion`

## Fast source navigation
- `rg -n "check_cmd_names|process_singlecmds|process_multicmds|process_geometrycmds" gprMax`
- `rg -n "dispersion|srcsteps|rxsteps|GeneralError" gprMax/model_build_run.py`

## Suggested source entry points
- `gprMax/input_cmds_file.py` | command validation and pre-processing (`check_cmd_names`, `process_include_files`)
- `gprMax/input_cmds_singleuse.py` | essential command handling (`process_singlecmds`)
- `gprMax/input_cmds_multiuse.py` | materials/sources/receivers command handling (`process_multicmds`)
- `gprMax/input_cmds_geometry.py` | ordered object construction (`process_geometrycmds`)
- `gprMax/input_cmd_funcs.py` | scriptable command helper functions (`domain`, `dx_dy_dz`, `rx_steps`, `src_steps`)
- `gprMax/model_build_run.py` | geometry stepping bounds and dispersion checks (`run_model`)
- `gprMax/materials.py` | material processing and update coefficients (`process_materials`)
