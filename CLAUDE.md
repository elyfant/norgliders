# Planning Repo — Instructions for Claude Code

This repo is the facility-level planning and oversight layer for Fiona's Ocean Glider facility software. It contains **no application code**. Its purpose is high-level system tracking across several sibling repos.

## What this repo is for
- Maintaining `SYSTEM_MAP.md` as the current architecture/ownership map
- Tracking per-project status in `status.md`
- Recording cross-project contracts in `dependencies.md`
- Recording standalone decisions in `decisions/` (one short file per decision)
- Housing SOPs/runbooks in `sops/` (see system #8 in the map)

## What this repo is NOT for
- No shared libraries or utility code — if code is needed here, that's a sign a different architectural decision is being made and should be flagged to Fiona explicitly, not just built
- Not a place to duplicate code from other repos

## Sibling repos
The other facility repos are cloned as siblings to this directory:
- `../OGDB/` — OGDB core database
- `../OGDB-portal/` — OGDB Portal UI
- `../slocum_data_processing/` — Slocum near-real-time + delayed-mode processing pipeline (covers system map items 3, 4, 5)
- `../norgliders-ERDDAP/` — ERDDAP data-serving layer (system map item 6)
- (add others as they're created — items 7 Visualization and 8 SOPs have no repo yet)

When asked to check the map for staleness or drift, read the sibling repos' code/README/config directly rather than relying on what `SYSTEM_MAP.md` currently claims — the map is a snapshot, the code is the truth.

## Typical session pattern
A planning session here usually looks like:
1. Read `status.md`, `SYSTEM_MAP.md`, and `dependencies.md` for current state
2. Optionally read into sibling repos to verify claims or check specific implementation details
3. Update the relevant markdown file(s) based on what's found or decided
4. If a new dependency or decision emerges, log it in `dependencies.md` or `decisions/`

## Fiona's context
Fiona is a systems engineer / oceanographic instrumentation specialist (not a professional developer) acting as product lead across this facility's software. She uses AI extensively for architecture and implementation. Keep suggestions grounded in what's maintainable by a small team — flag over-engineering rather than defaulting to it.
