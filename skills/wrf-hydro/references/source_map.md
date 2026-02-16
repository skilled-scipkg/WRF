# WRF source map: Hydro

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
- `hydro`
- `routing`
- `forcing`
- `namelist`
- `nwm`
- `coupling`

## Fast source navigation
- `rg -n "WRF_HYDRO_NUDGING|NETCDF_INC|NETCDF_LIB|HYDRO_stop|CHRTOUT_DOMAIN|READFORC" hydro`
- `rg -n "subroutine\s+(wrf_drv_HYDRO|rt_allocate|READFORC_WRF|output_chrt_NWM|HYDRO_stop|rt_nlst_check)" hydro`

## Suggested source entry points (verified and practical)
- `hydro/configure` | checks: compiler selection and NetCDF include/lib fallback logic.
- `hydro/CMakeLists.txt` | checks: hydro component graph and nudging-dependent compile branches.
- `hydro/CPL/WRF_cpl/wrf_drv_HYDRO.F90` | function checks: `wrf_drv_HYDRO`, `wrf_drv_HYDRO_ini` coupling entry points.
- `hydro/OrchestratorLayer/config.F90` | function checks: `config_init`, `rt_nlst_check`, `init_namelist_rt_field` namelist validation.
- `hydro/Routing/module_RT.F90` | function checks: `rt_allocate`, `LandRT_ini`, `deriveFromNode` routing state setup.
- `hydro/Routing/module_lsm_forcing.F90` | function checks: `READFORC_WRF`, `read_hydro_forcing_seq`, metadata guardrails.
- `hydro/Routing/module_NWM_io.F90` | function checks: `output_chrt_NWM`, `output_rt_NWM`, `nwmCheck` output validation.
- `hydro/utils/module_hydro_stop.F90` | function checks: `HYDRO_stop` fatal-stop path.
