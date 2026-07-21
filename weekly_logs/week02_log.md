# Week 02 Log — Data Pack, Contracts & Assumptions

**Week:** 2  
**Date range:** 18 July 2026 – 24 July 2026    
**Team:** Data Nexus / 02  
**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Sprint Goal

The goal for Week 02 was to understand the provided TripPulse datasets, define the data contracts and assumptions, and prepare the required project documentation. We focused on verifying the schemas of all source files and documenting the metadata needed for downstream processing.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Analyzed `trips.parquet` dataset schema | Team | Done | `data_dictionary.md` |
| Verified source file schemas (`drivers.json`, `payments.csv`, and `zones.csv`) | Team | Done | `data_dictionary.md` |
| Reviewed streaming event JSON file structures | Team | Done | `data_dictionary.md` |
| Updated project assumptions | Team | Done | `synthetic_data_assumptions.md` |
| Verified field names, data types, and sample records | Team | Done | Manual validation |

---

## 3. Key Decisions

- The provided datasets will be used directly without generating synthetic data.
- Source schemas and assumptions will be documented before implementing data ingestion pipelines.
- Null values will be handled according to the trip status during future transformations.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| Presence of null values in some fields | Requires careful handling during transformations | Define data quality rules |
| Dataset relationships require further validation | May affect joins across datasets | Verify mappings during implementation |


---

## 5. Evidence Added to GitHub

- Updated `data_dictionary.md` with source file schemas and field descriptions.
- Updated `synthetic_data_assumptions.md` with project assumptions.
- Verified and documented dataset structures for the provided source files.
---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI assisted in understanding dataset schemas, preparing documentation templates, and suggesting validation steps. |
| What we changed after AI suggestion | Dataset-specific information was manually updated after examining the provided files. |
| What we verified manually | Field names, data types, null values, and sample records from all datasets were manually verified. |
| What we can explain without AI | The dataset structure, assumptions made, and documentation prepared for Week 02 can be independently explained. |


---

## 7. Next Week Preparation

- Begin Bronze layer data ingestion.
- Implement data quality checks for source datasets.
- Define transformations for the Silver layer.
- Validate relationships between trips, drivers, payments, and zones datasets.

