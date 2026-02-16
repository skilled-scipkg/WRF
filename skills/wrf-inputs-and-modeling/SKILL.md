---
name: wrf-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in WRF; it prioritizes documentation references and then source inspection only for unresolved details.
---

# WRF: Inputs and Modeling

## High-Signal Playbook
- Route conditions:
  - Use this skill for input datasets, namelist stream wiring, runtime I/O stream selection, and ESMF SST-coupled input behavior.
  - Route to `wrf-build-and-install` for configure/compile/toolchain failures.
  - Route to `wrf-simulation-workflows` for timestep/integration-order runtime issues.
  - Route to `wrf-test` for case-specific test assets.
- Triage questions:
  - Are you running stand-alone WRF (`wrf.exe`) or coupled SST experiment (`wrf_SST_ESMF.exe`)?
  - Are SST auxiliary streams expected in netCDF (`io_form=2`) or ESMF state (`io_form=7`)?
  - Are runtime stream edits being applied through `iofields_filename` syntax?
  - Did `real.exe` produce `wrfbdy_d01` and `wrfinput_d01` before model execution?
  - Are you reading older pre-v4 inputs (`force_use_old_data`)?
  - Which files are missing: auxiliary SST, optional metgrid fields, or track input files?
- Canonical workflow:
  1. Start from a known namelist template (`test/em_real/namelist.input.jan00` or `test/em_esmf_exp/namelist.input.jan00.*`).
  2. If needed, configure runtime I/O stream edits from `test/em_real/sample.txt` and `doc/README.io_config`.
  3. For SST-coupled experiments, build with `Registry.EM_SST` and verify `main/wrf_SST_ESMF.exe`.
  4. In `test/em_esmf_exp`, unpack `WRF_CPL_SST.tar.gz` and verify required files.
  5. Run `real.exe`, verify `wrfbdy_d01` and `wrfinput_d01`.
  6. Run coupled (`io_form_auxinput5=7`) and stand-alone (`io_form_auxinput5=2`) cases.
  7. Validate completion and compare outputs where required.

- Minimal working example
```text
# runtime stream edits (test/em_real/sample.txt)
+:h:21:u,v,w,julian
-:h:0:W,P,PB,PH,PHB,T,U,V
+:h:24:RAINC,RAINNC
```

```bash
cd test/em_esmf_exp
cp namelist.input.jan00.NETCDFSST namelist.input
../../main/real.exe
ls -l wrfbdy_d01 wrfinput_d01
../../main/wrf.exe
tail -1 rsl.out.0000
```

- Pitfalls and fixes:
  - Wrong `io_form_auxinput5`/`io_form_auxhist5`: use `7` for ESMF-coupled runs, `2` for netCDF baseline.
  - `sstin_d01_000000` missing: unpack/copy data from `test/em_esmf_exp/WRF_CPL_SST.tar.gz` workflow.
  - `wrf_SST_ESMF.exe` missing: swap to `Registry.EM_SST` and rebuild.
  - ESMF I/O with unsupported field types can fatal-stop (`ext_esmf_read_field`/`ext_esmf_write_field`).
  - Input-version rejection at startup: use newer inputs or set `force_use_old_data=.true.` intentionally.
  - Missing optional/aux inputs: inspect stream options and `share/module_optional_input.F`.
- Convergence/validation checks:
  - `real.exe` produces `wrfbdy_d01` and `wrfinput_d01`.
  - `tail -1 rsl.out.0000` reports `SUCCESS COMPLETE WRF`.
  - Coupled vs stand-alone comparison workflow is reproducible (`cmp ... | wc` equals zeros in documented recipe).
  - Expected auxiliary stream outputs (for example `sstout_d01_000000`) are produced for selected backend.

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `test/em_real/sample.txt`
- `doc/README.io_config`
- `run/README.namelist`
- `test/em_real/examples.namelist`
- `test/em_esmf_exp/README_WRF_CPL_SST.txt`

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
- `main/wrf_SST_ESMF.F`: SST dummy component wrapper, event-loop behavior, and import/export state checks.
- `external/io_esmf/ext_esmf_read_field.F90`: ESMF import-state read path and field-type guards.
- `external/io_esmf/ext_esmf_write_field.F90`: ESMF export-state write path and field-type guards.
- `share/input_wrf.F`: input compatibility checks (`force_use_old_data`) and ingest workflow.
- `share/module_optional_input.F`: optional metgrid/SST/aux input switches by stream.
- `share/track_input.F`: optional `wrfinput_track.txt` ingestion behavior.
- `share/wrf_ext_read_field.F`: backend-independent field read dispatch layer.
- `share/wrf_ext_write_field.F`: backend-independent field write dispatch layer.
- `frame/module_cpl.F`: coupler field send/receive and storage hooks.
- `main/real_em.F`: real-data initialization sequencing before runtime.
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`).
