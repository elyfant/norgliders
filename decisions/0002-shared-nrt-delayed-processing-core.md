# Decision: NRT and delayed-mode Slocum processing share one pyglider core

**Date:** 2026-08-26

**Decision:** In `norgliders-data-pipeline` (renamed from `slocum_data_processing` 2026-09-16; systems 4/5), the pyglider decode+concatenate step lives in one mode-agnostic module (`python/src/norgliders_data_pipeline/processing/`). NRT and delayed-mode each get only a thin trigger module (`nrt/trigger.py`, `delayed/trigger.py`) that decides *when* to call into that shared core and with what raw-file set — not separate decode logic. QC (`qc/`) is kept as its own, deliberately separate module, designed later.

**Reasoning:** Pyglider itself doesn't know or care whether it's running in near-real-time or delayed mode — it decodes and concatenates whatever raw files it's given. What actually differs between the two modes is *what triggers the run* and *how complete the raw data is at that point*, not the decode logic. Two fully separate pipelines (one option considered) would duplicate that logic and risk drift when a decode fix needs to land twice. A single shared module with thin per-mode triggers keeps one source of truth while still giving each mode its own place to grow if scheduling/triggering genuinely diverges later.

**Not yet decided, tracked in `dependencies.md`:** the raw-file directory/naming convention (blocks both triggers), and whether the pyglider deployment YAML is generated from OGDB or maintained separately (blocks `processing/config.py`). The scaffolded modules are stubs (`NotImplementedError`) pending those.
