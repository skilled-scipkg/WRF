---
name: wrf-simulation-workflows
description: This skill should be used when users ask about simulation workflows in WRF; it prioritizes documentation references and then source inspection only for unresolved details.
---

# WRF: Simulation Workflows

## High-Signal Playbook
- Route conditions:
  - Use this skill for runtime flow questions: timestep control, integration order, boundary-read cadence, and output timing.
  - Route to `wrf-inputs-and-modeling` for preprocessing, input-stream setup, and auxiliary input semantics.
  - Route to `wrf-test` for case-specific packaged recipes.
- Triage questions:
  - Is the issue variable naming/mapping (`gribmap`) or integration behavior (CFL/time-step)?
  - Is adaptive timestep enabled and producing instability?
  - Is this a nested run with parent-child timestep mismatch?
  - Are boundary updates aligned with `interval_seconds`?
  - Are output timestamps aligned with `history_interval` and run length?
- Canonical workflow:
  1. Confirm variable aliases in `run/gribmap.txt` for required fields.
  2. Validate `&time_control` cadence (`start/end`, `interval_seconds`, `history_interval`) in `run/README.namelist`.
  3. Run case (`ideal.exe`/`real.exe` then `wrf.exe`) and inspect `rsl.out.*` for CFL/timestep warnings.
  4. For adaptive stepping behavior, inspect `dyn_em/adapt_timestep_em.F` and `dyn_em/solve_em.F`.
  5. For time/date rollovers and boundary-read cadence, inspect `share/module_date_time.F` and `share/module_bc_time_utilities.F`.
  6. Validate run completion and output cadence.

- Minimal working example
```bash
./compile em_squall2d_x
cd test/em_squall2d_x
../../main/ideal.exe
../../main/wrf.exe
tail -1 rsl.out.0000
```

```text
# representative GRIB-to-WRF aliases (run/gribmap.txt)
11:TMP:Temp. [K]:TT,T2,TSK,SKINTEMP:2
33:UGRD:u wind [m/s]:U,UU,U10,UZ0:3
34:VGRD:v wind [m/s]:V,VV,V10,VZ0:3
80:WTMP:Water temp. [K]:SST,SSTSK:2
```

- Pitfalls and fixes:
  - Wrong GRIB alias selection causes missing fields: verify aliases in `run/gribmap.txt` first.
  - Adaptive timestep swings too aggressively: tighten min/max timestep and CFL controls.
  - Nest timestep mismatch: inspect parent/child quantization in adaptive-step logic.
  - Boundary cadence mismatch: align model dt and `interval_seconds`; inspect boundary-time utilities.
  - Date rollover confusion: validate date conversion helpers in `share/module_date_time.F`.
- Convergence/validation checks:
  - Timestep remains within configured min/max range.
  - CFL statistics remain near target and below instability thresholds.
  - Boundary reads occur at intended cadence.
  - Output timestamps match `history_interval` and run length.
  - Run terminates with `SUCCESS COMPLETE WRF`.

## Scope
- Handle questions about simulation setup, execution flow, and runtime controls.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `run/gribmap.txt`
- `run/README.namelist`
- `doc/README.test_cases`
- `doc/README.rsl_output`

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
- `main/module_wrf_top.F`: top-level run orchestration (`wrf_init`, `wrf_run`, `wrf_finalize`).
- `dyn_em/solve_em.F`: ARW solve loop entry and timestep progression.
- `dyn_em/adapt_timestep_em.F`: adaptive timestep computation and CFL-based limiting.
- `dyn_em/module_small_step_em.F`: small-step state updates and fast-mode stepping.
- `dyn_em/module_first_rk_step_part1.F`: first RK step flow and tendency wiring.
- `dyn_em/module_big_step_utilities_em.F`: large-step utilities used in integration cycle.
- `dyn_em/module_em.F`: RK tendency/update routines used during integration.
- `share/module_date_time.F`: date parsing and time-difference routines.
- `share/module_bc_time_utilities.F`: boundary re-read scheduling utilities.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`).
