# Status — Weekly Snapshot

_Last updated: 2026-08-24_

Update this weekly (or whenever you sit down for a planning session). Keep entries short — this is a dashboard, not a diary.

## 1. OGDB
- **State:** ~80% done. Operational, backfilling historical data.
- **Blocked on:** —
- **Next:** Finish backfill (~1 week of work — going through log files and archives)

## 2. OGDB Portal
- **State:** ~80% done. Operational.
- **Blocked on:** OGDB backfill (1) — the backfill's user experience will inform what refinements the portal still needs.
- **Next:** Calibration scheduling feature (due-date tracking/alerting)

## 3. Raw Data Ingestion
- **State:** SFMC connection/event subscription + rsync raw mirror built, as part of the ogdp repo. Not yet formalized as its own standalone system.
- **Blocked on:** —
- **Next:** Not yet scheduled

## 4. NRT Processing
- **State:** Slocum pipeline in active development. Seaglider side already done (external, UW-maintained).
- **Blocked on:** —
- **Next:** PyGlider config + directory convention decision, then first test mission through the pipeline. Overall goal: platform-agnostic final data product.

## 5. Delayed-Mode Processing
- **State:** Seaglider done. Slocum not started (shares toolchain with #4).
- **Blocked on:** #4 build decisions (config convention will likely be shared)
- **Next:** —

## 6. ERDDAP
- **State:** Deployed, running (`norgliders-ERDDAP` repo), no data loaded yet. Currently scoped to delayed-mode ingestion only.
- **Blocked on:** Files from #4/#5
- **Next:** —

## 7. Visualization
- **State:** Two legacy per-platform servers, unification not started.
- **Blocked on:** Decision — fold into Portal or standalone
- **Next:** —

## 8. SOPs & Provenance
- **State:** Existing SOPs scattered across various formats, not version-controlled.
- **Blocked on:** —
- **Next:** Collate and standardize into `sops/`, bring under version control.

---

## This Week's Focus
- OGDB backfill (going through log files and archives)
- Slocum NRT pipeline development
