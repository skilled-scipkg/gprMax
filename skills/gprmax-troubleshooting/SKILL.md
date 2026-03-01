---
name: gprmax-troubleshooting
description: Use this skill for diagnosing installation, input, runtime, and post-processing failures in gprMax.
---

# gprmax: Troubleshooting

## High-Signal Playbook

### Route conditions
- Use `gprmax-build-and-install` for compiler/toolchain/build failures.
- Use `gprmax-inputs-and-modeling` for model command semantics and physical setup decisions.
- Use `gprmax-tools` for plotting/merge utility usage issues.
- Use `gprmax-parallel-hpc` for MPI/GPU scheduler and scaling issues.

### Triage questions
- Which phase fails: install/build, model parse/build, solver run, or post-processing?
- What exact command and full error text are you seeing?
- What platform and execution mode (CPU/OpenMP, GPU, MPI, job array) are you using?
- Can the same issue be reproduced with `user_models/cylinder_Ascan_2D.in`?
- Did you recently change input syntax, geometry order, or run flags?

### Canonical workflow
1. Reproduce with a known-good command on `user_models/cylinder_Ascan_2D.in`.
2. Run with `--geometry-only` to separate geometry errors from solver/runtime errors (`README.rst`, `docs/source/input.rst`).
3. Check essential commands and syntax (`#domain`, `#dx_dy_dz`, `#time_window`, valid command names) (`docs/source/input.rst`).
4. Check command-line mode constraints (`-mpi` with `-n > 1`, restart/task compatibility) (`gprMax/gprMax.py`).
5. Validate output artifacts (`.out` exists, `nrx > 0`, expected receiver components) (`docs/source/output.rst`).
6. Compare behavior with test fixtures if regression is suspected (`tests/test_models.py`).

### Minimal working example
```bash
python -m gprMax user_models/cylinder_Ascan_2D.in --geometry-only
python -m gprMax user_models/cylinder_Ascan_2D.in
python -m tools.plot_Ascan user_models/cylinder_Ascan_2D.out --outputs Ez
python -m tools.inputfile_old2new legacy_model.in
```

### Pitfalls and fixes
- Lines not starting with `#` are ignored in input files; command lines must be one-per-line (`docs/source/input.rst`).
- Invalid command names terminate parsing (`docs/source/input.rst`).
- Geometry overlaps from command order can hide intended objects (`docs/source/input.rst`, `docs/source/faqs.rst`).
- Source/target locations too close to or inside PML can produce erroneous physics (`docs/source/gprmodelling.rst`).
- `-mpi` is not useful/allowed for a single model run (`gprMax/gprMax.py`).
- Plot scripts fail when no receivers exist or component names are wrong (`tools/plot_Ascan.py`, `tools/plot_Bscan.py`).
- Legacy input syntax can break parsing; migrate with `tools.inputfile_old2new` (`docs/source/utils.rst`).

### Convergence and validation checks
- `--geometry-only` should complete and produce geometry output when requested.
- Full run should produce `.out` with expected root attributes and receiver datasets (`docs/source/output.rst`).
- Numerical dispersion warnings should be understood and mitigated when present (`gprMax/model_build_run.py`, `docs/source/gprmodelling.rst`).
- For canonical cylinder example, reflected arrival window should match docs qualitatively (`docs/source/examples_simple_2D.rst`).

## Scope
- Handle common failures across setup, parsing, simulation execution, and output handling.
- Keep diagnosis reproducible and grounded in known-good examples.

## Primary documentation references
- `docs/source/faqs.rst`
- `docs/source/input.rst`
- `docs/source/gprmodelling.rst`
- `README.rst`
- `docs/source/utils.rst`

## Workflow
- Start with FAQ-level known issues and canonical examples.
- Validate syntax and geometry first, then execution mode flags, then outputs.
- Escalate to source inspection for unresolved parser/runtime invariants.

## Tutorials and examples
- `user_models/cylinder_Ascan_2D.in`
- `user_models/cylinder_Bscan_2D.in`

## Test references
- `tests/test_models.py`
- `tests/models_basic`

## Optional deeper inspection
- `gprMax`
- `tools`
- `user_libs`

## Source entry points for unresolved issues
- `gprMax/gprMax.py` (CLI argument guards and mode branching)
- `gprMax/model_build_run.py` (input processing pipeline, geometry and stepping checks, dispersion warnings)
- `gprMax/input_cmds_file.py` (command name validation and processed-input logic)
- `gprMax/input_cmds_singleuse.py` (essential command parsing path)
- `gprMax/input_cmds_geometry.py` (geometry command parsing and overwrite behavior)
- `tools/inputfile_old2new.py` (legacy input syntax migration)
- `tools/plot_Ascan.py` (common post-processing error points)
- `tools/outputfiles_merge.py` (common merge/read error points)
- Prefer targeted source search: `rg -n "GeneralError|CmdInputError|check_cmd_names|geometry-only|mpi" gprMax tools tests`.
