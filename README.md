# TripPulse: Urban Mobility Analytics

> **Student note:** Start with `00_START_HERE.md` and `00_TEMPLATE_INDEX.md`.
> The placeholder files inside this repository are the provided templates.

**Program:** ZENAIZ x BVRIT Hyderabad Data Engineering Internship Program  
**Track:** Data Engineering  
**Duration:** 12 Weeks  
**Team:** Team - 02 : Data Nexus  
**Students:** Ms. N. Geethika, Ms. Badugu Sameeksha, Ms. Shaik Sameera  
**AI Teammate:** Used responsibly for explanation, debugging, review, and documentation support.

---

## 1. Project Summary

**TripPulse – Urban Mobility Analytics** is a Data Engineering project that
simulates a ride-hailing platform using synthetic trip, driver, zone and
payment data.

The project builds a governed data pipeline that transforms raw source
datasets into validated and decision-ready business insights. The pipeline
uses Databricks and Spark SQL for data ingestion, transformation, Data
Quality validation and Gold metric generation, with Power BI used for
business analytics and visualization.

The project analyzes ride demand, trip fulfilment, driver operations,
surge pricing and payment reliability. It also includes a controlled
streaming simulation of ride-request events to demonstrate event-driven
processing and live operational metrics.

- **Domain:** Urban Mobility Analytics / Ride-hailing Operations
- **Core engineering problem:** Transform raw mobility datasets containing
  missing keys, duplicate business records, relationship mismatches and
  different data grains into trusted, validated and decision-ready
  analytics.
- **Main batch pipeline:** Raw Sources → Bronze → Silver Candidate →
  Data Quality → Trusted Silver → Gold → Power BI
- **Streaming extension:** Streaming Event JSON → Controlled Streaming
  Simulation → Validated Live Metrics
- **Final outcome:** A complete GitHub repository containing Databricks
  notebooks, Bronze/Silver/Trusted/Gold processing, Data Quality evidence,
  Gold outputs, Power BI dashboards, completed streaming simulation
  artifacts, documentation and weekly execution logs.

---

## 2. Tools Used

| Tool | Purpose |
|---|---|
| Databricks Free Edition | Spark SQL notebooks, PySpark processing, Bronze/Silver/Trusted/Gold tables and streaming simulation |
| Apache Spark / Spark SQL | Data ingestion, transformation, validation, aggregation and streaming processing |
| GitHub | Repository, weekly evidence, documentation, screenshots and project history |
| Power BI Desktop | Dashboard creation and Gold-to-Power BI validation |
| AI Assistant | Explanation, debugging, review and documentation support with manual verification |

---

## 3. Repository Navigation

| Folder / File | Purpose |
|---|---|
| `docs/` | Project documentation, data dictionary, Data Quality summary, Gold metric definitions and dashboard insights |
| `src/` | Data generation utilities and reusable Data Quality helper scripts |
| `notebooks/` | Databricks notebooks for exploration, Bronze ingestion, Silver transformation, Data Quality, Gold processing, Power BI export and streaming |
| `data_sample/` | Small sample source and Gold/export data used for repository evidence; large datasets are kept outside GitHub |
| `dashboard/` | Final TripPulse Power BI PBIX file and dashboard documentation |
| `streaming/` | Streaming design, event schema and streaming-related documentation |
| `screenshots/` | Weekly Databricks, validation and Power BI evidence |
| `weekly_logs/` | Weekly execution logs and AI transparency notes |
| `final_submission/` | Final report, demo script, contribution details and submission checklist |

---

## 4. Data Pipeline Architecture

### 4.1 Batch Pipeline

The main TripPulse batch pipeline follows:

`Raw Sources → Bronze → Silver Candidate → Data Quality → Trusted Silver → Gold → Power BI`

The pipeline is divided into controlled processing stages so that source
preservation, transformation, validation and business analytics remain
traceable.

---

### 4.2 Raw Sources

The approved TripPulse batch sources are:

- `zones.csv`
- `drivers.json`
- `trips.parquet`
- `payments.csv`

The source datasets contain different business grains and relationships.

Examples include:

