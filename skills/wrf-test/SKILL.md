---
name: wrf-test
description: This skill should be used when users ask about test in WRF; it prioritizes documentation references and then source inspection only for unresolved details.
---

# WRF: Test

## High-Signal Playbook
- Route conditions:
  - Use this skill for packaged test cases (fire, SCM, real-data examples) and expected test assets/layout.
  - Route to `wrf-build-and-install` for compile toolchain failures.
  - Route to `wrf-inputs-and-modeling` for stream/auxinput semantics.
- Triage questions:
  - Which test family is needed (`em_fire`, `em_scm_xy`, `em_real`)?
  - Are you running idealized (`ideal.exe`) or real-data (`real.exe` + `wrf.exe`) flow?
  - Are required linked files/directories present (especially for `em_fire`)?
  - For SCM, do forcing file cadence and periodic boundaries match `README.scm`?
  - For wind turbines, is `windturbines.txt` in expected 3-column format?
- Canonical workflow:
  1. Select target case directory under `test/`.
  2. Compile matching target (`./compile em_fire`, `./compile em_scm_xy`, or `./compile em_real`).
  3. Stage case namelist/input files.
  4. Run initialization executable (`ideal.exe` or `real.exe`) and verify generated inputs.
  5. Run `wrf.exe` and inspect `rsl.out.0000`.
  6. Validate outputs against case expectations.

- Minimal working example (SCM)
```bash
./compile em_scm_xy
cd test/em_scm_xy
../../main/ideal.exe
../../main/wrf.exe
tail -1 rsl.out.0000
```

- Minimal working example (fire links)
```bash
cd test/em_fire
./create_links.sh
cd two_fires
ls ideal.exe wrf.exe namelist.input namelist.fire input_sounding
```

```text
# wind turbine entry format (test/em_real/windturbines.txt)
55.574051 6.883480 1
```

- Pitfalls and fixes:
  - `em_fire` subdirectories missing: rerun `./compile em_fire` (or `test/em_fire/create_links.sh`).
  - `./clean -a` removed fire subdirs: rerun `./compile em_fire`.
  - SCM setup not single-column-like: enforce periodic boundaries and SCM settings from `test/em_scm_xy/README.scm`.
  - SCM forcing mismatch: align `auxinput3_inname`/interval with forcing file contents.
  - Invalid wind turbine list format: use `lat lon category` rows.
  - SCM forcing-array inconsistencies can fatal-stop in `dyn_em/module_force_scm.F`.
- Convergence/validation checks:
  - Fire case directories and links exist after compile (`two_fires`, `rain`).
  - Initialization artifacts are produced (`wrfinput*`, `wrfbdy*`, or ideal initial fields).
  - Runtime ends with `SUCCESS COMPLETE WRF`.
  - Case-specific tendencies/fluxes are non-zero where expected (for example fire heat/moisture tendencies).

## Scope
- Handle questions about documentation grouped under the 'test' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/README.test_cases`
- `test/em_fire/README.txt`
- `test/em_scm_xy/README.scm`
- `test/em_scm_xy/GABLS_II_forcing.txt`
- `test/em_real/windturbines.txt`
- `doc/README.windturbine`

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
- `test/em_fire/create_links.sh`: authoritative fire-test link/staging logic.
- `phys/module_fr_fire_driver_wrf.F`: fire-atmosphere initialization/step coupling hooks.
- `dyn_em/module_initialize_fire.F`: fire ideal-case initialization workflow.
- `dyn_em/module_initialize_scm_xy.F`: SCM initialization and forcing-file assumptions.
- `dyn_em/module_force_scm.F`: SCM large-scale/advection forcing and safety checks.
- `phys/module_sf_scmflux.F`: SCM surface-flux forcing routine.
- `phys/module_sf_scmskintemp.F`: SCM skin-temperature forcing routine.
- `dyn_em/module_initialize_real.F`: real-data initialization path used by `em_real`.
- `main/real_em.F`: executable-level real initialization call path.
- `main/module_wrf_top.F`: runtime entry/finalization flow for test executions.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`).
