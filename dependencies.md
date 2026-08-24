# Cross-Project Dependencies

Explicit contracts between systems — file formats, paths, IDs, schemas that more than one repo relies on. When one side changes, check this file for what else breaks.

## OGDB ↔ NRT/Delayed-Mode Processing
- Mission metadata (mission_id, glider_id) referenced by processing scripts — **format/location TBD**. Decide: does PyGlider deployment YAML get generated from OGDB, or maintained separately and just cross-referenced by ID?

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
