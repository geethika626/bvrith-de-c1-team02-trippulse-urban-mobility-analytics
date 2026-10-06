# Week 03 Log — Data Exploration & Source Validation

**Week:** 3  
**Date range:** 25th July 2026 - 30th July 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Sprint Goal

Profile all four Week-3 TripPulse sources (`zones.csv`, `drivers.json`, `trips.parquet`, `payments.csv`) in Databricks, validate primary-key and foreign-key relationships, demonstrate the trip-to-payment overcount risk, and build exactly one Bronze demonstration table with one downstream lineage view — without starting any Week-4 full Bronze, Silver or Gold work.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Convert the Week-3 template into `notebooks/01_data_exploration.ipynb` for TripPulse | Team | Done | `notebooks/01_data_exploration.ipynb` |
| Load all four sources as PySpark DataFrames and Spark SQL views (`zones`, `drivers`, `trips`, `payments`) | Team | Done | `screenshots/week03_01_source_files.png`, `screenshots/week03_02_dataframes.png` |
| Inspect schemas for all four sources against `docs/data_dictionary.md` | Team | Done | `screenshots/week03_03_sources_zones.png`, `screenshots/week03_04_sources_drivers.png`, `screenshots/week03_05_sources_trips.png`, `screenshots/week03_06_sources_payments.png` |
| Diagnose and fix the `trips.parquet` nanosecond-timestamp read failure (`PARQUET_TYPE_ILLEGAL`) | Team | Done | Explicit-schema fix in `notebooks/01_data_exploration.ipynb` |
| Perform grain, physical-row-count, distinct-key and value-distribution checks | Team | Done | `screenshots/week03_07_grain_counts_values.png` |
| Validate relationships between drivers, zones, trips and payments | Team | Done | `screenshots/week03_08_relationship_checks.png` |
| Demonstrate the trip-to-payment overcount risk | Team | Done | `screenshots/week03_09_overcount_demo.png` |
| Analyse the business question: highest-activity pickup zone | Team | Done | `notebooks/01_data_exploration.ipynb` |
| Create one Bronze demonstration table and one downstream lineage view | Team | Done | `notebooks/01_data_exploration.ipynb` |

---

## 3. Key Decisions

- Used Spark SQL as the primary language for profiling and relationship checks, with PySpark used for file reads, DataFrame creation, displays and simple counts.

- Excluded `ride_request_event_drop_01.json` and `ride_request_event_drop_02.json` from the Week 03 exploration because they were not part of the Week-3 Data Pack upload. Their presence in the project documentation was noted and the streaming source was flagged for confirmation before the applicable downstream stage.

- For the `trips.parquet` `INT64 TIMESTAMP(NANOS)` read failure, used an explicit-schema workaround by declaring the affected timestamp columns as `LongType` and casting them to `TimestampType` after loading.

- The configuration-based workaround using `spark.sql.legacy.parquet.nanosAsLong` was not used because the configuration was unavailable on Serverless compute (`CONFIG_NOT_AVAILABLE.WITHOUT_SUGGESTION`).

- Used left and left-anti joins for relationship validation so that unmatched child records remain visible instead of being silently removed.

- Treated Trips and Payments as different-grain datasets because one trip can have multiple payment attempts.

- Built exactly one Bronze demonstration table and one downstream lineage demonstration view, while reserving complete Bronze, Silver and Gold implementation for later weeks.

---

## 4. Blockers / Risks

| Blocker | Impact | Resolution / Handling |
|---|---|---|
| `trips.parquet` timestamp columns were stored as Parquet `INT64 TIMESTAMP(NANOS)`, which was unreadable by Spark's default inference | Blocked the Trips DataFrame read (`PARQUET_TYPE_ILLEGAL`) | Resolved using an explicit `LongType` schema followed by timestamp casting |
| `spark.sql.legacy.parquet.nanosAsLong` was unavailable on Serverless compute | The first configuration-based fix failed (`CONFIG_NOT_AVAILABLE`) | Resolved by switching to the schema-based workaround without requiring a session configuration |
| `ride_request_event_drop_01.json` and `ride_request_event_drop_02.json` were not present in the Week-3 Data Pack | Streaming event source could not be profiled as part of Week 03 | Flagged for confirmation before the applicable downstream stage |
| Trips and Payments have different grains | A direct join can multiply trip-level rows and distort trip-level metrics | Demonstrated the overcount risk using joined-row and distinct-trip counts |

---

## 5. Evidence Added to GitHub

### Notebook

- `notebooks/01_data_exploration.ipynb`

### Screenshots

- `screenshots/week03_01_source_files.png`
- `screenshots/week03_02_dataframes.png`
- `screenshots/week03_03_sources_zones.png`
- `screenshots/week03_04_sources_drivers.png`
- `screenshots/week03_05_sources_trips.png`
- `screenshots/week03_06_sources_payments.png`
- `screenshots/week03_07_grain_counts_values.png`
- `screenshots/week03_08_relationship_checks.png`
- `screenshots/week03_09_overcount_demo.png`

### Weekly Log

- `weekly_logs/week03_log.md`

The Bronze demonstration table and downstream lineage view are documented in the Week 03 notebook because separate screenshots for these two items were not retained.

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to explain Spark/PySpark concepts, source profiling approaches, relationship checks, anti-joins, the Parquet timestamp error, and trip-to-payment join fan-out analysis. |
| What we changed after AI suggestion | The initial configuration-based Parquet timestamp workaround was replaced with an explicit-schema approach after the configuration was unavailable on Serverless compute. |
| What we verified manually | Source availability, schemas, physical row counts, distinct keys, null and duplicate key counts, relationship checks, overcount results, the corrected Trips DataFrame read, the Bronze demonstration table and the downstream lineage view were verified in Databricks. |
| What we can explain without AI | We can explain the source grains and relationships, why physical rows can exceed distinct business keys, why left-anti joins are useful for relationship validation, why Trips-to-Payments joins can multiply rows, and why complete Bronze/Silver/Gold processing was outside the Week 03 scope. |

---

## 7. Next Week Preparation

- Start the complete Bronze-layer ingestion for all available TripPulse source files.
- Apply data-quality and validation checks during Bronze ingestion.
- Prepare the cleaned and validated data for the Silver-layer transformation.
- Document the Bronze-to-Silver data flow and transformation requirements.
- Confirm the expected handling and arrival of the streaming event-drop files before implementing the applicable downstream workflow.
