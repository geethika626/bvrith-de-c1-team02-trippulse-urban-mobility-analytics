# Week 03 Log — Data Exploration & Source Validation

**Week:** 3  
**Date range:** 25th July 2026 - 30th July 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Sprint Goal

Profile all four Week-3 TripPulse sources (zones.csv, drivers.json, trips.parquet, payments.csv) in Databricks, prove primary-key/foreign-key relationships, demonstrate the trip-to-payment overcount risk, and build exactly one Bronze demonstration table with one downstream lineage view — without starting any Week-4 (full Bronze, Silver, Gold) work.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Convert PageLoop Week-3 template into `notebooks/01_data_exploration.ipynb` for TripPulse | Team | Done | `notebooks/01_data_exploration.ipynb` |
| Load all four sources as PySpark DataFrames + Spark SQL views (`zones`, `drivers`, `trips`, `payments`) | Team | Done | `screenshots/week03/week03_01_source_files.png`, `screenshots/week03/week03_02_dataframes.png` |
| Inspect schema for all four sources vs. `docs/data_dictionary.md` | Team | Done | `screenshots/week03/week03_03_sources_zones.png`, `screenshots/week03/week03_03_sources_drivers.png`, `screenshots/week03/week03_03_sources_trips.png`, `screenshots/week03/week03_03_sources_payments.png` |
| Diagnose and fix `trips.parquet` nanosecond-timestamp read failure (`PARQUET_TYPE_ILLEGAL`) | Team | Done | Explicit-schema fix in notebook Section 3.3 |
| Grain, physical-row-count, distinct-key, and value-distribution checks | Team | Done | `screenshots/week03/week03_04_grain_count_physical_rows.png`, `screenshots/week03/week03_04_grain_count_business_keys.png` |
| Relationship checks (driver→zone, trip→driver, trip→pickup/dropoff zone, payment→trip) | Team | Done | `screenshots/week03/week03_05_relationship_checks.png` |
| Trip-to-payment overcount demonstration (joined rows vs. distinct trips) | Team | Done | `screenshots/week03/week03_06_overcount.png` |
| Business question: highest-activity pickup zone | Team | Done | Notebook Section 15 |
| One Bronze demo table: `trippulse_week03_bronze_demo_trips` | Team | Done | `screenshots/week03/week03_07_bronze_demo.png` |
| One lineage demo view: `trippulse_week03_lineage_demo_view` + Catalog Explorer lineage graph | Team | Done | `screenshots/week03/week03_08_lineage.png` |

---

## 3. Key Decisions

- Used Spark SQL as the primary language for profiling and relationship checks, PySpark only for file reads, DataFrame creation, displays, and simple counts, per project convention.
- Excluded `ride_request_event_drop_01.json` / `_02.json` from this notebook — they're listed in `docs/data_dictionary.md` but were not part of the Week-3 Data Pack upload. Flagged for confirmation before Week 4.
- For the `trips.parquet` `INT64 TIMESTAMP(NANOS)` read failure, chose the explicit-schema workaround (declare the six `*_ts` columns as `LongType`, then cast to `TimestampType` after load) over `spark.conf.set("spark.sql.legacy.parquet.nanosAsLong", "true")`, because the config-based fix is blocked on Serverless compute (`CONFIG_NOT_AVAILABLE.WITHOUT_SUGGESTION`) while the explicit-schema approach works on any compute type.
- Used left/left-anti joins (never inner joins) for every relationship check, so unmatched child rows stay visible instead of being silently dropped.
- Built exactly one Bronze demo table and one lineage view, deliberately stopping short of a full Bronze layer, reconciliation framework, or Silver/Gold work — reserved for Week 4

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| `trips.parquet` timestamp columns stored as Parquet `INT64 TIMESTAMP(NANOS)`, unreadable by Spark's default inference | Blocked the trips DataFrame read entirely (`PARQUET_TYPE_ILLEGAL`) | Resolved — explicit `LongType` schema + post-read cast to timestamp |
| `spark.sql.legacy.parquet.nanosAsLong` not available on Serverless compute | First attempted fix failed (`CONFIG_NOT_AVAILABLE`) | Resolved — switched to schema-based fix, no session config needed |
| `ride_request_event_drop_01/02.json` referenced in Week-2 data dictionary but not present in this Data Pack | Cannot profile streaming event source this week | Confirm with mentor whether these are expected for Week 3 or arrive in Week 4 |

---

## 5. Evidence Added to GitHub

- `notebooks/01_data_exploration.ipynb` — full Week-3 TripPulse exploration notebook
- `screenshots/week03_source_files.png` — Volume file listing
- `screenshots/week03_dataframes.png` — DataFrame creation confirmation
- `screenshots/week03_sources_zones.png` — zones schema/source inspection
- `screenshots/week03_sources_drivers.png` — drivers schema/source inspection
- `screenshots/week03_sources_trips.png` — trips schema/source inspection
- `screenshots/week03_sources_payments.png` — payments schema/source inspection
- `screenshots/week03_grain_count_business_keys.png` — distinct business-key counts
- `screenshots/week03_grain_count_physical_rows.png` — physical row counts
- `screenshots/week03_relationship_checks.png` — FK relationship checks
- `screenshots/week03_overcount.png` — trip-to-payment overcount demo
- `screenshots/week03_bronze_demo.png` — Bronze demo table creation and preview
- `screenshots/week03_lineage.png` — Catalog Explorer lineage graph
- This log (`weekly_logs/week03_log.md`)

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | Converting the PageLoop Week-3B template into a TripPulse-specific notebook (renaming all entities, columns, paths, tables, and business questions to match the assigned project); diagnosing two Databricks execution errors (`PARQUET_TYPE_ILLEGAL` on `trips.parquet`, and `CONFIG_NOT_AVAILABLE` when the first fix attempt failed on Serverless compute) and proposing fixes. |
| What we changed after AI suggestion | Replaced the initial `spark.conf.set(...)` fix with the explicit-schema (`LongType` for nanosecond columns) approach after it failed on Serverless compute; verified the fix against the actual Parquet footer schema before applying it. |
| What we verified manually | Ran every notebook cell in Databricks and recorded actual counts/results (the notebook intentionally marks execution-dependent findings as "Run and record actual result" rather than using AI-generated numbers); confirmed the fixed `trips_df` read against `docs/data_dictionary.md`; checked the Catalog Explorer lineage graph reflects the created Bronze table and view. |
| What we can explain without AI | The trip/driver/zone/payment grain and relationships, why physical rows can exceed distinct business keys, why a left-anti join (not an inner join) is the correct tool for a relationship check, why joining `trips` to `payments` inflates row counts, and the Week-3 vs. Week-4 scope boundary. |

---

## 7. Next Week Preparation

- Start the complete Bronze-layer ingestion for all available TripPulse source files.
- Apply data-quality and validation checks during Bronze ingestion.
- Prepare the cleaned and validated data for the Silver-layer transformation.
- Document the Bronze-to-Silver data flow and transformation requirements.
