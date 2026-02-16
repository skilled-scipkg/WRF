---
name: wrf-build-and-install
description: This skill should be used when users ask about build and install in WRF; it prioritizes documentation references and then source inspection only for unresolved details.
---

# WRF: Build and Install

## High-Signal Playbook
- Route conditions:
  - Use this skill for configure/compile/install/toolchain/runtime-file staging questions.
  - Route to `wrf-inputs-and-modeling` for namelist streams and input-file behavior.
  - Route to `wrf-test` for case-specific run assets and expected test layouts.
  - Route to `wrf-chem` or `wrf-hydro` for subsystem-specific generation/build logic.
- Triage questions:
  - Are you using legacy scripts (`configure`/`compile`) or CMake scripts (`configure_new`/`compile_new`)?
  - Which target are you building (`em_real`, `em_fire`, `wrf`, `all_wrfvar`)?
  - Are `NETCDF`, `HDF5`, and optional `JASPER*` env vars consistent with your compiler?
  - Do you need MPI/OpenMP, chemistry, WRFDA, or hydro support at build time?
- Canonical workflow (legacy):
  1. `./clean -a`
  2. Export library paths (`NETCDF`, `NETCDF4`, `HDF5`, optional `JASPER*`).
  3. Run `./configure`.
  4. Compile target (`./compile em_real` or case-specific target).
  5. Verify executables in `main/` and stage required run tables.
  6. Run smoke test and check `rsl.out.0000` for `SUCCESS COMPLETE WRF`.
- Canonical workflow (CMake):
  1. Run `./configure_new`.
  2. Run `./compile_new -j <n>`.
  3. Verify executables in `install/bin/`.
  4. Run from `install/test/<case_name>`.

- Minimal working example (legacy)
```bash
./clean -a
export NETCDF=/usr NETCDF4=1 HDF5=/usr
export JASPER=/usr JASPERLIB=/usr/lib JASPERINC=/usr/include
./configure
./compile em_real
ls -1 main/*exe
tail -1 test/em_real/rsl.out.0000
```

- Minimal working example (CMake)
```bash
./configure_new
./compile_new -j 8
ls -1 install/bin
```

- Pitfalls and fixes:
  - `configure.wrf` missing before compile: run `./configure` first.
  - Path contains spaces: relocate the source tree to a whitespace-free path.
  - `NETCDFPAR` failure with old netCDF: use netCDF >= 4.7.4 or unset `NETCDFPAR`.
  - `all_wrfvar` failure: rerun `./configure wrfda` before compile.
  - Missing runtime physics tables: stage/link required files from `run/README.physics_files`.
- Convergence/validation checks:
  - Expected binaries exist (`main/real.exe`, `main/wrf.exe`, or `install/bin/{real,wrf}`).
  - `real.exe` creates `wrfbdy_d01` and `wrfinput_d01` for real-data runs.
  - Runtime log ends with `SUCCESS COMPLETE WRF`.
  - Required runtime physics tables for selected options are present.

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/README.cygwin.md`
- `doc/README.cmake_build`
- `doc/README.netcdf4par`
- `doc/README.test_cases`
- `run/README.physics_files`
- `test/em_real/CMakeLists.txt`
- `test/em_fire/CMakeLists.txt`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `run`
- `var/test/tutorial`

## Test references
- `test`
- `.ci/tests`
- `var/test`

## Optional deeper inspection
- `chem`
- `dyn_em`
- `external`
- `frame`
- `hydro`
- `inc`
- `main`
- `phys`
- `share`
- `tools`
- `var`
- `wrftladj`

## Source entry points for unresolved issues
- `configure`: option parsing, compiler feature checks, NetCDF capability checks, and hard-stop conditions.
- `compile`: target selection, registry switching, chemistry/DA guards, and executable expectations.
- `configure_new`: CMake configuration prompts/options and build-directory setup.
- `compile_new`: CMake build/install invocation behavior.
- `CMakeLists.txt`: top-level CMake dependency graph and package linkage order.
- `external/CMakeLists.txt`: external I/O and library build integration.
- `hydro/CMakeLists.txt`: WRF-Hydro component dependency ordering in CMake mode.
- `test/em_real/CMakeLists.txt`: canonical test install/staging of runtime tables and executables.
- `test/em_fire/CMakeLists.txt`: fire test install/staging and target wiring.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`).
