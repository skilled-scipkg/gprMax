# gprmax source map: Build and Install

Generated from source roots:
- `gprMax`
- `tools`
- `user_libs`

Use this map only after exhausting the topic docs in `doc_map.md` (same directory).

## Topic query tokens
- `build`
- `install`
- `cleanall`
- `openmp`
- `mpi`
- `gpu`

## Fast source navigation
- `rg -n "cleanall|fopenmp|/openmp|mpi|gpu|build_ext" setup.py gprMax`
- `rg -n "add_argument|mpi|gpu|benchmark" gprMax/gprMax.py`

## Suggested source entry points
- `setup.py` | build/install command handling (`cleanall`, compiler/linker options)
- `gprMax/gprMax.py` | execution mode validation and argument constraints (`api`, `run_main`)
- `gprMax/model_build_run.py` | runtime model build path after install (`run_model`)
- `gprMax/utilities.py` | host/GPU detection support code (`get_host_info`, `detect_check_gpus`)
- `gprMax/fields_updates_gpu.py` | GPU solver kernel setup
- `gprMax/source_updates_gpu.py` | GPU source update kernels
