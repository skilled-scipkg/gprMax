---
name: gprmax-inputs-and-modeling
description: Use this skill for gprMax input-file design, material/geometry setup, resolution and time-window validation, and reproducible A-scan/B-scan workflows.
---

# gprmax: Inputs and Modeling

## High-Signal Playbook

### Route conditions
- Use `gprmax-getting-started` for environment setup and first command invocation.
- Use `gprmax-build-and-install` for compiler/build failures.
- Use `gprmax-tools` or `gprmax-analysis-and-output` for plotting and post-processing questions.
- Use `gprmax-parallel-hpc` for MPI/OpenMP/GPU execution strategy.

### Triage questions
- Is the model 2D (single-cell invariant axis) or full 3D? (`docs/source/gprmodelling.rst`)
- What are the highest significant frequency and smallest geometric feature? (`docs/source/examples_simple_2D.rst`, `docs/source/gprmodelling.rst`)
- What essential commands are already present (`#domain`, `#dx_dy_dz`, `#time_window`)? (`docs/source/input.rst`)
- Are source/receiver and targets sufficiently far from PML boundaries? (`docs/source/gprmodelling.rst`)
- Are you building A-scan only, or B-scan with `#src_steps/#rx_steps` and `-n`?
- Do you need custom scripting blocks (`#python ... #end_python`)? (`docs/source/python_scripting.rst`)

### Canonical workflow
1. Define essential commands: `#domain`, `#dx_dy_dz`, `#time_window` (`docs/source/input.rst`).
2. Define materials (`#material` and optional dispersive commands) (`docs/source/input.rst`).
3. Define excitation (`#waveform`) and source (`#hertzian_dipole`/other) (`docs/source/input.rst`).
4. Define receiver(s) (`#rx` or `#rx_array`) (`docs/source/input.rst`).
5. Build geometry objects in intended overwrite order (`#box`, `#cylinder`, etc.) (`docs/source/input.rst`).
6. For scan series, add `#src_steps`/`#rx_steps` and run with `-n` (`docs/source/examples_simple_2D.rst`, `docs/source/input.rst`).
7. Run `--geometry-only` first for geometry sanity check (`README.rst`, `docs/source/input.rst`).
8. Run full simulation and validate traces/B-scan shape (`docs/source/examples_simple_2D.rst`).

### Minimal working example
```none
#title: A-scan from a metal cylinder buried in a dielectric half-space
#domain: 0.240 0.210 0.002
#dx_dy_dz: 0.002 0.002 0.002
#time_window: 3e-9
#material: 6 0 1 0 half_space
#waveform: ricker 1 1.5e9 my_ricker
#hertzian_dipole: z 0.100 0.170 0 my_ricker
#rx: 0.140 0.170 0
#box: 0 0 0 0.240 0.170 0.002 half_space
#cylinder: 0.120 0.080 0 0.120 0.080 0.002 0.010 pec
```

```bash
python -m gprMax user_models/cylinder_Ascan_2D.in
python -m tools.plot_Ascan user_models/cylinder_Ascan_2D.out
python -m gprMax user_models/cylinder_Bscan_2D.in -n 60
python -m tools.outputfiles_merge user_models/cylinder_Bscan_2D
python -m tools.plot_Bscan user_models/cylinder_Bscan_2D_merged.out Ez
```

### Pitfalls and fixes
- Command lines not starting with `#` are ignored; malformed command names abort execution (`docs/source/input.rst`).
- One command per line; multi-parameter commands must be space-separated (`docs/source/input.rst`).
- Object creation order matters; later objects overwrite earlier geometry (`docs/source/input.rst`, `docs/source/faqs.rst`).
- 2D models still require one cell in invariant direction (`docs/source/examples_simple_2D.rst`, `docs/source/gprmodelling.rst`).
- Overly coarse `#dx_dy_dz` causes dispersion and poor geometry resolution (`docs/source/gprmodelling.rst`).
- Too-short `#time_window` misses target reflections (`docs/source/examples_simple_2D.rst`).
- Sources/targets placed in or too close to PML yield non-physical results (`docs/source/gprmodelling.rst`).
- `#src_steps/#rx_steps` are for simple sources/receivers, not complex antenna geometry (`docs/source/input.rst`).

### Convergence and validation checks
- Use wavelength sampling rule of thumb: at least ~10 cells per smallest wavelength (`docs/source/gprmodelling.rst`).
- Ensure PML spacing: keep sources/targets at least ~15 cells from boundaries (`docs/source/gprmodelling.rst`).
- Confirm CFL-consistent time stepping through gprMax defaults and optional stability factor only when justified (`docs/source/input.rst`).
- Run `--geometry-only` plus `#geometry_view` before expensive runs (`docs/source/input.rst`).
- Check for dispersion warnings and coordinate stepping errors in runtime output (`gprMax/model_build_run.py`).

## Scope
- Handle input command syntax, material/geometry modeling, and run configuration for reproducible simulations.
- Emphasize correctness-critical knobs (resolution, time window, PML spacing, source/receiver placement).

## Primary documentation references
- `docs/source/input.rst`
- `docs/source/gprmodelling.rst`
- `docs/source/examples_simple_2D.rst`
- `docs/source/examples_antennas.rst`
- `docs/source/python_scripting.rst`
- `docs/source/output.rst`

## Workflow
- Begin with essential commands and geometry/material definition from `docs/source/input.rst`.
- Use `docs/source/examples_simple_2D.rst` as the canonical minimal reproduction path.
- Use `docs/source/gprmodelling.rst` for resolution/CFL/PML reasoning before parameter sweeps.
- Escalate to source inspection only when docs and examples do not explain observed behavior.

## Tutorials and examples
- `user_models/cylinder_Ascan_2D.in`
- `user_models/cylinder_Bscan_2D.in`
- `user_models`

## Test references
- `tests/models_basic/cylinder_Ascan_2D`
- `tests/models_basic/hertzian_dipole_fs`

## Optional deeper inspection
- `gprMax`
- `tools`
- `user_libs`

## Source entry points for unresolved issues
- `gprMax/input_cmds_file.py` (`check_cmd_names`, processed-input generation)
- `gprMax/input_cmds_singleuse.py` (essential command processing)
- `gprMax/input_cmds_multiuse.py` (materials, sources, receivers)
- `gprMax/input_cmds_geometry.py` (ordered geometry construction)
- `gprMax/input_cmd_funcs.py` (Python scripting helpers for command generation)
- `gprMax/model_build_run.py` (geometry checks, source/receiver stepping bounds, dispersion warnings)
- `gprMax/materials.py` (material definitions and dispersive behavior handling)
- Prefer targeted source search: `rg -n "#domain|#dx_dy_dz|#time_window|rx_steps|src_steps|dispersion" gprMax`.