- Zones represent mobility service zones.
- Drivers represent driver-level information.
- Trips represent trip/request-level information.
- Payments represent payment-attempt-level information.

The project explicitly considers these different grains when performing
joins, validation and aggregation.

---

### 4.3 Bronze Layer

The Bronze layer is the persistent raw-preserving ingestion boundary.

Bronze processing:

- Reads the approved source datasets.
- Preserves source business values.
- Adds technical ingestion metadata.
- Adds record-level lineage information.
- Generates record hashes where applicable.
- Persists the ingested datasets as Delta tables.
- Performs source-to-Bronze reconciliation.
- Validates controlled rerun behaviour.

The Bronze layer does not apply Silver-level business cleaning or
standardization.

The Week 04 Bronze implementation is documented in:

`notebooks/02_bronze_ingestion.ipynb`

---

### 4.4 Silver Candidate Layer

The Silver Candidate layer transforms Bronze data into standardized
Candidate tables before Data Quality processing.

The transformation stage includes:

- Data-type standardization.
- Timestamp handling.
- Domain standardization.
- Candidate table creation.
- Source lineage preservation.
- Record-level validation.
- Preparation for Data Quality evaluation.

The Silver Candidate implementation is documented in:

`notebooks/03_silver_transformations.ipynb`

---

### 4.5 Data Quality Layer

The Data Quality layer validates the Silver Candidate datasets using
project-specific rules.

The project uses Critical and Major DQ classifications based on the
potential impact of a rule failure on downstream analytics.

The DQ process includes checks for:

- Key completeness and uniqueness.
- Reference integrity.
- Timestamp chronology.
- Trip status consistency.
- Service and driver compatibility.
- Distance validity.
- Fare and surge validity.
- Payment key and trip integrity.
- Payment attempt and reconciliation logic.

Records are routed into two governed outcomes:

- **Trusted Silver** — records that satisfy the required validation rules.
- **Quarantine** — records that fail the applicable Data Quality rules.

Failed records are retained in Quarantine instead of being silently
discarded.

The Data Quality implementation is documented in:

`notebooks/04_data_quality_checks.ipynb`

The DQ summary is maintained in:

`docs/data_quality_summary.md`

---

### 4.6 Gold Layer

The Gold layer contains governed business-level aggregations generated
from validated Trusted Silver data.

The Gold outputs support analysis of:

- Trip operations.
- Zone demand.
- Surge impact.
- Driver performance.
- Payment reliability.
- Date-based operational trends.

The Gold layer is the reporting boundary for Power BI.

Power BI does not directly consume:

- Raw source files.
- Bronze tables.
- Silver Candidate tables.
- Quarantine tables.

---

## 5. Power BI Dashboard

The final TripPulse Power BI dashboard contains three approved pages.

### PBI-01 — Ride Operations Overview

Purpose:

Provides an overview of ride demand, completion, cancellation, unfulfilled
requests and driver response performance.

Key components:

- Total Trip Requests
- Completion Rate
- Cancellation Rate
- Unfulfilled Rate
- Average Driver Response Minutes
- Daily Requests vs Completions
- Trip Outcome by Service Type
- Top Pickup Zones by Trip Requests
- Key Insight

Filters:

- Date
- Service Type
- Pickup Zone Type

---

### PBI-02 — Zone Demand and Surge

Purpose:

Provides zone-level demand analysis, surge exposure and fulfilment
performance across pickup zones.

Key components:

- Total Trip Requests
- Completion Rate
- Cancellation Rate
- Surge Trip Share
- Average Final Fare
- Zone Demand by Date
- Surge Band Requests vs Completion
- Top Pickup Zones by Demand
- Service-Type Response-Time Comparison
- Key Insight

Filters:

- Date
- Zone ID
- Zone Type
- Surge Band

---

### PBI-03 — Driver and Payment Reliability

Purpose:

Provides driver-performance and payment-attempt analysis to identify
operational reliability areas.

Key components:

- Driver Reliability Rate
- Average Driver Response Minutes
- Average Trip Duration
- Payment Attempt Success Rate
- Average Attempts per Trip
- Driver Reliability vs Assigned Volume
- Payment Success Trend by Method
- Driver Cancellation Counts
- Payment Status and Retry Distribution
- Key Insight

