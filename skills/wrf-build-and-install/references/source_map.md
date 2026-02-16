# WRF source map: Build and Install

Generated from source roots:
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

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `build`
- `configure`
- `compile`
- `cmake`
- `install`
- `netcdf`
- `registry`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`
- `rg -n "NETCDFPAR|nc-config|Registry\.EM|WRF_HYDRO|WRF_KPP" configure compile configure_new compile_new`

## Suggested source entry points (verified and practical)
- `configure` | checks: CLI option parsing, `NETCDF_C` probing, `NETCDFPAR` version guard, hard-stop diagnostics.
- `compile` | checks: target dispatch, registry selection/copying, chemistry hook (`chem/KPP/compile_wkc`), hydro link flags.
- `configure_new` | checks: CMake configuration wrapper behavior and option forwarding.
- `compile_new` | checks: CMake build/invoke flow and alternate build-dir handling.
- `CMakeLists.txt` | checks: global package discovery and top-level subdirectory build ordering.
- `external/CMakeLists.txt` | checks: external I/O backend integration ordering and linkage.
- `hydro/CMakeLists.txt` | checks: hydro library composition and optional nudging graph.
- `main/CMakeLists.txt` | checks: executable targets (`real`, `wrf`) and link dependencies.
- `test/em_real/CMakeLists.txt` | checks: runtime table linking and real-case staging.
- `test/em_fire/CMakeLists.txt` | checks: fire-case file staging and case-directory generation.
