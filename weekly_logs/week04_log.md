# Week 04 Log — Bronze Table Construction

**Week:** 4  
**Date range:** 31st July 2026 – 7th August 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Sprint Goal

The goal of Week 4 was to build the persistent Bronze layer for the four approved TripPulse batch source datasets: `zones.csv`, `drivers.json`, `trips.parquet`, and `payments.csv`. The work focused on preserving source business values, adding ingestion and lineage metadata, creating persistent Delta Bronze tables, reconciling source and Bronze record counts, validating record preservation, and checking controlled rerun behaviour. Silver transformations, Gold aggregation, Power BI, and streaming processing were kept outside the Week 4 scope.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Created the Week 4 Bronze ingestion notebook `notebooks/02_bronze_ingestion.ipynb` | Team | Done | `notebooks/02_bronze_ingestion.ipynb` |
| Verified the four approved TripPulse batch source files in the Databricks Volume | Team | Done | `screenshots/week04_01_bronze_tables.png` |
| Loaded `zones.csv`, `drivers.json`, `trips.parquet`, and `payments.csv` using appropriate Spark readers | Team | Done | `notebooks/02_bronze_ingestion.ipynb` |
| Created persistent Bronze Delta tables for Zones, Drivers, Trips, and Payments | Team | Done | `screenshots/week04_01_bronze_tables.png` |
| Added technical ingestion and lineage metadata to Bronze records | Team | Done | `screenshots/week04_04_bronze_schema_metadata.png` |
| Reconciled source row counts with Bronze row counts for all four datasets | Team | Done | `screenshots/week04_02_source_bronze_counts.png` |
| Compared a deterministic source record with its corresponding Bronze record to verify source-value preservation | Team | Done | `screenshots/week04_03_record_comparison.png` |
| Validated Bronze lineage metadata completeness | Team | Done | `screenshots/week04_05_event_retention.png` |
| Performed a controlled rerun and checked that Bronze record counts did not increase unexpectedly | Team | Done | `screenshots/week04_06_idempotent_rerun.png` |
| Completed the final Bronze validation checks | Team | Done | `screenshots/week04_07_final_validation.png` |

---

## 3. Key Decisions

- Used the existing TripPulse Unity Catalog Volume `/Volumes/trippulse/default/trippulsedata` as the source location for Week 4 batch ingestion.
- Processed the four approved batch sources: `zones.csv`, `drivers.json`, `trips.parquet`, and `payments.csv`.
- Kept streaming event processing outside the Week 4 implementation because the project workflow schedules streaming work for a later stage.
- Preserved source business values in Bronze without applying Silver-level cleaning, standardization, deduplication, or business transformations.
- Persisted the four Bronze datasets as Delta tables:
  - `bronze_zones_raw`
  - `bronze_drivers_raw`
  - `bronze_trips_raw`
  - `bronze_payments_raw`
- Added technical metadata for traceability and auditability, including source file, ingestion timestamp, source-row context, run identifier, record hash, and schema-version information where applicable.
- Used source-to-Bronze reconciliation to verify that the controlled batch inputs were loaded without unexpected row-count loss.
- Used a deterministic source-record comparison to verify that Bronze preserved source business values.
- Used a controlled rerun to verify that the Bronze load did not unintentionally increase the business-record count.
- Retained the explicit-schema handling for `trips.parquet` because of its Parquet nanosecond timestamp fields identified during Week 3.

---

## 4. Blockers / Risks

| Blocker / Risk | Impact | Resolution / Handling |
|---|---|---|
| `trips.parquet` contains Parquet `INT64 TIMESTAMP(NANOS)` fields | Default Spark schema inference could not reliably read the Trips source | Reused the explicit-schema approach established during Week 3 |
| Temporary Bronze-ready DataFrames/views are session-scoped | Temporary objects alone would not provide a persistent Bronze layer | Persisted the processed datasets as Delta Bronze tables |
| Successful table-creation commands do not directly display table rows | Could make a successful write appear as if no data was loaded | Validated the created Bronze tables separately using table-existence and row-count queries |
| Controlled reruns must not unintentionally create duplicate business records | An incorrect rerun could increase Bronze counts | Performed a before/after rerun validation and checked the resulting counts |

---

## 5. Evidence Added to GitHub

### Notebook

- `notebooks/02_bronze_ingestion.ipynb`

### Screenshots

- `screenshots/week04_01_bronze_tables.png` — persistent Bronze Delta tables
- `screenshots/week04_02_source_bronze_counts.png` — source-to-Bronze count reconciliation
- `screenshots/week04_03_record_comparison.png` — source and Bronze record comparison
- `screenshots/week04_04_bronze_schema_metadata.png` — Bronze schema and technical metadata
- `screenshots/week04_05_event_retention.png` — Week 4 validation output retained as supporting Bronze evidence
- `screenshots/week04_06_idempotent_rerun.png` — controlled rerun validation
- `screenshots/week04_07_final_validation.png` — final Bronze validation

### Weekly Log

- `weekly_logs/week04_log.md`

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to explain Bronze-layer ingestion patterns, Spark/Databricks behaviour, Delta tables, technical metadata, source-to-Bronze reconciliation, record comparison, and controlled rerun validation. |
| What we changed after AI suggestion | The ingestion logic was adapted to the actual TripPulse source paths, four approved batch datasets, project-specific Bronze table names, metadata fields, and the explicit-schema handling required for `trips.parquet`. |
| What we verified manually | The Week 4 notebook was executed in Databricks; source availability was checked; the four Bronze tables were inspected; source and Bronze counts were reconciled; source and Bronze records were compared; metadata completeness was checked; and controlled rerun behaviour was validated. |
| What we can explain without AI | We can explain the purpose of the Bronze layer, why source business values are preserved, why technical metadata is added, how source-to-Bronze reconciliation works, why persistent Delta tables are used, and why controlled reruns must not unintentionally create duplicate records. |

---

## 7. Next Week Preparation

- Use the validated Bronze Delta tables as the inputs for Week 5 Silver Candidate transformations.
- Identify required type conversions, standardization, derived fields, and grain-preservation requirements for each dataset.
- Define the Silver Candidate transformations without mixing them with Trusted Silver data-quality decisions.
- Validate Candidate counts, grain, lineage, and derived fields against the Bronze layer.
- Prepare the Week 5 evidence and documentation before implementing the Silver transformations.
