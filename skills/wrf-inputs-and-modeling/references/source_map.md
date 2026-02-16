# WRF source map: Inputs and Modeling

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
- `input`
- `iofields`
- `auxinput`
- `esmf`
- `sst`
- `stream`
- `namelist`

## Fast source navigation
- `rg -n "io_form_auxinput5|iofields_filename|force_use_old_data|wrfinput_track|auxinput" run share main frame external`
- `rg -n "subroutine\s+(input_wrf|optional_input|optional_sst|track_input|ext_esmf_read_field|ext_esmf_write_field)" share external`

## Suggested source entry points (verified and practical)
- `main/wrf_SST_ESMF.F` | function checks: `sst_component_init1`, `read_data`, `compare_data` for coupled SST workflow.
- `external/io_esmf/ext_esmf_read_field.F90` | function checks: `ext_esmf_read_field` handling of import-state and field-type constraints.
- `external/io_esmf/ext_esmf_write_field.F90` | function checks: `ext_esmf_write_field` export-state writes and type constraints.
- `share/input_wrf.F` | function checks: `input_wrf`, `is_this_data_ok_to_use`, `check_which_switch`.
- `share/module_optional_input.F` | function checks: `optional_input`, `optional_sst`, `optional_metgrid`.
- `share/track_input.F` | function checks: `track_input` and missing-track-file behavior.
- `share/wrf_ext_read_field.F` | function checks: `wrf_ext_read_field_arr`, `wrf_ext_read_field` dispatch behavior.
- `share/wrf_ext_write_field.F` | function checks: `wrf_ext_write_field_arr`, `wrf_ext_write_field` dispatch behavior.
- `frame/module_cpl.F` | function checks: `cpl_rcv`, `cpl_snd`, `cpl_store_input` coupler data movement.
- `main/real_em.F` | function checks: `med_sidata_input`, `compute_si_start_and_end` and initialization sequencing.
