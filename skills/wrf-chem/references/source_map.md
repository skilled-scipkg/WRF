# WRF source map: Chem

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
- `kpp`
- `wkc`
- `species`
- `registry`
- `coupler`
- `mechanism`

## Fast source navigation
- `rg -n "WRF_KPP|run_wkc|FATAL ERROR MAPPING WRF TO KPP SPECIES|configure.kpp" chem/KPP`
- `rg -n "int main\(|gen_kpp|get_wrf_chem_specs|get_kpp_chem_specs" chem/KPP/util/wkc`

## Suggested source entry points (verified and practical)
- `chem/KPP/compile_wkc` | checks: KPP build sequence, coupler build, registry trigger execution.
- `chem/KPP/configure_kpp` | checks: flex library detection and `configure.kpp` content generation.
- `chem/KPP/util/Makefile` | checks: `run_wkc` touch logic and utility build dependencies.
- `chem/KPP/util/wkc/registry_kpp.c` | function checks: `main` registry parse and coupler dispatch.
- `chem/KPP/util/wkc/gen_kpp.c` | function checks: `check_all`, `gen_kpp` species mapping and artifact generation.
- `chem/KPP/util/wkc/gen_kpp_interface.c` | function checks: `gen_kpp_interface` wrapper generation.
- `chem/KPP/util/wkc/gen_kpp_args_to_Update_Rconst.c` | function checks: Update_Rconst argument generation.
- `chem/KPP/util/wkc/do_makefiles_kpp.c` | function checks: `write_makefiles_kpp` mechanism-specific Makefile output.
- `chem/KPP/util/wkc/get_wrf_chem_specs.c` | function checks: `get_wrf_chem_specs` WRF species extraction.
- `chem/KPP/util/wkc/get_kpp_chem_specs.c` | function checks: `get_kpp_chem_specs` KPP species extraction.
