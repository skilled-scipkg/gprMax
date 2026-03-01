---
name: gprmax-build-and-install
description: Use this skill for gprMax build/install workflows, clean rebuilds, and compiler/OpenMP setup across platforms.
---

# gprmax: Build and Install

## High-Signal Playbook

### Route conditions
- Use `gprmax-getting-started` for first simulation runs after installation.
- Use `gprmax-parallel-hpc` for MPI/GPU job distribution and scheduler scripts.
- Use `gprmax-troubleshooting` for runtime/model errors after installation succeeds.

### Triage questions
- Which OS are you building on (Linux, macOS, Windows)? (`README.rst`)
- Is `conda activate gprMax` active before build/install?
- CPU only, or do you need MPI (`mpi4py`) and/or GPU (`pycuda`)? (`docs/source/openmp_mpi.rst`, `docs/source/gpu.rst`)
- Is this a clean build after updates, or incremental rebuild?
- What exact compiler/linker error are you seeing?

### Canonical workflow
1. Create and activate the conda environment (`README.rst`).
2. Install OpenMP-capable compiler for your platform (`README.rst`).
3. Optional: install MPI and `mpi4py` for task farm usage (`docs/source/openmp_mpi.rst`).
4. Optional: install CUDA toolkit and `pycuda` for GPU solver usage (`docs/source/gpu.rst`).
5. Clean old artifacts when rebuilding (`python setup.py cleanall`) (`README.rst`, `setup.py`).
6. Build and install (`python setup.py build`, `python setup.py install`) (`README.rst`).
7. Smoke test with `--geometry-only` on a known input (`README.rst`).

### Minimal working example
```bash
conda env create -f conda_env.yml
conda activate gprMax
python setup.py cleanall
python setup.py build
python setup.py install
python -m gprMax user_models/cylinder_Ascan_2D.in --geometry-only
python -m gprMax -h
```

### Pitfalls and fixes
- macOS clang cannot provide OpenMP for gprMax: install Homebrew `gcc` and rebuild (`README.rst`, `setup.py`).
- Windows missing MSVC/OpenMP toolchain: install Build Tools + SDK and ensure PATH (`README.rst`).
- Stale compiled artifacts after `git pull`: run `python setup.py cleanall` before rebuild (`README.rst`, `setup.py`).
- CUDA toolkit/compiler mismatch for GPU build: verify toolkit compatibility before installing `pycuda` (`docs/source/gpu.rst`).
- Missing `mpi4py` when using `-mpi`: install in active gprMax environment (`docs/source/openmp_mpi.rst`).
- Using `-mpi` with one model run (`-n 1`): invalid by design (`gprMax/gprMax.py`).

### Convergence and validation checks
- `python -m gprMax -h` prints expected flags (`-n`, `-gpu`, `-mpi`, `--geometry-only`) (`gprMax/gprMax.py`).
- `python -m gprMax user_models/cylinder_Ascan_2D.in --geometry-only` completes without build/runtime exceptions.
- CPU build uses OpenMP flags appropriate to platform (`setup.py`).
- Optional GPU smoke test runs with `-gpu` on a CUDA-capable host (`docs/source/gpu.rst`).

## Scope
- Handle build/install, toolchain setup, and rebuild workflow questions.
- Keep responses command-first and platform-specific.

## Primary documentation references
- `README.rst`
- `docs/source/openmp_mpi.rst`
- `docs/source/gpu.rst`
- `docs/source/coding.rst`
- `setup.py`

## Workflow
- Start from `README.rst` installation order: environment, compiler, build/install.
- Use MPI/GPU docs for optional dependencies.
- Escalate to `setup.py` and runtime source files only for unresolved platform/compiler behavior.

## Tutorials and examples
- `user_models/cylinder_Ascan_2D.in`

## Test references
- `tests/models_basic/cylinder_Ascan_2D`
- `tests/benchmarking`

## Optional deeper inspection
- `gprMax`
- `tools`

## Source entry points for unresolved issues
- `setup.py` (compiler flags, OpenMP linkage, `cleanall`, Cython build mode)
- `gprMax/gprMax.py` (CLI flags and invalid mode combinations)
- `gprMax/model_build_run.py` (runtime model build path after installation)
- `gprMax/utilities.py` (GPU detection and host information helpers)
- `gprMax/fields_updates_gpu.py` (GPU field update kernel path)
- `gprMax/source_updates_gpu.py` (GPU source update kernel path)
- `tools/HPC_scripts/gprmax_omp.sh`
- `tools/HPC_scripts/gprmax_omp_mpi.sh`
- `tools/HPC_scripts/gprmax_omp_mpi_no_spawn.sh`
- Prefer targeted source search: `rg -n "openmp|mpi|gpu|build|cleanall" gprMax tools setup.py`.
