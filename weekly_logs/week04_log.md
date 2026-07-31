# Week 04 Log — Bronze Table Construction
**Week:** 4

**Date range:** 31st July 2026 - 7th August 2026

**Team:** Data Nexus / Team02

**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Sprint Goal

Build the complete Bronze layer for the four approved TripPulse batch sources (`zones.csv`, `drivers.json`, `trips.parquet`, `payments.csv`) in Databricks by reading and validating each source, preserving source business values, adding technical ingestion metadata, persisting each dataset as a Delta table, reconciling source and Bronze row counts, and proving controlled rerun behaviour — without starting Silver, Gold, Power BI, or streaming work.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Create TripPulse Week-4 Bronze ingestion notebook `notebooks/02_bronze_ingestion.ipynb` | Team | Done | `notebooks/02_bronze_ingestion.ipynb` |
| Verify all four approved batch files in Unity Catalog Volume `/Volumes/trippulse/default/trippulsedata` | Team | Done | `screenshots/week04/week04_Volume_source_files.jpeg` |
| Read and inspect `zones.csv`, `drivers.json`, `trips.parquet`, and `payments.csv` using appropriate Spark readers | Team | Done | Week-4 notebook source sections |
| Preserve source business values and add Bronze technical metadata | Team | Done | Week-4 notebook Bronze-ready sections |
| Create Delta table `bronze_trippulse_zones` | Team | Done | Week-4 notebook |
| Create Delta table `bronze_trippulse_drivers` | Team | Done | `screenshots/week04/week04_Sample_Bronze_table.jpeg` |
| Create Delta table `bronze_trippulse_trips` | Team | Done | Week-4 notebook |
| Create Delta table `bronze_trippulse_payments` | Team | Done | Week-4 notebook |
| Reconcile source and Bronze row counts for all four datasets | Team | Done | `screenshots/week04/week04_Consolidated_reconciliation_output.jpeg` |
| Perform controlled repeat-run test and verify that the Bronze count does not increase unexpectedly | Team | Done | `screenshots/week04/week04_Repeat_run_proof.jpeg` |
| Inspect Delta table history for the rerun-tested Bronze table | Team | Done | `screenshots/week04/week04_Delta_History.jpeg` |

---

## 3. Key Decisions

- Used the existing TripPulse Unity Catalog Volume `/Volumes/trippulse/default/trippulsedata` as the source location for all Week-4 batch ingestion.
- Processed only the four approved batch sources: `zones.csv`, `drivers.json`, `trips.parquet`, and `payments.csv`.
- Kept the ride-request event/drop JSON files outside the Week-4 Bronze batch workflow because streaming processing is reserved for a later project stage.
- Preserved source business values in Bronze without applying Silver-layer cleaning, standardization, deduplication, or business rules.
- Added technical metadata to Bronze records for lineage and auditability:
  - `_source_file_name`
  - `_source_file_path`
  - `_ingested_at`
  - `_ingestion_run_id`
  - `_schema_version`
  - `_record_hash`
- Used Delta tables as the persistent Bronze storage layer instead of relying on temporary Spark views.
- Used `CREATE OR REPLACE TABLE ... USING DELTA` for the controlled Bronze writes so that rerunning the demonstrated load does not unintentionally append duplicate rows.
- Retained the project-specific explicit schema handling for `trips.parquet` required because of its Parquet nanosecond timestamp fields.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| `trips.parquet` contains timestamp fields stored as Parquet `INT64 TIMESTAMP(NANOS)` | Default Spark schema inference cannot be used reliably for the trips source | Resolved using the explicit-schema approach established during Week 3 |
| Temporary views such as `drivers_bronze_ready` are session-scoped and are not persistent Bronze tables | Could cause confusion between Bronze-ready temporary data and the actual Bronze layer | Resolved by persisting each Bronze-ready dataset as a Delta table |
| `CREATE OR REPLACE TABLE` cell displays `No rows returned` after successful execution | Could be mistaken for an empty Bronze table | Resolved by validating the created table separately with `SELECT` and row-count queries |

---

## 5. Evidence Added to GitHub

- `notebooks/02_bronze_ingestion.ipynb` — complete Week-4 TripPulse Source-to-Bronze ingestion notebook
- `screenshots/week04/README.md` — index of Week-4 evidence screenshots
- `screenshots/week04/week04_Volume_source_files.jpeg` — approved source files visible in the Unity Catalog Volume
- `screenshots/week04/week04_Sample_Bronze_table.jpeg` — sample persistent Bronze Delta table output
- `screenshots/week04/week04_Consolidated_reconciliation_output.jpeg` — consolidated source-to-Bronze row-count validation
- `screenshots/week04/week04_Repeat_run_proof.jpeg` — controlled rerun validation showing no unintended row-count increase
- `screenshots/week04/week04_Delta_History.jpeg` — Delta history of the rerun-tested Bronze table
- This log (`weekly_logs/week04_log.md`)

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | Converting the Week-4 PageLoop Bronze-ingestion pattern into a TripPulse-specific notebook using the project's four approved batch sources, actual Unity Catalog Volume path, Bronze table names, metadata fields, reconciliation checks, repeat-run validation, and Delta-history checks. AI also helped explain Databricks execution behaviour while validating the Bronze tables. |
| What we changed after AI suggestion | Updated the initial Volume path to the actual project path `/Volumes/trippulse/default/trippulsedata`; retained the TripPulse-specific `trips.parquet` explicit-schema handling from Week 3 rather than using a generic Parquet reader. |
| What we verified manually | Ran the Week-4 notebook cells in Databricks; confirmed the source files were available in the Volume; inspected Bronze table records; verified source and Bronze counts through reconciliation; checked controlled repeat-run behaviour; and inspected Delta history. |
| What we can explain without AI | The purpose of the Bronze layer, the difference between a DataFrame/temp view and a persistent Delta table, why Bronze preserves source business values, why ingestion metadata is required, how source-to-Bronze reconciliation validates ingestion completeness, and why controlled reruns must not create unintended duplicate records. |

---

## 7. Next Week Preparation

- Begin Silver-layer design using the validated Bronze Delta tables as inputs.
- Identify cleaning, standardization, type-correction, and deduplication requirements for each TripPulse dataset.
- Define data-quality checks for important business keys and relationships before creating Silver tables.
- Review Week-5 instructions and confirm the required Silver table names, transformations, and evidence before implementation.