Filters:

- Date
- Driver ID
- Service Type
- Payment Method

Driver-performance metrics are treated as fictional educational measures
and are not intended to represent real worker evaluations.

Payment success is measured at the payment-attempt level.

---

### Power BI Source Rule

Power BI reporting uses validated Gold outputs and approved Gold
dimensions only.

The dashboard does not use:

- Raw source files
- Bronze tables
- Silver Candidate tables
- Quarantine tables

The final dashboard is maintained under:

`dashboard/TripPulse_Dashboard_Final.pbix`

Dashboard documentation is maintained under:

`dashboard/README.md`

Dashboard insights and validation details are maintained under:

`docs/dashboard_insights.md`

---

## 6. Gold-to-Power BI Validation

The Power BI dashboard was validated against governed Gold calculations
using the validation period:

**01-Jan-2026 to 14-Jan-2026**

### PBI-01 Validation

Selected values:

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.12% | 64.12% | PASS |
| Cancellation Rate | 23.82% | 24% | PASS |

The cancellation-rate difference is only due to Power BI display rounding.

---

### PBI-02 Validation

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |
| Surge Trip Share | 55.2808% | 55.28% | PASS |
| Average Final Fare | ₹446.6148 | ₹446.61 | PASS |

---

### PBI-03 Driver Slice Validation

Driver slice:

`DRV-000008`

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Driver Reliability Rate | 88.8889% | 88.89% | PASS |
| Average Driver Response Minutes | 3.7630 | 3.76 | PASS |
| Average Trip Duration | 35.8188 minutes | 35.82 minutes | PASS |

---

### PBI-03 Payment Method Validation

Payment method slice:

`UPI`

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Payment Attempt Success Rate | 36.2996% | 36.30% | PASS |
| Average Attempts per Trip | 1.4051 | 1.41 | PASS |

The selected Power BI values matched the corresponding Gold calculations
after normal dashboard display rounding.

---

## 7. Streaming Simulation

TripPulse includes a **completed controlled streaming simulation** for
ride-request events.

The streaming component demonstrates event-driven processing separately
from the completed batch Gold and Power BI reporting model.

The completed streaming work includes:

- JSON event ingestion.
- Event schema validation.
- Event-time processing.
- Controlled event simulation.
- Streaming Data Quality handling.
- Event sequence validation.
- Lifecycle/transition validation.
- Schema-drift handling.
- Live operational metrics.

### Streaming Event Schema

The streaming event design includes fields such as:

- `event_id`
- `schema_version`
- `event_ts`
- `event_type`
- `trip_id`
- `driver_id`
- `pickup_zone_id`
- `dropoff_zone_id`
- `service_type`
- `status_from`
- `status_to`
- `surge_multiplier`
- `estimated_fare_inr`
- `producer_run_id`
- `event_sequence_no`

The streaming design also supports controlled handling of unexpected or
schema-drift fields.

### Streaming Validation

The streaming workflow considers:

- Event identity and references.
- Event-time acceptance.
- Event ordering and transitions.
- Required fields and data types.
- Schema-version handling.
- Invalid or unexpected payload handling.

The streaming implementation is maintained separately from the batch Gold
model so that the completed Power BI reporting layer remains governed by
the validated batch Gold outputs.

---

## 8. 12-Week Execution Map

| Week | Focus | Main Evidence |
|---:|---|---|
| 1 | Project framing + GitHub | README, project framing, Week 1 log |
| 2 | Dataset design | Data dictionary, assumptions, sample data plan |
| 3 | Databricks exploration | Exploration notebook, schema/count/relationship evidence |
| 4 | Bronze ingestion | Bronze notebook, source-to-Bronze reconciliation, lineage and rerun evidence |
| 5 | Silver Candidate transformation | Silver notebook, Candidate tables, lineage and transformation validation |
| 6 | Data Quality | DQ notebook, Trusted/Quarantine outputs, DQ summary and reconciliation |
| 7 | Gold metrics | Gold tables, metric definitions and validation |
| 8 | Power BI foundation | Gold exports, Gold validation, Power BI model and PBI-01 |
| 9 | Dashboard refinement | Three dashboard pages, filter validation, KPI reconciliation and insights |
| 10 | Streaming simulation | Controlled streaming notebook, event validation and live metrics |
| 11 | Integration | Pipeline walkthrough, cleaned documentation and final evidence |
| 12 | Final demo | Final report, demo script, contribution note and submission checklist |

