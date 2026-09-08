# Cross-Project Dependencies

Explicit contracts between systems — file formats, paths, IDs, schemas that more than one repo relies on. When one side changes, check this file for what else breaks.

## OGDB ↔ NRT/Delayed-Mode Processing
- **RESOLVED (2026-09-08, [decision 0003](decisions/0003-ogdb-generated-deployment-config.md)):** the pyglider `deployment.yml` is **generated from OGDB per processing run** and written into the mission's data folder (provenance-stamped). It is not hand-maintained in the repo. `slocum_data_processing.processing.config.resolve(mission_number)` is the generator.
- **Contract:**
  - The processing script locates the mission folder by globbing `<data-root>/<NNN>-*` (the mission-number prefix). `missions.internal_data_path` is **not** reliable for this (inconsistent / half-populated).
  - `resolve()` reads: `norglider_missions` + `missions` FK ids; `asset_glider_details` + `assets` + `platforms`/NVS B76; science payload via the recursive `asset_assignments` walk at `launch_date`; latest `asset_{ct,do,eco}_sensor_cal` ≤ `launch_date`; `projects.funder`/`fund_number`.
  - Only `processing.l1_time_range` (and later QC narrative) is set by a human, after inspecting L0. `--regenerate` refreshes the OGDB-derived block only.
  - The `netcdf_variables` block stays repo-side (pyglider config schema, keyed by sensor model) — OGDB is not asked to model it.
- **Pending OGDB schema additions** (do not block starting `resolve()`): `missions.summary` (+ Add-Mission modal auto-fill); NVS C19 sea-area terms + a `mission_sea_names` junction (many-to-many, separate from `site`); NVS L22 device-model terms + `asset_sensor_details.l22_model_id`.
- Downstream: `OGDB/scripts/ingest_slocum_mission.py` (already exists) reads the pyglider L2 and writes `missions` dates/track + sets `l1_file`/`l2_file`.

## NRT/Delayed-Mode Processing → ERDDAP
- Output format: **OG1 NetCDF**
- Expected file landing path on ERDDAP server: **TBD**
- Dataset registration (ERDDAP XML config) — manual or scripted?

## Raw Ingestion → NRT Processing
- Directory structure / naming convention: **TBD** — needs to be defined before NRT pipeline build goes much further, since the pipeline will assume a fixed layout

## OGDB → Visualization (future)
- If visualization needs glider/mission metadata (not just processed data), same question as above applies: direct DB query, API, or flat reference?

---

_Add a new section any time a decision creates a contract between two systems. If it's not written here, assume the two systems disagree with each other._
