---
name: gprmax-tools
description: Use this skill for gprMax output handling, A-scan/B-scan plotting, merge utilities, and notebook/CLI post-processing workflows.
---

# gprmax: Tools

## High-Signal Playbook

### Route conditions
- Use `gprmax-inputs-and-modeling` when the issue is bad model setup rather than post-processing.
- Use `gprmax-troubleshooting` when runs fail before producing output files.
- Use `gprmax-analysis-and-output` when the question is about output format semantics rather than plotting commands.

### Triage questions
- Do you have a single A-scan output (`.out`) or many trace files from `-n` runs?
- Which receiver component do you need (`Ex`, `Ey`, `Ez`, `Hx`, `Hy`, `Hz`, `Ix`, `Iy`, `Iz`)?
- Do you need time-series only, FFT, B-scan image, or antenna parameters (`s11`/`s21`)?
- Is your analysis CLI-first, notebook-based, MATLAB-based, or Paraview-based?
- Are receivers present in the output (`nrx > 0`)?

### Canonical workflow
1. Confirm output file(s) exist and identify whether they are single-trace or multi-trace sets (`docs/source/output.rst`).
2. For A-scans, run `python -m tools.plot_Ascan <outputfile>` (`docs/source/plotting.rst`).
3. For B-scans, merge traces first with `python -m tools.outputfiles_merge <basefilename>` (`docs/source/utils.rst`).
4. Plot merged B-scan with `python -m tools.plot_Bscan <merged.out> <component>` (`docs/source/plotting.rst`).
5. For antennas, use `python -m tools.plot_antenna_params <outputfile> [options]` (`docs/source/plotting.rst`).
6. For geometry/snapshot viewing, use Paraview and gprMax macros (`docs/source/output.rst`).

### Minimal working example
```bash
python -m tools.plot_Ascan user_models/cylinder_Ascan_2D.out --outputs Ez -fft
python -m tools.outputfiles_merge user_models/cylinder_Bscan_2D
python -m tools.plot_Bscan user_models/cylinder_Bscan_2D_merged.out Ez
python -m tools.plot_antenna_params user_models/antenna_like_MALA_1200_fs.out
python -m tools.convert_png2h5 my_layers.png 0.002 0.002 0.002 -zcells 150
```

### Pitfalls and fixes
- Plotting B-scan before merge: merge first with `tools/outputfiles_merge.py` (`docs/source/utils.rst`, `docs/source/plotting.rst`).
- Requesting missing receiver component: inspect available outputs in the HDF5 receiver group (`docs/source/output.rst`).
- Using `-fft` with multiple outputs in `plot_Ascan`: only one output is allowed (`tools/plot_Ascan.py`).
- Output has `nrx == 0`: plotting scripts will fail by design (`tools/plot_Ascan.py`, `tools/plot_Bscan.py`).
- `--remove-files` deletes individual trace files after merge; use only when cleanup is intended (`docs/source/utils.rst`).

### Convergence and validation checks
- Confirm output root attributes (`Iterations`, `dt`, `nrx`) before plotting (`docs/source/output.rst`).
- For B-scan, verify merged dataset shape is `[Iterations, number_of_traces]` (`tools/outputfiles_merge.py`).
- Use consistent receiver component across merge/plot/analysis commands.
- For FFT inspection, ensure time window and `dt` are physically meaningful for the excitation bandwidth.

## Scope
- Handle post-processing commands and data inspection from gprMax outputs.
- Provide reproducible CLI workflows first; mention notebook/MATLAB/Paraview paths when relevant.

## Primary documentation references
- `docs/source/plotting.rst`
- `docs/source/utils.rst`
- `docs/source/output.rst`
- `tools/Jupyter_notebooks/README.rst`

## Workflow
- Begin with output structure (`docs/source/output.rst`) to verify data availability.
- Apply plotting and merge utilities from `docs/source/plotting.rst` and `docs/source/utils.rst`.
- Escalate to source scripts when output component or shape behavior is unclear.

## Tutorials and examples
- `tools/Jupyter_notebooks`
- `user_models/cylinder_Ascan_2D.in`
- `user_models/cylinder_Bscan_2D.in`

## Test references
- `tests/models_basic/cylinder_Ascan_2D`
- `tests/experimental`

## Optional deeper inspection
- `tools`
- `gprMax`

## Source entry points for unresolved issues
- `tools/plot_Ascan.py` (output-component selection, FFT rules, receiver checks)
- `tools/outputfiles_merge.py` (trace-file merge logic and metadata writing)
- `tools/plot_Bscan.py` (B-scan rendering path)
- `tools/plot_antenna_params.py` (antenna impedance/s-parameter plotting)
- `tools/convert_png2h5.py` (image-to-geometry conversion path)
- `gprMax/fields_outputs.py` (HDF5 output writing path)
- Prefer targeted source search: `rg -n "nrx|output|merge|plot|fft|rxcomponent" tools gprMax`.
