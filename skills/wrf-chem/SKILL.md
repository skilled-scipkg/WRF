---
name: wrf-chem
description: This skill should be used when users ask about chem in WRF; it prioritizes documentation references and then source inspection only for unresolved details.
---

# WRF: Chem

## High-Signal Playbook
- Route conditions:
  - Use this skill for WRF-Chem KPP coupler generation, mechanism wiring, and registry-to-mechanism mapping failures.
  - Route to `wrf-build-and-install` for generic toolchain/linker issues not specific to KPP/WKC.
  - Route to `wrf-inputs-and-modeling` for runtime chemistry input-stream behavior.
- Triage questions:
  - Are chemistry/KPP options enabled at configure time?
  - Is `WRF_KPP=1` set before compile so `chem/KPP/compile_wkc` executes?
  - Is `flex` and `libfl` discoverable by `chem/KPP/configure_kpp`?
  - Did registry edits require forcing coupler regeneration (`util/run_wkc` trigger)?
  - Which mechanism/package fails mapping (`*.spc`, `*_wrfkpp.equiv`)?
  - Is the working path excessively long (known yacc/path sensitivity)?
- Canonical workflow:
  1. Configure WRF with chemistry/KPP options.
  2. Export `WRF_KPP=1`.
  3. Run `./compile em_real` to trigger `chem/KPP/compile_wkc`.
  4. `compile_wkc` runs `configure_kpp`, builds KPP binaries, links WKC utility, and executes coupler generation.
  5. Verify regenerated outputs under `chem/` and `inc/`.
  6. Resolve any species-mapping fatal messages before rerunning.

- Minimal working example
```bash
./configure chem kpp
export WRF_KPP=1
./compile em_real
```

```bash
# direct coupler regeneration path used by compile_wkc
chem/KPP/util/wkc/registry_kpp -DDA_CORE=0 -DEM_CORE=1 -DBUILD_CHEM=1 Registry/Registry.EM_CHEM
```

```bash
# post-generation artifact checks
ls chem/module_kpp_*_interface.F chem/kpp_mechanism_driver.F
ls inc/call_to_kpp_mech_drive.inc inc/args_to_update_rconst.inc
```

- Pitfalls and fixes:
  - `libfl` not found: set `FLEX_LIB_DIR` or install flex libraries.
  - Missing `configure.wrf`: run top-level `./configure` first.
  - Species mapping fatal-stop: inspect mechanism `.spc` and optional `*_wrfkpp.equiv` mapping files.
  - Stale generated code after registry changes: ensure `util/run_wkc` trigger path executes.
  - Very long absolute path can break KPP yacc flow: shorten WRF root path.
  - Generated interface/include files missing: rerun `compile_wkc` and inspect generator diagnostics.
- Convergence/validation checks:
  - `chem/KPP/configure.kpp` is regenerated.
  - `chem/module_kpp_*_interface.F` and `chem/kpp_mechanism_driver.F` are regenerated.
  - `inc/call_to_kpp_mech_drive.inc` and `inc/args_to_update_rconst.inc` are present.
  - No `FATAL ERROR MAPPING WRF TO KPP SPECIES` during coupler generation.
  - Main WRF compile proceeds past chemistry code-generation stage.

## Scope
- Handle questions about documentation grouped under the 'chem' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `chem/KPP/documentation/wkc_kpp.txt`
- `chem/KPP/documentation/gpl/gpl_wkc.txt`
- `chem/KPP/documentation/gpl/gpl_kpp.txt`
- `doc/README.DA_chem`

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
- `chem/KPP/compile_wkc`: end-to-end WKC/KPP build-and-generate driver.
- `chem/KPP/configure_kpp`: flex/libfl detection and `configure.kpp` generation.
- `chem/KPP/util/Makefile`: `run_wkc` regeneration trigger and utility build path.
- `chem/KPP/util/wkc/registry_kpp.c`: coupler main entry (`main`) and registry parse path.
- `chem/KPP/util/wkc/gen_kpp.c`: species mapping checks and generated-file orchestration.
- `chem/KPP/util/wkc/gen_kpp_interface.c`: generation of `module_kpp_*_interface.F` wrappers.
- `chem/KPP/util/wkc/gen_kpp_args_to_Update_Rconst.c`: generation of update-constant argument lists.
- `chem/KPP/util/wkc/do_makefiles_kpp.c`: per-mechanism KPP Makefile generation.
- `chem/KPP/util/wkc/get_wrf_chem_specs.c`: extraction of WRF chemistry species.
- `chem/KPP/util/wkc/get_kpp_chem_specs.c`: extraction of KPP mechanism species.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`).
