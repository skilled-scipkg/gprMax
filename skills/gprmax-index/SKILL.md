---
name: gprmax-index
description: Use this index to route gprMax requests to the right topic skill first, keeping responses docs-first and escalating to source inspection only when needed.
---

# gprmax Skills Index

## Route the request
- **Install/build/update issues** -> `gprmax-build-and-install`
- **First run, basic CLI usage, smoke tests** -> `gprmax-getting-started`
- **Input commands, materials, geometry, resolution/CFL/time-window** -> `gprmax-inputs-and-modeling`
- **MPI/OpenMP/GPU and cluster script patterns** -> `gprmax-parallel-hpc`
- **Output file structure and interpretation** -> `gprmax-analysis-and-output`
- **Plotting/merge/utilities/notebooks** -> `gprmax-tools`
- **Regression/experimental/benchmark validation** -> `gprmax-tests`
- **Error diagnosis and known failure patterns** -> `gprmax-troubleshooting`
- **Low-frequency reference/index/analytical-doc topics** -> `gprmax-advanced-topics`

## Generated topic skills
- `gprmax-build-and-install`: toolchains, compilation, rebuild workflow
- `gprmax-getting-started`: first simulation path and baseline CLI flags
- `gprmax-inputs-and-modeling`: core input-file and physical-model workflow
- `gprmax-parallel-hpc`: OpenMP/MPI/GPU execution and HPC launch patterns
- `gprmax-analysis-and-output`: output formats and interpretation paths
- `gprmax-tools`: plotting and utility scripts (CLI + notebook-adjacent)
- `gprmax-tests`: regression, analytical/experimental comparison, benchmarking
- `gprmax-troubleshooting`: diagnostics and failure triage
- `gprmax-advanced-topics`: consolidated tail topics (references/index/analytical docs)

## Simulation-first route
1. Start in `gprmax-getting-started` to run a known-good A-scan/B-scan baseline.
2. Move to `gprmax-inputs-and-modeling` to adapt physics, geometry, and discretization.
3. Move to `gprmax-parallel-hpc` for scaling (OpenMP/MPI/GPU) once baseline correctness is established.
4. Use `gprmax-tools` or `gprmax-analysis-and-output` for output checks and interpretation.
5. Use `gprmax-tests` to compare against references/benchmarks before large campaign runs.

## Documentation-first escalation rules
1. Start from the routed skill's **Primary documentation references**.
2. If still ambiguous, inspect that skill's doc map in its `references/` folder.
3. Only then inspect that skill's source map in its `references/` folder and source entry points.
4. Use targeted symbol search while inspecting source (`rg -n "<symbol_or_keyword>" gprMax tools user_libs tests`).

## Documentation-first inputs
- `docs/source`

## Tutorials and examples roots
- `user_models`
- `tools/Jupyter_notebooks`

## Test roots for behavior checks
- `tests`

## Source directories for deeper inspection
- `gprMax`
- `tools`
- `user_libs`
