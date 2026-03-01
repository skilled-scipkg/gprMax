---
name: gprmax-advanced-topics
description: Use this skill for low-frequency documentation topics that do not justify standalone routing skills (references, analytical comparisons, include-readme internals, and top-level guide context).
---

# gprmax: Advanced Topics

## Scope
- Consolidated routing for low-signal one-doc topics.
- Covers documentation context from:
  - top-level user-guide index
  - include-readme linkage
  - analytical comparison writeup
  - bibliography/references section

## Route the request
- If the question is about setup or first run, use `gprmax-getting-started`.
- If the question is about model commands or physics setup, use `gprmax-inputs-and-modeling`.
- If the question is about execution scaling (MPI/OpenMP/GPU), use `gprmax-parallel-hpc`.
- If the question is about plotting/output interpretation, use `gprmax-tools` or `gprmax-analysis-and-output`.
- Use this skill when the user specifically asks for analytical-comparison narrative, reference citations, or documentation-structure navigation.

## Primary documentation references
- `docs/source/index.rst`
- `docs/source/include_readme.rst`
- `docs/source/comparisons_analytical.rst`
- `docs/source/references.rst`

## Workflow
- Start with the primary docs above.
- Keep answers short and link-forward to the correct core skill when actionable simulation steps are required.
- Escalate to source only if docs do not resolve an implementation detail.

## Source entry points for unresolved issues
- `tests/analytical_solutions.py` (analytical field references used by tests)
- `tests/models_basic/hertzian_dipole_fs_analytical/hertzian_dipole_fs_analytical.in` (analytical comparison fixture)
- `tests/test_models.py` (where analytical vs simulated comparisons are executed)
- `gprMax/_version.py` (version metadata referenced by docs and outputs)
- `gprMax/__init__.py` (package metadata entry point)
- Prefer targeted source search: `rg -n "analytical|reference|citation|version" tests gprMax docs/source`.
