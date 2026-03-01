# gprmax source map: Analysis and Output

Generated from source roots:
- `gprMax`
- `tools`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `output`
- `hdf5`
- `nrx`
- `merge`
- `fft`
- `plot`
- `s11`
- `s21`

## Fast source navigation
- `rg -n "write_hdf5_outputfile|store_outputs|nrx|rxsteps" gprMax/fields_outputs.py gprMax/model_build_run.py`
- `rg -n "^def mpl_plot|^def calculate_antenna_params|CmdInputError|fft|get_output_data|merge_files" tools/plot_Ascan.py tools/plot_Bscan.py tools/plot_antenna_params.py tools/outputfiles_merge.py`
- `rg -n "outputfiles_merge|plot_Ascan|plot_Bscan|plot_antenna_params" docs/source/plotting.rst docs/source/utils.rst`

## Suggested source entry points
- `gprMax/fields_outputs.py` | output writing path (`store_outputs`, `write_hdf5_outputfile`)
- `gprMax/model_build_run.py` | solver-to-output control flow (`run_model` -> output write)
- `tools/outputfiles_merge.py` | merge behavior (`merge_files`, `get_output_data`)
- `tools/plot_Ascan.py` | receiver/component checks and FFT constraints (`mpl_plot`)
- `tools/plot_Bscan.py` | B-scan rendering path and component selection (`mpl_plot`)
- `tools/plot_antenna_params.py` | antenna impedance/s-parameter extraction (`calculate_antenna_params`)
- `tools/plot_source_wave.py` | waveform/time-window sanity checks (`check_timewindow`, `mpl_plot`)
- `tools/MATLAB_scripts/outputfile_converter.m` | MATLAB conversion path for output parity checks
