# Status — Weekly Snapshot

_Last updated: 2026-08-24_

Update this weekly (or whenever you sit down for a planning session). Keep entries short — this is a dashboard, not a diary.

## Sub-projects

| # | Project | Local directory | GitHub repo | Description |
|---|---|---|---|---|
| 1 | OGDB | `~/projects/OGDB` | [elyfant/OGDB](https://github.com/elyfant/OGDB) | Core Postgres+PostGIS asset-tracking database — gliders, sensors, sub-assemblies, calibration history, missions |
| 2 | OGDB Portal | `~/projects/OGDB-portal` | [elyfant/OGDB-portal](https://github.com/elyfant/OGDB-portal) | Web UI on top of OGDB for daily team use (mission/glider/sensor lookups); also owns calibration scheduling |
| 3/4/5 | slocum_data_processing | `~/projects/slocum_data_processing` | [elyfant/slocum_data_processing](https://github.com/elyfant/slocum_data_processing) | Raw data ingestion (SFMC/rsync), near-real-time + delayed-mode processing (PyGlider + IOOS QC), aiming for a platform-agnostic OG1 output |
| 6 | norgliders-ERDDAP | `~/projects/norgliders-ERDDAP` | [elyfant/norgliders-ERDDAP](https://github.com/elyfant/norgliders-ERDDAP) | ERDDAP data-serving layer; deployed and running, no data loaded yet, currently scoped to delayed-mode only |
| 7 | Visualization | *(none yet)* | *(none yet)* | Piloting decision support / general data viewing; unifies two legacy per-platform servers. Not started |
| 8 | SOPs & Provenance | `~/projects/norgliders/sops` | — (lives in norgliders) | Runbooks + data-provenance policy; existing SOPs are scattered and need collating/standardizing under version control |
| — | norgliders | `~/projects/norgliders` | [elyfant/norgliders](https://github.com/elyfant/norgliders) | The facility-level planning/oversight repo itself |

## 1. OGDB — `~/projects/OGDB` · [elyfant/OGDB](https://github.com/elyfant/OGDB)
- **State:** ~80% done. Operational, backfilling historical data.
- **Blocked on:** —
- **Next:** Finish backfill (~1 week of work — going through log files and archives)

## 2. OGDB Portal — `~/projects/OGDB-portal` · [elyfant/OGDB-portal](https://github.com/elyfant/OGDB-portal)
- **State:** ~80% done. Operational.
- **Blocked on:** OGDB backfill (1) — the backfill's user experience will inform what refinements the portal still needs.
- **Next:** Calibration scheduling feature (due-date tracking/alerting)

## 3. Raw Data Ingestion — `~/projects/slocum_data_processing` · [elyfant/slocum_data_processing](https://github.com/elyfant/slocum_data_processing)
- **State:** SFMC connection/event subscription + rsync raw mirror built, as part of the slocum_data_processing repo. Not yet formalized as its own standalone system.
- **Blocked on:** —
- **Next:** Not yet scheduled

## 4. NRT Processing — `~/projects/slocum_data_processing` · [elyfant/slocum_data_processing](https://github.com/elyfant/slocum_data_processing)
- **State:** Slocum pipeline in active development. Seaglider side already done (external, UW-maintained). Shared processing core (`processing/`) scaffolded per [decisions/0002](decisions/0002-shared-nrt-delayed-processing-core.md), not yet implemented.
- **Blocked on:** —
- **Next:** PyGlider config + directory convention decision, then first test mission through the pipeline. Overall goal: platform-agnostic final data product.

## 5. Delayed-Mode Processing — `~/projects/slocum_data_processing` · [elyfant/slocum_data_processing](https://github.com/elyfant/slocum_data_processing)
- **State:** Seaglider done. Slocum build starting now, sharing a processing core with #4 (see [decisions/0002](decisions/0002-shared-nrt-delayed-processing-core.md)) rather than a separate toolchain.
- **Blocked on:** #4 build decisions (config convention will likely be shared)
- **Next:** Implement `processing/pyglider_run.py` and `delayed/trigger.py` (currently stubs)

## 6. ERDDAP — `~/projects/norgliders-ERDDAP` · [elyfant/norgliders-ERDDAP](https://github.com/elyfant/norgliders-ERDDAP)
- **State:** Deployed, running, no data loaded yet. Currently scoped to delayed-mode ingestion only.
- **Blocked on:** Files from #4/#5
- **Next:** —

## 7. Visualization — *(no repo yet)*
- **State:** Two legacy per-platform servers, unification not started.
- **Blocked on:** Decision — fold into Portal or standalone
- **Next:** —

## 8. SOPs & Provenance — `~/projects/norgliders/sops`
- **State:** Existing SOPs scattered across various formats, not version-controlled.
- **Blocked on:** —
- **Next:** Collate and standardize into `sops/`, bring under version control.

---

## This Week's Focus
- OGDB backfill (going through log files and archives)
- Slocum NRT pipeline development
