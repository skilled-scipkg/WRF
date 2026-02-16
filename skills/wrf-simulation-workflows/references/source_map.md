# WRF source map: Simulation Workflows

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
- `timestep`
- `cfl`
- `rk`
- `integration`
- `boundary`
- `date`
- `history_interval`

## Fast source navigation
- `rg -n "adapt_timestep|max_horiz_cfl|max_vert_cfl|history_interval|interval_seconds" dyn_em share run`
- `rg -n "subroutine\s+(wrf_run|solve_em|adapt_timestep|geth_newdate|lbc_read_time)" main dyn_em share`

## Suggested source entry points (verified and practical)
- `main/module_wrf_top.F` | function checks: `wrf_init`, `wrf_run`, `wrf_finalize` call flow.
- `dyn_em/solve_em.F` | function checks: `solve_em` core integration loop behavior.
- `dyn_em/adapt_timestep_em.F` | function checks: `adapt_timestep`, `calc_dt`, CFL-limited dt decisions.
- `dyn_em/module_small_step_em.F` | function checks: `small_step_prep`, `advance_all`, `small_step_finish`.
- `dyn_em/module_first_rk_step_part1.F` | function checks: `first_rk_step_part1` stage-1 dynamics/physics path.
- `dyn_em/module_big_step_utilities_em.F` | function checks: `calc_p_rho_phi`, `rhs_ph`, `horizontal_pressure_gradient`.
- `dyn_em/module_em.F` | function checks: `rk_tendency`, `rk_update_scalar`, `rk_addtend_dry`.
- `share/module_date_time.F` | function checks: `geth_newdate`, `geth_idts`, `calc_current_date`.
- `share/module_bc_time_utilities.F` | function checks: `lbc_read_time`, `set_time_to_read_again`, `get_time_to_read_again`.
