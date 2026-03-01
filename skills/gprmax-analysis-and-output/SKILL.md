---
name: gprmax-analysis-and-output
description: Use this skill for interpreting gprMax output structure, validating generated datasets, and running analysis/post-processing workflows.
---

# gprmax: Analysis and Output

## High-Signal Playbook

### Route conditions
- Use `gprmax-tools` for direct plotting/merge utility command usage.
- Use `gprmax-inputs-and-modeling` when output anomalies are caused by model setup choices.
- Use `gprmax-troubleshooting` when simulations fail before output files are produced.

### Triage questions
- Do you already have `.out` files, or do you need a minimal run first?
- Is the target A-scan, B-scan, antenna parameter analysis, or raw HDF5 inspection?
- Which receiver number/component is required (`rx1`, `Ez`, etc.)?
- Are you validating waveform physics, data integrity, or only plotting mechanics?
- Do you need CLI-only processing, notebooks, MATLAB, or Paraview workflow?

### Canonical workflow
1. Generate or locate output file(s), using a known input first if needed (`user_models/cylinder_Ascan_2D.in`).
2. Validate HDF5 root attributes and receiver groups (`docs/source/output.rst`).
3. For A-scan analysis, inspect/plot receiver outputs (`docs/source/plotting.rst`).
4. For B-scan analysis, merge per-trace files before plotting (`docs/source/utils.rst`, `docs/source/plotting.rst`).
5. For antenna studies, extract impedance/s-parameters from transmission-line or receiver data.
6. Escalate to source when expected datasets/components are absent or shaped unexpectedly.

### Minimal working example
```bash
python -m gprMax user_models/cylinder_Ascan_2D.in
python -m tools.plot_Ascan user_models/cylinder_Ascan_2D.out --outputs Ez -fft
python -m gprMax user_models/cylinder_Bscan_2D.in -n 60
python -m tools.outputfiles_merge user_models/cylinder_Bscan_2D
python -m tools.plot_Bscan user_models/cylinder_Bscan_2D_merged.out Ez
python -m tools.plot_antenna_params user_models/antenna_like_MALA_1200_fs.out
```

### Pitfalls and fixes
- `.out` file exists but has `nrx == 0`: plotting scripts fail by design.
- B-scan plotted before merge: run `python -m tools.outputfiles_merge <base>` first.
- Requested component missing in `/rxs/rx*/`: inspect available datasets before plotting.
- `-fft` in `plot_Ascan` requires a single output component.
- Mixed receiver/component assumptions across traces produce misleading comparisons.

### Convergence and validation checks
- Root attributes include expected `Iterations`, `dt`, `nrx`, and `dx_dy_dz` (`docs/source/output.rst`).
- Receiver groups (`/rxs/rx1/...`) contain requested components.
- Merged B-scan dimensions are physically plausible: `[Iterations, number_of_traces]`.
- A-scan/B-scan qualitative signatures match known examples before advanced interpretation.

## Scope
- Handle questions about output formats, analysis, and post-processing.
- Keep guidance validation-first: confirm data integrity before interpretation.

## Primary documentation references
- `docs/source/plotting.rst`
- `docs/source/output.rst`
- `docs/source/utils.rst`
- `tools/Jupyter_notebooks/README.rst`

## Workflow
- Start with `docs/source/output.rst` to verify what data exists and how it is structured.
- Use plotting and utility docs to choose analysis workflow (A-scan/B-scan/antenna/snapshot).
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with function-level entry points.

## Tutorials and examples
- `user_models/cylinder_Ascan_2D.in`
- `user_models/cylinder_Bscan_2D.in`
- `tools/Jupyter_notebooks`

## Test references
- `tests/models_basic/cylinder_Ascan_2D`
- `tests/experimental`

## Optional deeper inspection
- `gprMax`
- `tools`

## Source entry points for unresolved issues
- `tools/outputfiles_merge.py` (`merge_files`, `get_output_data`)
- `tools/plot_Ascan.py` (`mpl_plot`, receiver/component validation, FFT guardrails)
- `tools/plot_Bscan.py` (`mpl_plot`, receiver loop and component checks)
- `tools/plot_antenna_params.py` (`calculate_antenna_params`)
- `tools/plot_source_wave.py` (`check_timewindow`, waveform FFT diagnostics)
- `gprMax/fields_outputs.py` (`store_outputs`, `write_hdf5_outputfile`)
- `gprMax/model_build_run.py` (`run_model`, output write invocation path)
- `tools/MATLAB_scripts/plot_Bscan.m` (MATLAB B-scan rendering parity path)
- `tools/MATLAB_scripts/outputfile_converter.m` (MATLAB output conversion parity path)
- Prefer targeted source search: `rg -n "write_hdf5_outputfile|nrx|get_output_data|merge_files|mpl_plot|calculate_antenna_params" gprMax tools`.
