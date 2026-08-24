# Decision: Calibration scheduling lives in OGDB Portal, not OGDB core

**Date:** 2026-08-24

**Decision:** Calibration due-date tracking and alerting is a feature of OGDB Portal, built on top of calibration coefficients already stored in OGDB.

**Reasoning:** OGDB stores the data (calibration coefficients as JSONB). Scheduling/alerting is a workflow on top of that data, not storage itself — consistent with the OGDB/Portal split (core data vs. operational UI/workflow).
