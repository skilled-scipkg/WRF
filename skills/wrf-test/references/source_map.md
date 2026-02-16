# WRF source map: Test

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
- `fire`
- `scm`
- `forcing`
- `windturbine`
- `ideal`
- `real`

## Fast source navigation
- `rg -n "create_links|fire_driver_em|force_scm|scmflux|scmskintemp|windturbines" test dyn_em phys main`
- `rg -n "subroutine\s+(fire_driver_em_init|fire_driver_em_step|force_scm|init_domain_rk|scmflux|scmskintemp)" dyn_em phys`

## Suggested source entry points (verified and practical)
- `test/em_fire/create_links.sh` | checks: expected fire subdirectory/link creation.
- `phys/module_fr_fire_driver_wrf.F` | function checks: `fire_driver_em_init`, `fire_driver_em_step`.
- `dyn_em/module_initialize_fire.F` | function checks: `init_domain_rk`, `get_sounding`, `read_sounding`.
- `dyn_em/module_initialize_scm_xy.F` | function checks: `init_domain_rk`, `read_sounding`, `read_soil`.
- `dyn_em/module_force_scm.F` | function checks: `force_scm` forcing application and guardrails.
- `phys/module_sf_scmflux.F` | function checks: `scmflux` surface flux injection for SCM.
- `phys/module_sf_scmskintemp.F` | function checks: `scmskintemp` skin-temperature forcing logic.
- `dyn_em/module_initialize_real.F` | function checks: `init_domain_rk` real-data init path.
- `main/real_em.F` | function checks: `real_data` program flow and preprocessing sequence.
- `main/module_wrf_top.F` | function checks: `wrf_run` execution loop for test cases.
