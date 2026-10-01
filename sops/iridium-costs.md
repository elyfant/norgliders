# Iridium costs: monthly invoice update

**Full runbook:** `~/projects/norgliders-utils/iridium-costs/docs/monthly-update.md`
([GitHub](https://github.com/elyfant/norgliders-utils/blob/main/iridium-costs/docs/monthly-update.md)).
This page is a pointer; keep the details there.

| | |
|---|---|
| **What** | Reads the Metocean iridium invoice PDFs, allocates every cost to a glider and mission, and pushes the result to OGDB. Shown in OGDB-portal → Workshop → Iridium. |
| **When** | Monthly, when the Metocean invoices arrive (one per account: 19178 Slocum, 20458 SeaGlider) |
| **Where it runs** | Fiona's workstation. It needs `/Data/gfi/projects/naco/purchasing/invoices/metocean/` (which the portal server can't see) and `ssh nrec_app`. |
| **How to run** | Save the PDFs under `<account>/<year>/`, then `~/projects/norgliders-utils/iridium-costs/update.sh` (add `--dry-run` to rehearse) |
| **Healthy looks like** | Output ends `Pushed to OGDB … invoices 2 new`, and the portal's Iridium page says "updated" just now, with the new month as the latest |
| **If it breaks** | Nothing is written unless the run reaches "Pushed to OGDB", so fixing the problem and rerunning is always safe. The runbook has a table of every error and its fix. |
| **Who** | Fiona Elliott (technical lead) |
| **Depends on** | OGDB tables `iridium_*` + `usd_nok_rates` (written as role `iridium_importer`); OGDB SIM/IMEI and mission dates; Norges Bank exchange-rate API |
