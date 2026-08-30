# norgliders

Facility-level planning and oversight layer for the **Ocean Glider
Facility**, Geophysical Institute, University of Bergen.

This repo contains **no application code**. Start at
[SYSTEM_MAP.md](SYSTEM_MAP.md) for the architecture, then
[status.md](status.md) for what's happening right now.

- [SYSTEM_MAP.md](SYSTEM_MAP.md) — the systems, how they connect, current status of each
- [status.md](status.md) — weekly snapshot dashboard
- [dependencies.md](dependencies.md) — cross-repo contracts (formats, paths, IDs, schemas)
- [decisions/](decisions/) — one short file per standalone architectural decision
- [sops/](sops/) — runbooks per system

## Sub-projects

| # | Project | Local directory | GitHub repo | Description |
|---|---|---|---|---|
| 1 | OGDB | `~/projects/OGDB` | [elyfant/OGDB](https://github.com/elyfant/OGDB) | Core Postgres+PostGIS asset-tracking database — gliders, sensors, sub-assemblies, calibration history, missions |
| 2 | OGDB Portal | `~/projects/OGDB-portal` | [elyfant/OGDB-portal](https://github.com/elyfant/OGDB-portal) | Web UI on top of OGDB for daily team use (mission/glider/sensor lookups); also owns calibration scheduling |
| 3/4/5 | slocum_data_processing | `~/projects/slocum_data_processing` | [elyfant/slocum_data_processing](https://github.com/elyfant/slocum_data_processing) | Raw data ingestion (SFMC/rsync), near-real-time + delayed-mode processing (PyGlider + IOOS QC), aiming for a platform-agnostic OG1 output |
| 6 | norgliders-ERDDAP | `~/projects/norgliders-ERDDAP` | [elyfant/norgliders-ERDDAP](https://github.com/elyfant/norgliders-ERDDAP) | ERDDAP data-serving layer; deployed and running, no data loaded yet, currently scoped to delayed-mode only |
| 7 | Visualization | *(none yet)* | *(none yet)* | Piloting decision support / general data viewing; unifies two legacy per-platform servers. Not started — open question whether it folds into OGDB Portal or stands alone |
| 8 | SOPs & Provenance | `~/projects/norgliders/sops` | — (lives in norgliders) | Runbooks + data-provenance policy; existing SOPs are scattered and need collating/standardizing under version control |
| — | norgliders | `~/projects/norgliders` | [elyfant/norgliders](https://github.com/elyfant/norgliders) | The facility-level planning/oversight repo itself — no application code, houses `SYSTEM_MAP.md`, `status.md`, `dependencies.md`, `decisions/`, `sops/` |

Source of truth for this table is [SYSTEM_MAP.md](SYSTEM_MAP.md) — update there too whenever this changes.
