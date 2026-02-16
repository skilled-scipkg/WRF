---
name: wrf-hydro
description: This skill should be used when users ask about hydro in WRF; it prioritizes documentation references and then source inspection only for unresolved details.
---

# WRF: Hydro

## High-Signal Playbook
- Route conditions:
  - Use this skill for WRF-Hydro configuration, coupling hooks, routing/forcing I/O, and hydro output behavior.
  - Route to `wrf-build-and-install` for generic WRF build/toolchain questions.
  - Route to `wrf-inputs-and-modeling` when issues are atmospheric stream setup rather than hydro internals.
- Triage questions:
  - Are you running fully coupled WRF-Hydro or hydro-focused component tests?
  - Is `WRF_HYDRO=1` set before WRF configure/compile?
  - Which hydro compiler profile is selected in `hydro/configure` (`gfort`, `ifort`, etc.)?
  - Are `NETCDF_INC` and `NETCDF_LIB` resolved (or derivable from `NETCDF`/`nc-config`)?
  - Are forcing/static files carrying required fields and attributes?
  - Are expected outputs disabled by namelist settings (`CHRTOUT_DOMAIN`, `channel_bypass`, etc.)?
- Canonical workflow:
  1. Read hydro run/build guidance (`doc/README.hydro`, `hydro/Doc/link_to_documentation.txt`).
  2. Set hydro-related environment (`WRF_HYDRO=1`, `NETCDF` or explicit `NETCDF_INC`/`NETCDF_LIB`).
  3. Run `./hydro/configure <compiler_key>` and verify generated `hydro/macros`.
  4. Configure/compile WRF with hydro enabled.
  5. Stage hydro runtime templates (`hydro/template/HYDRO/hydro.namelist`, `HYDRO.TBL`, `CHANPARM.TBL`) into run directory.
  6. Run coupled case and validate hydro outputs/logs.

- Minimal working example
```bash
export WRF_HYDRO=1 HYDRO_D=1
export NETCDF=/path/to/netcdf
./hydro/configure gfort
test -f hydro/macros && ls -d hydro/lib hydro/mod
./configure
./compile em_real
```

```text
# forcing variables consumed by READFORC_WRF (module_lsm_forcing.F90)
T2 Q2 U10 V10 PSFC GLW SWDOWN RAINC RAINNC VEGFRA LAI
```

- Pitfalls and fixes:
  - `NETCDF_INC`/`NETCDF_LIB` unresolved: define env vars or ensure `nc-config` is on `PATH`.
  - No compiler profile selected: pass a valid `hydro/configure` argument; script exits without `macros` otherwise.
  - Missing Fortran netCDF linkage behavior: verify `libnetcdff` handling in `hydro/configure`.
  - Forcing/static files missing required attrs (`ISWATER`, `ISOILWATER`, `ISLAKE`) or dimensions: correct NetCDF metadata.
  - Missing expected channel outputs: verify `CHRTOUT_DOMAIN`/`channel_bypass` settings and output flags.
  - Hydro namelist path errors: verify files staged from `hydro/template/HYDRO`.
- Convergence/validation checks:
  - `hydro/macros` is generated and `hydro/lib`, `hydro/mod` exist after configure.
  - Hydro forcing/static files open cleanly with no `HYDRO_stop` fatal.
  - Expected hydro output families are produced when enabled.
  - Coupled run completes without early routing/forcing fatal-stop.

## Scope
- Handle questions about documentation grouped under the 'hydro' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `hydro/Doc/link_to_documentation.txt`
- `doc/README.hydro`
- `hydro/template/HYDRO/hydro.namelist`

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
- `hydro/configure`: compiler/profile selection and NetCDF include/lib resolution.
- `hydro/CMakeLists.txt`: hydro component build graph and optional nudging wiring.
- `hydro/CPL/WRF_cpl/wrf_drv_HYDRO.F90`: WRF-to-hydro coupling entry points.
- `hydro/OrchestratorLayer/config.F90`: hydro namelist checks and fatal validations.
- `hydro/Routing/module_RT.F90`: routing-domain allocation/state setup.
- `hydro/Routing/module_lsm_forcing.F90`: forcing/static NetCDF ingestion and required variable/attribute checks.
- `hydro/Routing/module_NWM_io.F90`: hydro output product generation and gating logic.
- `hydro/utils/module_hydro_stop.F90`: centralized hydro fatal-stop behavior.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`).
