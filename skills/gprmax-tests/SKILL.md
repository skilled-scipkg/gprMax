---
name: gprmax-tests
description: Use this skill for regression, analytical comparison, experimental comparison, and benchmarking workflows in gprMax.
---

# gprmax: Tests

## High-Signal Playbook

### Route conditions
- Use `gprmax-inputs-and-modeling` when the request is about creating/fixing model input files.
- Use `gprmax-tools` when the request is only about plotting outputs, not validating against baselines.
- Use `gprmax-parallel-hpc` when the request is about scaling job launch strategy rather than correctness checks.

### Triage questions
- Which validation category: basic regression, advanced antenna, PML, experimental, or benchmark?
- Do you need comparison against `_ref.out` files or analytical solution (`hertzian_dipole_fs_analytical`)?
- CPU or GPU path, and single-run or batch (`-n`) path?
- What acceptance criterion matters (max dB diff, waveform shape, runtime trend)?
- Are you running from repo root so relative paths in test scripts resolve correctly?

### Canonical workflow
1. Pick fixture family under `tests/models_basic`, `tests/models_advanced`, `tests/models_pmls`, `tests/experimental`, or `tests/benchmarking`.
2. Run known model(s) with `python -m gprMax <inputfile>` when validating a specific case.
3. Run `python -m tests.test_models` for automated model-vs-reference comparisons (`tests/test_models.py`).
4. Review summary max-difference output and generated diff plots in each model folder.
5. For experimental comparisons, run `python -m tests.test_experimental <modelfile> <realfile> <output>` (`tests/test_experimental.py`).
6. For performance baselines, run benchmark mode (`-benchmark`) with inputs in `tests/benchmarking` (`docs/source/benchmarking.rst`).

### Minimal working example
```bash
python -m tests.test_models
python -m tests.test_experimental \
  tests/experimental/antenna_MALA_1200_fs/antenna_MALA_1200_fs.out \
  tests/experimental/antenna_MALA_1200_fs/antenna_MALA_1200_fs_real.txt Ez
python -m gprMax tests/benchmarking/bench_100x100x100.in -benchmark
```

### Pitfalls and fixes
- `tests.test_models` defaults to `models_basic`; advanced/PML sets are commented and require explicit list edits (`tests/test_models.py`).
- Running outside repo root breaks relative paths in test scripts; run from top-level directory.
- Missing/incorrect `_ref.out` files prevents meaningful regression comparisons.
- Requested output component absent in HDF5 receiver group triggers `CmdInputError` (`tests/test_experimental.py`).
- NaNs in test data are treated as hard failures (`tests/test_models.py`).
- Experimental comparison normalizes and time-aligns by maxima; check polarity and chosen component carefully (`tests/test_experimental.py`).

### Convergence and validation checks
- Max diff in dB from `tests.test_models` should remain within expected historical envelope for the targeted model set.
- Field component sets in test and reference outputs must match exactly (`tests/test_models.py`).
- No NaNs in compared datasets (`tests/test_models.py`).
- Benchmark outputs should show plausible threading/GPU scaling trends (`docs/source/benchmarking.rst`, `tests/benchmarking/results`).

## Scope
- Handle behavior validation workflows, including reference comparisons, experimental overlays, and performance benchmarks.
- Focus on reproducibility and explicit baseline selection.

## Primary documentation references
- `tests/test_models.py`
- `tests/test_experimental.py`
- `docs/source/benchmarking.rst`
- `tests/models_basic`
- `tests/models_advanced`
- `tests/models_pmls`
- `tests/experimental`
- `tests/benchmarking`

## Workflow
- Start with existing regression fixtures before introducing new custom models.
- Use analytical and experimental fixtures to separate numerical correctness from application realism.
- Use benchmark mode only for performance characterization, not waveform correctness.

## Tutorials and examples
- `tests/models_basic/cylinder_Ascan_2D/cylinder_Ascan_2D.in`
- `tests/benchmarking/bench_100x100x100.in`

## Test references
- `tests/models_basic/*/*_ref.out`
- `tests/experimental/*/*_real.txt`
- `tests/benchmarking/results`

## Optional deeper inspection
- `gprMax`
- `tools`

## Source entry points for unresolved issues
- `tests/test_models.py` (reference and analytical comparison driver)
- `tests/test_experimental.py` (model-vs-experiment comparison logic)
- `tests/analytical_solutions.py` (analytical dipole reference computation)
- `tests/benchmarking/plot_benchmark.py` (benchmark result post-processing)
- `gprMax/gprMax.py` (benchmark and execution mode argument guards)
- `tools/plot_Ascan.py` (visual confirmation of A-scan test outputs)
- `tools/plot_Bscan.py` (visual confirmation of B-scan test outputs)
- Prefer targeted source search: `rg -n "Max diff|analytical|benchmark|NaN|_ref\.out" tests gprMax tools`.