---

## 9. Important Rules

- Power BI must use governed Gold outputs and approved Gold dimensions.
- Do not connect Power BI directly to raw, Bronze, Silver Candidate or
  Quarantine data.
- Do not submit copied internet GitHub repositories as project work.
- External references must be documented in `docs/references.md`.
- AI-generated code or content must be manually verified and explainable.
- Every project week must have a corresponding GitHub commit and weekly
  log.
- Keep large generated datasets outside GitHub.
- Keep only appropriate sample data and evidence files in the repository.
- Preserve source lineage throughout the Bronze and Silver processing
  stages.
- Data Quality failures must be routed to Quarantine rather than silently
  discarded.
- Gold metrics must be generated from governed Trusted Silver data.
- Dashboard KPIs must be reconciled against their owning Gold outputs.
- Streaming processing must remain controlled and separate from the
  validated batch Power BI reporting model.

---

## 10. Evidence and Documentation

The repository maintains evidence for each major project stage.

### Week 03 — Exploration

`notebooks/01_data_exploration.ipynb`

Source schema, grain, key-quality, relationship and join-multiplication
evidence is maintained through the Week 03 notebook and screenshots.

### Week 04 — Bronze

`notebooks/02_bronze_ingestion.ipynb`

Bronze ingestion, source-to-Bronze reconciliation, lineage metadata,
record comparison and controlled rerun evidence are maintained through
the Week 04 notebook and screenshots.

### Week 05 — Silver Candidate

`notebooks/03_silver_transformations.ipynb`

Silver Candidate transformations, lineage preservation and validation
evidence are maintained through the Week 05 notebook and screenshots.

### Week 06 — Data Quality

`notebooks/04_data_quality_checks.ipynb`

Data Quality rules, Trusted Silver, Quarantine and reconciliation evidence
are maintained through the Week 06 notebook and DQ documentation.

### Week 07 — Gold

Gold tables, metric definitions and validation evidence are maintained in
the Gold processing notebooks and supporting documentation.

### Week 08–09 — Power BI

`notebooks/06_powerbi_export.ipynb`

`dashboard/TripPulse_Dashboard_Final.pbix`

`dashboard/README.md`

`docs/dashboard_insights.md`

The Power BI evidence includes dashboard pages, filter validation,
Gold-to-Power BI reconciliation and evidence-backed insights.

### Streaming

Streaming notebooks and supporting documentation are maintained under:

`notebooks/`

`streaming/`

The repository contains evidence for the completed controlled streaming
simulation and its validation.

### Weekly Evidence

`screenshots/`

`weekly_logs/`

Each weekly log records:

- Sprint goal
- Work completed
- Key decisions
- Blockers / risks
- Evidence added to GitHub
- AI transparency
- Next week preparation

---

## 11. Final Project Proof

The completed TripPulse project demonstrates that the team:

- Designed and documented the TripPulse source datasets.
- Explored source schemas, business grains and relationships.
- Identified key-quality and relationship issues during source profiling.
- Built a persistent Bronze ingestion layer.
- Preserved source values and technical lineage in Bronze.
- Built Silver Candidate tables with standardized data.
- Implemented project-specific Data Quality rules.
- Routed invalid records to Quarantine.
- Created governed Trusted Silver outputs.
- Built Gold metric tables from validated data.
- Reconciled Gold outputs before Power BI reporting.
- Built Power BI dashboards using Gold outputs only.
- Validated dashboard KPIs against Gold calculations.
- Documented dashboard insights and synthetic-data limitations.
- Completed a controlled streaming simulation for ride-request events.
- Demonstrated streaming event validation, event-time handling and
  operational metrics.
- Maintained weekly GitHub evidence and execution logs.
- Maintained traceability across the major pipeline stages.
- Prepared a complete project that the team can explain and defend.
