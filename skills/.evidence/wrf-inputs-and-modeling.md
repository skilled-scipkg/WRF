# Evidence: wrf-inputs-and-modeling

## Primary docs
- `test/em_real/sample.txt`
- `test/em_esmf_exp/README_WRF_CPL_SST.txt`

## Primary source entry points
- `skills/wrf-inputs-and-modeling/references/doc_map.md`
- `main/wrf_SST_ESMF.F`
- `external/io_esmf/ext_esmf_write_field.F90`
- `external/io_esmf/ext_esmf_read_field.F90`
- `var/external/crtm_2.3.0/libsrc/CRTM_Geometry_Define.f90`
- `var/external/crtm_2.3.0/libsrc/CRTM_GeometryInfo_Define.f90`
- `var/external/crtm_2.3.0/libsrc/CRTM_GeometryInfo.f90`
- `share/wrf_ext_write_field.F`
- `share/wrf_ext_read_field.F`
- `tools/gen_model_data_ord.c`
- `share/module_model_constants.F`
- `share/mediation_force_domain.F`
- `phys/module_fr_fire_model.F`
- `share/track_input.F`
- `share/module_optional_input.F`
- `share/input_wrf.F`
- `main/real_nmm.F`
- `main/real_em.F`
- `frame/module_cpl_oasis3.F`
- `frame/module_cpl.F`

## Extracted headings
- sample input file for runtime config of I/O streams
- This toy example adds the state variables u,v,w,and julian
- to the set of variables that are output with auxiliary
- history stream auxhist21.
- At the same time different the same and different variables
- may be removed from the standard output stream.
- For additional information see README.io_config in top-level directory

## Executable command hints
- $WRFDIR/Registry >> mv -f Registry.EM Registry.EM_ORIG
- $WRFDIR/Registry >> cp -f Registry.EM_SST Registry.EM
- $WRFDIR >> echo 5 | configure
- $WRFDIR >> compile em_real >&! compile.em_real.5.out
- $WRFDIR >> ls -1 main/*exe
- $WRFDIR/test/em_esmf_exp >> gunzip WRF_CPL_SST.tar.gz
- $WRFDIR/test/em_esmf_exp >> tar xvf WRF_CPL_SST.tar
- $WRFDIR/test/em_esmf_exp >> ls -l sstin_d01_000000 namelist.input.jan00.* *.csh
- $WRFDIR/test/em_esmf_exp >> diff namelist.input.jan00.ESMFSST namelist.input.jan00.NETCDFSST
- $WRFDIR/test/em_esmf_exp >> llsubmit real.csh
- $WRFDIR/test/em_esmf_exp >> bsub < real.lsf.csh
- $WRFDIR/test/em_esmf_exp >> ls -al wrfbdy_d01 wrfinput_d01

## Warnings and pitfalls
- (none extracted)
