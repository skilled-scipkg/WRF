# Evidence: wrf-build-and-install

## Primary docs
- `doc/README.cygwin.md`
- `test/em_tropical_cyclone/CMakeLists.txt`
- `test/em_squall2d_y/CMakeLists.txt`
- `test/em_squall2d_x/CMakeLists.txt`
- `test/em_seabreeze2d_x/CMakeLists.txt`
- `test/em_scm_xy/CMakeLists.txt`
- `test/em_real/CMakeLists.txt`
- `test/em_quarter_ss/CMakeLists.txt`
- `test/em_les/CMakeLists.txt`
- `test/em_hill2d_x/CMakeLists.txt`
- `test/em_heldsuarez/CMakeLists.txt`
- `test/em_grav2d_x/CMakeLists.txt`

## Primary source entry points
- `skills/wrf-build-and-install/references/doc_map.md`
- `wrftladj/CMakeLists.txt`
- `tools/CMakeLists.txt`
- `share/CMakeLists.txt`
- `phys/CMakeLists.txt`
- `main/CMakeLists.txt`
- `inc/CMakeLists.txt`
- `hydro/CMakeLists.txt`
- `frame/CMakeLists.txt`
- `external/CMakeLists.txt`
- `dyn_em/CMakeLists.txt`
- `chem/CMakeLists.txt`
- `tools/CodeBase/CMakeLists.txt`
- `hydro/utils/CMakeLists.txt`
- `hydro/Routing/CMakeLists.txt`
- `hydro/OrchestratorLayer/CMakeLists.txt`
- `hydro/nudging/CMakeLists.txt`
- `hydro/MPP/CMakeLists.txt`
- `hydro/IO/CMakeLists.txt`
- `hydro/HYDRO_drv/CMakeLists.txt`

## Extracted headings
- Installing WRF on Cygwin
- These are just rules for this case, could be named CMakeLists.txt or something
- like install_rules.cmake, whatever you want really
- ${PROJECT_SOURCE_DIR}/run/CAMtr_volume_mixing_ratio.SSP245 # Has alt name, why?

## Executable command hints
- ${PROJECT_SOURCE_DIR}/run/README.namelist
- ${PROJECT_SOURCE_DIR}/run/LANDUSE.TBL
- ${PROJECT_SOURCE_DIR}/run/RRTM_DATA
- ${PROJECT_SOURCE_DIR}/run/GENPARM.TBL
- ${PROJECT_SOURCE_DIR}/run/SOILPARM.TBL
- ${PROJECT_SOURCE_DIR}/run/VEGPARM.TBL
- ${PROJECT_SOURCE_DIR}/run/README.physics_files
- ${PROJECT_SOURCE_DIR}/run/ETAMPNEW_DATA
- ${PROJECT_SOURCE_DIR}/run/ETAMPNEW_DATA.expanded_rain
- ${PROJECT_SOURCE_DIR}/run/RRTMG_LW_DATA
- ${PROJECT_SOURCE_DIR}/run/RRTMG_SW_DATA
- ${PROJECT_SOURCE_DIR}/run/CAM_ABS_DATA

## Warnings and pitfalls
- (none extracted)
