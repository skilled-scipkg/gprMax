# gprmax source map: Tools

Generated from source roots:
- `gprMax`
- `tools`
- `user_libs`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `plot`
- `merge`
- `fft`
- `rxcomponent`
- `hdf5`
- `notebook`

## Fast source navigation
- `rg -n "CmdInputError|nrx|fft|merge_files|get_output_data" tools`
- `rg -n "write_hdf5_outputfile|fields_outputs" gprMax`

## Suggested source entry points
- `tools/plot_Ascan.py` | A-scan plotting and FFT constraints (`mpl_plot`)
- `tools/outputfiles_merge.py` | merge behavior for multi-trace outputs (`merge_files`, `get_output_data`)
- `tools/plot_Bscan.py` | B-scan image generation (`mpl_plot`)
- `tools/plot_antenna_params.py` | antenna impedance/s-parameter plotting (`calculate_antenna_params`)
- `tools/convert_png2h5.py` | image-to-geometry conversion
- `gprMax/fields_outputs.py` | output file writing conventions (`store_outputs`, `write_hdf5_outputfile`)
