# Ocean Glider Facility — System Map

_Last updated: 2026-08-24_

This document is the single source of truth for the software architecture of the facility. It is maintained here, not duplicated into individual project repos. Individual repos should link back to this file rather than copy it.

## Purpose

This repo is a **planning and oversight layer**, not an application. It contains no shared code and no dependencies that other repos import. Its job is to answer: *what are we building, why, how do the pieces connect, and what's the state of each piece right now.*

---

## The Map

### 1. OGDB — Ocean Glider Database
Core asset-tracking database: gliders, sensors, sub-assemblies, calibration history, and missions.

- **Status:** ~80% done. Operational, actively being backfilled with historical data. Needs some refinement as the web app (2) develops and surfaces new requirements.
- **Repo:** [`~/projects/OGDB`](https://github.com/elyfant/OGDB)
- **Stack:** PostgreSQL + PostGIS

### 2. OGDB Portal
UI layer on top of OGDB for daily team use — mission/glider/sensor lookups. Also owns **calibration scheduling** (due-date tracking and alerting, built on the calibration coefficients stored in OGDB — see [decisions/0001](decisions/0001-calibration-scheduling-ownership.md)).

- **Status:** ~80% done. Waiting on OGDB's backfill (~1 week of work, going through log files and archives) — that backfill's user experience will inform what refinements the portal still needs.
- **Repo:** [`~/projects/OGDB-portal`](https://github.com/elyfant/OGDB-portal)
- **Depends on:** OGDB (1)
- **Stack:** React admin framework (Refine or react-admin, TBD) + hand-built operational screens

### 3. Raw Data Ingestion
Dockserver/basestation landing raw glider files off Freewave/Iridium — directory structure, naming convention, retention policy, failure detection (disk full, silent callback failures).

- **Status:** Not yet formalized as its own system. SFMC connection/event subscription + rsync raw mirror already built as part of (4)'s repo.
- **Repo:** [`~/projects/slocum_data_processing`](https://github.com/elyfant/slocum_data_processing)
- **Feeds:** NRT Processing (4)

### 4. Near-Real-Time Data Processing
- **Seaglider:** done — UW's existing pipeline, housed in different servers/repos. May be documented here but not built here.
- **Slocum:** to build — pipeline using PyGlider + IOOS QC toolbox
- **Output:** OG1-format NetCDF files → delivered to ERDDAP (6)
- **Status:** In active development (Slocum side). Goal is a platform-agnostic final data product, not a Slocum-only one.
- **Repo:** [`~/projects/slocum_data_processing`](https://github.com/elyfant/slocum_data_processing)
- **Depends on:** Raw Data Ingestion (3)

### 5. Delayed-Mode Processing
- **Seaglider:** done, existing toolset in good shape.
- **Slocum:** to build — same PyGlider + IOOS QC toolchain as (4), delayed-mode QC pass.
- **Status:** Seaglider done, Slocum not started.
- **Repo:** [`~/projects/slocum_data_processing`](https://github.com/elyfant/slocum_data_processing) (shared with 4)

### 6. ERDDAP Server
Data serving layer for glider mission datasets.

- **Status:** Deployed, running, no data loaded yet. Currently scoped for delayed-mode ingestion only.
- **Repo:** [`~/projects/norgliders-ERDDAP`](https://github.com/elyfant/norgliders-ERDDAP)
- **Depends on:** NRT Processing (4), Delayed-Mode Processing (5)
- **Future:** near-real-time serving; national archive/DAC submission (IOOS Glider DAC / NCEI) not yet scoped

### 7. Visualization — Real-Time + Delayed-Mode
Piloting decision support and general data viewing.

- **Status:** Not started. Two separate legacy servers exist today (one per glider type). Goal: unified tool.
- **Repo:** `<TBD — new build>`
- **Open question:** fold into OGDB Portal (2) or build standalone?
- **Possible sub-component:** mission monitoring/alerting (comms timeout, missed surfacing, battery/ballast thresholds)

### 8. SOPs & Provenance
Not a software system — documentation and process.

- Existing SOPs already exist, but scattered across various formats — need collating, standardizing, and bringing under version control.
- Runbooks per system: where it runs, how to restart it, who to call.
- Data provenance/versioning policy for reprocessed missions (updated cal coefficients, new PyGlider versions — are old outputs retained?).
- **Status:** Not started as a version-controlled system.
- **Location:** this planning repo, under [`sops/`](sops/).

---

## Cross-Cutting Concerns (not owned by any single system)

- 🚩 **Authorization/identity** — OGDB Portal, ERDDAP, and Visualization will each want auth. No shared identity story yet. Flagged for future decision before retrofitting becomes painful. **Do not solve this yet** — just don't make per-app choices that would block a future shared solution.

---

## Dependency Flow (high level)

```
Raw Ingestion (3)
      │
      ▼
NRT Processing (4) ──┐
      │              │
Delayed Processing(5)│
      │              │
      └──────┬───────┘
             ▼
        ERDDAP (6)
             │
             ▼
      Visualization (7)

OGDB (1) ──► OGDB Portal (2) ◄── (calibration scheduling lives here)
      ▲
      └── referenced by missions in (4)/(5), and possibly by (7)
```

---

## Change Log
_Add an entry whenever the map changes shape — new system, re-scoped ownership, dependency change._

- 2026-08-24 — Initial map drafted. Ordering finalized: OGDB → Portal → Raw Ingestion → NRT Processing → Delayed Processing → ERDDAP → Visualization → SOPs. Calibration scheduling assigned to Portal. Auth flagged for later.
- 2026-08-24 — Repo names/links resolved against actual GitHub repos: systems 3/4/5 all live in `ogdp` (the Slocum pipeline project); system 6 identified as the existing `norgliders-ERDDAP` repo; systems 7/8 confirmed to have no repo yet.
- 2026-08-26 — `ogdp` renamed to `slocum_data_processing` (too easily confused with `OGDB`). Its `erddap/` ingest module (already moved to `norgliders-ERDDAP/ingest/` in a prior session) removed as a leftover. Systems 4/5 given a shared processing-core architecture — see [decisions/0002](decisions/0002-shared-nrt-delayed-processing-core.md).
