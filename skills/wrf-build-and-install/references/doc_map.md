# WRF documentation map: Build and Install

Generated from documentation roots:
- `doc`
- `hydro/Doc`
- `chem/KPP/documentation`
- `run`
- `test`

Total docs grouped in this topic: 11

## File inventory
- `doc/README.cygwin.md` | why: Cygwin package/toolchain setup and common module mismatch fixes.
- `doc/README.cmake_build` | why: modern CMake configure/compile workflow (`configure_new`, `compile_new`).
- `doc/README.netcdf4par` | why: `NETCDFPAR` requirements and runtime `io_form=13` implications.
- `doc/README.test_cases` | why: baseline case build/run sequence and expected test directory workflow.
- `run/README.physics_files` | why: runtime physics-file dependencies required after build.
- `test/em_real/CMakeLists.txt` | why: installation/staging rules for real-data case assets and executables.
- `test/em_fire/CMakeLists.txt` | why: fire case install rules and generated test links.
- `test/em_scm_xy/CMakeLists.txt` | why: SCM case install rules and run assets.
- `test/em_les/CMakeLists.txt` | why: LES case install rules and runtime file expectations.
- `test/em_tropical_cyclone/CMakeLists.txt` | why: idealized TC case staging in test harness.
- `test/em_quarter_ss/CMakeLists.txt` | why: quarter supercell case staging and executable wiring.
