# Week 05 Log — Silver Candidate Transformation

**Week:** 5  
**Date range:** 07th August 2026 - 14th August 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Sprint Goal

Build the Silver Candidate layer by transforming Bronze data into standardized Candidate tables for trips, drivers, payments, and zones.  
Validate row counts, hash integrity, parsing, table creation, and Delta format.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Created Silver Candidate table for trips | Team | Done | `week05_silver_trips_candidate_sample.png` |
| Created Silver Candidate table for drivers | Team | Done | `week05_silver_drivers_candidate_sample.png` |
| Created Silver Candidate table for payments | Team | Done | `week05_silver_payments_candidate_sample.png` |
| Created Silver Candidate table for zones | Team | Done | `week05_silver_zones_candidate_sample.png` |
| Validated Bronze-to-Candidate row counts | Team | Done | `week05_Silver_Candidate_Row_Count_Validation.png` |
| Validated Candidate hash integrity | Team | Done | `week05_Silver_Candidate_Hash_Integrity_Validation.png` |
| Checked parse failures | Team | Done | `week05_Silver_Parse_Failure_Validation.png` |
| Verified Candidate tables and Delta format | Team | Done | `week05_Silver_Tables_and_Delta_Format_Verification.png` |
| Verified Bronze tables and baseline counts | Team | Done | `week05_bronze_tables_verification.png`, `week05_bronze_baseline_count.png` |

---

## 3. Key Decisions

- Created separate Silver Candidate tables for drivers, payments, trips, and zones.
- Preserved Bronze-to-Candidate row counts and verified hash integrity.
- Used Delta format for the Silver Candidate tables.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| No major blockers encountered | None | Not required |
| Final Silver layer is not yet completed | Candidate layer only at this stage | Continue with the next planned stage |

---

## 5. Evidence Added to GitHub

- `notebooks/03_silver_transformations.ipynb`
- `week05_bronze_tables_verification.png`
- `week05_bronze_baseline_count.png`
- `week05_silver_drivers_candidate_sample.png`
- `week05_silver_payments_candidate_sample.png`
- `week05_silver_trips_candidate_sample.png`
- `week05_silver_zones_candidate_sample.png`
- `week05_Silver_Candidate_Row_Count_Validation.png`
- `week05_Silver_Candidate_Hash_Integrity_Validation.png`
- `week05_Silver_Parse_Failure_Validation.png`
- `week05_Silver_Tables_and_Delta_Format_Verification.png`

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to assist with notebook structure, SQL logic, documentation, and validation queries. |
| What we changed after AI suggestion | We reviewed and adapted the suggested logic to match the project's Bronze and Silver Candidate table structure. |
| What we verified manually | Table creation, row counts, hash integrity, parse failures, schemas, and Delta format were manually checked in Databricks. |
| What we can explain without AI | We can explain the Bronze-to-Silver Candidate transformation, table structure, validation checks, and the purpose of each validation. |

---

## 7. Next Week Preparation

- Review and prepare the Data Quality rules and validation process.
- Continue from the completed Silver Candidate tables toward the next pipeline stage.
