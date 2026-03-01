---
name: gprmax-getting-started
description: Use this skill for first-run setup, first simulation execution, and basic CLI usage in gprMax.
---

# gprmax: Getting Started

## High-Signal Playbook

### Route conditions
- Use `gprmax-build-and-install` when the question is about compiler/toolchain failures, rebuilds, or platform-specific installation issues.
- Use `gprmax-inputs-and-modeling` when the question is about input commands, resolution/CFL choices, or model design.
- Use `gprmax-parallel-hpc` when the question is about MPI/OpenMP/GPU scaling or scheduler scripts.
- Use `gprmax-tools` or `gprmax-analysis-and-output` when the question is about plotting, merging outputs, or post-processing.
- Use `gprmax-tests` when the question is about validation against references or benchmark baselines.

### Triage questions
- Which platform are you on (Linux, macOS, Windows)?
- Are you running from the repository root with the `gprMax` conda environment active? (`README.rst`)
- Do you need CPU-only first run, or GPU/MPI first run? (`docs/source/gpu.rst`, `docs/source/openmp_mpi.rst`)
- Are you trying a known example (`user_models/cylinder_Ascan_2D.in`) or a custom input file?
- What exact command and error output are you getting?

### Canonical workflow
1. Create and activate the environment from `conda_env.yml` (`README.rst`).
2. Ensure an OpenMP-capable compiler is installed for your OS (`README.rst`).
3. Build and install gprMax from source (`python setup.py build`, `python setup.py install`) (`README.rst`).
4. Run the known A-scan example (`user_models/cylinder_Ascan_2D.in`) (`README.rst`, `docs/source/examples_simple_2D.rst`).
5. Plot the A-scan output with `tools.plot_Ascan` (`README.rst`, `docs/source/plotting.rst`).
6. For a B-scan, run `-n` repeated models, merge outputs, then plot B-scan (`README.rst`, `docs/source/examples_simple_2D.rst`, `docs/source/utils.rst`).

### Minimal working example
```bash
conda env create -f conda_env.yml
conda activate gprMax
python setup.py build
python setup.py install
python -m gprMax user_models/cylinder_Ascan_2D.in
python -m tools.plot_Ascan user_models/cylinder_Ascan_2D.out
python -m gprMax user_models/cylinder_Bscan_2D.in -n 60
python -m tools.outputfiles_merge user_models/cylinder_Bscan_2D
python -m tools.plot_Bscan user_models/cylinder_Bscan_2D_merged.out Ez
```

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

### Pitfalls and fixes
- Environment not active: re-run `conda activate gprMax` before invoking `python -m gprMax` (`README.rst`).
- macOS default clang lacks OpenMP: install `gcc` via Homebrew and rebuild (`README.rst`).
- Windows build tools missing from PATH: install VS Build Tools + SDK and set PATH (`README.rst`).
- B-scan plotted before merge: run `python -m tools.outputfiles_merge <base>` first (`docs/source/utils.rst`, `docs/source/plotting.rst`).
- Wrong component/receiver in plots: validate available outputs in the HDF5 file (`docs/source/output.rst`, `docs/source/plotting.rst`).

### Convergence and validation checks
- `python -m gprMax -h` works and shows expected CLI flags (`README.rst`, `gprMax/gprMax.py`).
- `user_models/cylinder_Ascan_2D.out` is created and includes receiver datasets (`docs/source/output.rst`).
- A-scan shows direct wave then cylinder reflection in the expected time ranges (`docs/source/examples_simple_2D.rst`).
- B-scan merged file exists and produces the expected hyperbolic response (`docs/source/examples_simple_2D.rst`).

## Scope
- Handle initial setup, first run, and basic command-line invocation patterns.
- Keep answers practical and docs-first.

## Primary documentation references
- `README.rst`
- `docs/source/include_readme.rst`
- `docs/source/examples_simple_2D.rst`
- `docs/source/plotting.rst`
- `docs/source/utils.rst`
- `docs/source/output.rst`

## Workflow
- Start from `README.rst` installation and run commands.
- Use `docs/source/examples_simple_2D.rst` for the first known-good A-scan/B-scan path.
- Use plotting and output docs to confirm successful run artifacts.
- Escalate to `references/source_map.md` and source entry points only when docs leave ambiguity.

## Tutorials and examples
- `user_models/cylinder_Ascan_2D.in`
- `user_models/cylinder_Bscan_2D.in`
- `tools/Jupyter_notebooks`

## Test references
- `tests/models_basic/cylinder_Ascan_2D`

## Optional deeper inspection
- `gprMax`
- `tools`
- `user_libs`

## Source entry points for unresolved issues
- `gprMax/__main__.py` (module entrypoint for `python -m gprMax`)
- `gprMax/gprMax.py` (CLI argument parsing and mode routing)
- `gprMax/model_build_run.py` (input processing, geometry build, solver loop)
- `tools/plot_Ascan.py` (A-scan plotting behavior and CLI options)
- `tools/outputfiles_merge.py` (B-scan merge mechanics)
- `tools/plot_Bscan.py` (B-scan rendering and component checks)
- Prefer targeted source search: `rg -n "<symbol_or_keyword>" gprMax tools user_libs tests`.
