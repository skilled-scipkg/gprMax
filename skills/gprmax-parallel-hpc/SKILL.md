---
name: gprmax-parallel-hpc
description: Use this skill for OpenMP/MPI/GPU execution strategy, HPC launcher patterns, and performance benchmarking in gprMax.
---

# gprmax: Parallel and HPC

## High-Signal Playbook

### Route conditions
- Use `gprmax-build-and-install` for compiler/CUDA/`mpi4py` installation issues.
- Use `gprmax-inputs-and-modeling` when the model itself is unstable or physically misconfigured.
- Use `gprmax-troubleshooting` when runtime failures are not clearly tied to parallel mode configuration.

### Triage questions
- What is the target run shape: one model, B-scan task farm (`-n`), or benchmark?
- How many CPU cores and GPU devices are available on each node?
- Does your cluster support MPI spawn, or do you need `--mpi-no-spawn`?
- Are you using scheduler job arrays (`-task`) or MPI task farm (`-mpi`), but not both?
- What is the exact command and error output?

### Canonical workflow
1. Confirm a serial CPU run first on a known input (`user_models/cylinder_Ascan_2D.in`).
2. Set OpenMP thread count (`OMP_NUM_THREADS`) or model-level `#num_threads` (`docs/source/openmp_mpi.rst`).
3. For B-scans on one host/cluster with MPI spawn, run `-n <traces> -mpi <traces+1>`.
4. If MPI spawn is unsupported, launch with `mpirun` and `--mpi-no-spawn`.
5. For GPU acceleration, use `-gpu` (single GPU) or `-gpu <ids...>` with MPI workers.
6. For performance baselines, run `-benchmark` models and plot `.npz` outputs (`docs/source/benchmarking.rst`).

### Minimal working example
```bash
export OMP_NUM_THREADS=8
python -m gprMax user_models/cylinder_Ascan_2D.in
python -m gprMax user_models/cylinder_Bscan_2D.in -n 60 -mpi 61
mpirun -n 61 python -m gprMax user_models/cylinder_Bscan_2D.in -n 60 --mpi-no-spawn
python -m gprMax user_models/cylinder_Ascan_2D.in -gpu
python -m gprMax tests/benchmarking/bench_100x100x100.in -benchmark
python -m tests.benchmarking.plot_benchmark tests/benchmarking/bench_100x100x100.npz
```

### Pitfalls and fixes
- `-mpi` or `--mpi-no-spawn` with `-n 1` is rejected (`gprMax/gprMax.py`).
- `-task` cannot be combined with MPI task-farm modes (`gprMax/gprMax.py`).
- Benchmark mode cannot be combined with MPI, job array mode, or `-n > 1` (`gprMax/gprMax.py`).
- Incorrect MPI task count: workers should match model count, plus one master task.
- GPU IDs must exist on host; check detected devices in startup output (`gprMax/utilities.py`).
- Oversubscribed threads reduce throughput; align `OMP_NUM_THREADS` with allocated cores.

### Convergence and validation checks
- Startup log reports host information and expected GPU detection (`gprMax/gprMax.py`).
- MPI modes print clear master/worker startup messages and complete all model indices.
- B-scan run creates expected numbered output files (or merged outputs after post-processing).
- Benchmark run writes `.npz` archive with CPU/GPU timing arrays (`tests/benchmarking`).
- Speed-up trend with more threads/GPUs is monotonic or near-monotonic for fixed model size.

## Scope
- Handle questions about MPI/OpenMP/GPU execution, scaling, and batch systems.
- Keep guidance command-first, with clear mode constraints and scheduler-ready examples.

## Primary documentation references
- `docs/source/openmp_mpi.rst`
- `docs/source/gpu.rst`
- `docs/source/benchmarking.rst`
- `README.rst`

## Workflow
- Start with `docs/source/openmp_mpi.rst` and `docs/source/gpu.rst` for run modes and dependencies.
- Use `docs/source/benchmarking.rst` for reproducible performance measurement.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- If ambiguity remains after docs, inspect `references/source_map.md` and use the listed function-level entry points.

## Tutorials and examples
- `user_models/cylinder_Bscan_2D.in`
- `tests/benchmarking/bench_100x100x100.in`
- `tools/HPC_scripts`

## Test references
- `tests/benchmarking`
- `tests/models_basic/cylinder_Ascan_2D`

## Optional deeper inspection
- `gprMax`
- `tools`

## Source entry points for unresolved issues
- `gprMax/gprMax.py` (`api`, `run_main`, `run_mpi_sim`, `run_mpi_no_spawn_sim`, `run_benchmark_sim`)
- `gprMax/model_build_run.py` (`run_model`, `solve_cpu`, `solve_gpu`)
- `gprMax/utilities.py` (`detect_check_gpus`, `get_host_info`)
- `tools/HPC_scripts/gprmax_omp.sh` (single-node OpenMP launcher template)
- `tools/HPC_scripts/gprmax_omp_mpi.sh` (MPI spawn launcher template)
- `tools/HPC_scripts/gprmax_omp_mpi_no_spawn.sh` (MPI no-spawn launcher template)
- `tools/HPC_scripts/gprmax_omp_jobarray.sh` (scheduler job-array launcher template)
- `tests/benchmarking/plot_benchmark.py` (timing plot and speed-up aggregation)
- `gprMax/fields_updates_gpu.py` (GPU field-update kernel path for deep performance debugging)
- `gprMax/source_updates_gpu.py` (GPU source-update kernel path for deep performance debugging)
- Prefer targeted source search: `rg -n "run_mpi|mpi_no_spawn|benchmark|detect_check_gpus|OMP_NUM_THREADS" gprMax tools tests`.
