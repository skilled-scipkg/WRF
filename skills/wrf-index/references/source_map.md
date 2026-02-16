# WRF source map: Index bootstrap

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

Use this map only after exhausting the docs in `doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`
- `rg -n "subroutine\s+|function\s+|PROGRAM" main dyn_em share frame hydro chem`

## Suggested source entry points (verified paths)
- `configure` | checks: CLI option parsing and NetCDF/NETCDFPAR compatibility guards.
- `compile` | checks: target wiring (`em_real`, `em_fire`, `wrf`), registry switching, KPP trigger.
- `configure_new` | checks: CMake configuration path and option handoff.
- `compile_new` | checks: CMake build invocation and install staging behavior.
- `main/module_wrf_top.F` | checks: top-level runtime orchestration (`wrf_init`, `wrf_run`, `wrf_finalize`).
- `dyn_em/solve_em.F` | checks: ARW solve loop entry and timestep progression.
- `share/input_wrf.F` | checks: input-file compatibility and runtime input guards.
- `chem/KPP/compile_wkc` | checks: chemistry code-generation bootstrap before main compile.
- `hydro/configure` | checks: hydro-specific compiler and NetCDF include/lib resolution.
- `hydro/CPL/WRF_cpl/wrf_drv_HYDRO.F90` | checks: hydro coupling call interface.
