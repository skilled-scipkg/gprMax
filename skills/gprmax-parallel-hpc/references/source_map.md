# gprmax source map: Parallel and HPC

Generated from source roots:
- `gprMax`
- `tools`
- `tests`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `openmp`
- `mpi`
- `mpi_no_spawn`
- `gpu`
- `benchmark`
- `task`
- `parallel`
- `OMP_NUM_THREADS`

## Fast source navigation
- `rg -n "add_argument\\(|run_mpi|run_mpi_no_spawn|run_benchmark|task|gpu" gprMax/gprMax.py`
- `rg -n "^def run_model|^def solve_cpu|^def solve_gpu|srcsteps|rxsteps" gprMax/model_build_run.py`
- `rg -n "detect_check_gpus|get_host_info|OMP_NUM_THREADS|mpi-no-spawn|mpirun" gprMax/utilities.py docs/source/openmp_mpi.rst tools/HPC_scripts`

## Suggested source entry points
- `gprMax/gprMax.py` | run-mode dispatch and constraints (`run_main`, `run_mpi_sim`, `run_mpi_no_spawn_sim`, `run_benchmark_sim`)
- `gprMax/model_build_run.py` | per-model execution path and solver backend selection (`run_model`, `solve_cpu`, `solve_gpu`)
- `gprMax/utilities.py` | host/GPU detection before dispatch (`get_host_info`, `detect_check_gpus`)
- `tools/HPC_scripts/gprmax_omp.sh` | single-node OpenMP launcher template with `OMP_NUM_THREADS`
- `tools/HPC_scripts/gprmax_omp_mpi.sh` | MPI spawn launcher template (`-n`, `-mpi`)
- `tools/HPC_scripts/gprmax_omp_mpi_no_spawn.sh` | no-spawn MPI launcher template (`mpirun -n ... --mpi-no-spawn`)
- `tools/HPC_scripts/gprmax_omp_jobarray.sh` | scheduler job-array launcher template (`-task`)
- `tests/benchmarking/plot_benchmark.py` | post-process benchmark `.npz` into timing/speed-up plots
- `gprMax/fields_updates_gpu.py` | GPU E/H field update kernels for performance-path debugging
- `gprMax/source_updates_gpu.py` | GPU source update kernels for excitation-path debugging
