---
name: wrf-index
description: This skill should be used when users ask how to use WRF and the correct generated documentation skill must be selected before going deeper into source code.
---

# WRF Skills Index

## Route the request
- Classify the request into one generated topic skill first.
- Prefer documentation-first guidance for large scientific packages; inspect source only when docs leave ambiguity.

## Fast start from `skills/`
1. Pick one topic skill below.
2. Read that topic `SKILL.md` and follow its playbook.
3. If details are still missing, open `skills/<topic>/references/doc_map.md`.
4. If behavior is still unclear, open `skills/<topic>/references/source_map.md` and run targeted symbol searches.

## Generated topic skills
- `wrf-build-and-install`: build, installation, compilation, environment setup.
- `wrf-inputs-and-modeling`: namelists, input streams, coupled SST input behavior.
- `wrf-simulation-workflows`: runtime flow, timestepping, integration cadence.
- `wrf-test`: packaged test cases and expected run assets.
- `wrf-chem`: WRF-Chem KPP/WKC generation and mechanism mapping.
- `wrf-hydro`: WRF-Hydro configuration, forcing, routing, coupling.

## Index references
- `references/doc_map.md`
- `references/source_map.md`

## Documentation-first inputs
- `doc`
- `hydro/Doc`
- `chem/KPP/documentation`

## Tutorials and examples roots
- `run`
- `var/test/tutorial`

## Test roots for behavior checks
- `test`
- `.ci/tests`
- `var/test`

## Escalate only when needed
- Start from topic skill primary references.
- Then inspect `skills/<topic>/references/doc_map.md`.
- Then inspect `skills/<topic>/references/source_map.md`.
- Use targeted symbol search while inspecting source (for example: `rg -n "<symbol_or_keyword>" chem dyn_em external frame hydro inc main phys share tools var wrftladj`).

## Source directories for deeper inspection
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
