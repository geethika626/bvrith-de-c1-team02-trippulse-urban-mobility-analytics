# Week 06 Log — Data Quality, Trusted Silver & Quarantine

**Week:** 6  
**Date range:** 14th August 2026 - 20st August 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse - Urban Mobility Analytics

---

## 1. Sprint Goal

The goal of Week 6 was to apply the approved Data Quality rules to the TripPulse Silver Candidate tables and establish governed Trusted Silver and Quarantine outputs. The work focused on validating reference integrity, keys, timestamps, lifecycle consistency, service compatibility, distance, fare, payment and reconciliation rules, routing failed records to Quarantine, routing valid records to Trusted Silver, and proving entity-level reconciliation without silent record loss.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Verified the Silver Candidate tables received from Week 5 | Team | Done | `screenshots/week06_01_candidate_handoff.png` |
| Applied Zone Data Quality and reference-integrity checks | Team | Done | `screenshots/week06_02_zone_dq.png` |
| Applied Driver reference-integrity checks | Team | Done | `screenshots/week06_03_driver_dq.png` |
| Applied Trip key, reference, timestamp, status, service, distance and fare/surge validation rules | Team | Done | `screenshots/week06_04_trip_dq_results.png` |
| Reconciled Trip Candidate records with Trusted Silver and Quarantine outputs | Team | Done | `screenshots/week06_05_trip_reconciliation.png` |
| Applied Payment key, trip-integrity, attempt and reconciliation checks and validated payment grain | Team | Done | `screenshots/week06_06_payment_dq_grain.png` |
| Reconciled Candidate, Trusted Silver and Quarantine counts for all four entities | Team | Done | `screenshots/week06_07_final_reconciliation.png` |
| Completed final Week 06 Data Quality validation | Team | Done | `screenshots/week06_08_final_validation.png` |
| Documented DQ failures, routing decisions and business impact | Team | Done | `docs/data_quality_summary.md` |

---

## 3. Key Decisions

- Used the four Silver Candidate datasets from Week 5 as the inputs for Week 6 Data Quality processing.
- Applied separate Data Quality rules for Zones, Drivers, Trips and Payments based on the business requirements of each dataset.
- Classified important rules using Critical and Major severity levels according to their potential impact on downstream analytics.
- Valid records were routed to Trusted Silver, while records failing applicable Data Quality rules were routed to Quarantine.
- Failed records were retained in Quarantine rather than being silently deleted.
- Trip validation covered key completeness and uniqueness, reference integrity, timestamp chronology, trip-status consistency, service and driver compatibility, distance validity, fare/surge validity, and declared-window/lineage checks.
- Payment validation covered payment-key and trip integrity together with payment-attempt and reconciliation logic.
- During payment validation, join multiplication was identified as a risk because multiple payment rows can relate to trip records. The validation logic was corrected to use distinct trip identifiers when checking payment-trip integrity.
- Candidate, Trusted Silver and Quarantine outputs were reconciled to verify that records were accounted for without unexplained loss or overlap.
- Gold metrics were not created during Week 6; only governed Trusted Silver data is intended to flow into later Gold processing.

---

## 4. Blockers / Risks

| Blocker / Risk | Impact | Resolution / Handling |
|---|---|---|
| Some Trip Candidate records failed multiple Data Quality rules | Could affect downstream operational KPIs if invalid records were included | Failed records were routed to Quarantine and excluded from Trusted Silver |
| Driver and Trip reference mismatches were present in the Candidate data | Invalid relationships could affect driver and trip analytics | Reference-integrity rules were applied and failed records were quarantined |
| Trip lifecycle records contained timestamp/status conditions requiring validation | Invalid chronology could produce incorrect response or duration metrics | Timestamp and lifecycle consistency rules were applied before Trusted Silver routing |
| Payment validation was initially affected by join multiplication | Could incorrectly increase payment failure/reconciliation counts | Corrected the validation logic to use distinct trip IDs for trip-existence checks |
| Multiple payment attempts exist for the same trip | Assuming one payment row per trip could incorrectly classify valid records | Payment grain and attempt-level logic were validated separately |

---

## 5. Evidence Added to GitHub

### Notebook

- `notebooks/04_data_quality_checks.ipynb`

### Documentation

- `docs/data_quality_summary.md`

### Screenshots

- `screenshots/week06_01_candidate_handoff.png` — Silver Candidate handoff into Week 6
- `screenshots/week06_02_zone_dq.png` — Zone Data Quality validation
- `screenshots/week06_03_driver_dq.png` — Driver Data Quality validation
- `screenshots/week06_04_trip_dq_results.png` — Trip Data Quality results and rule failures
- `screenshots/week06_05_trip_reconciliation.png` — Trip Candidate-to-Trusted/Quarantine reconciliation
- `screenshots/week06_06_payment_dq_grain.png` — Payment Data Quality and grain validation
- `screenshots/week06_07_final_reconciliation.png` — entity-level Candidate/Trusted/Quarantine reconciliation
- `screenshots/week06_08_final_validation.png` — final Week 6 validation

### Final Reconciliation

| Entity | Candidate | Trusted | Quarantine | Variance |
|---|---:|---:|---:|---:|
| Zones | 120 | 120 | 0 | 0 |
| Drivers | 2,800 | 2,793 | 7 | 0 |
| Trips | 250,875 | 241,654 | 9,221 | 0 |
| Payments | 180,315 | 177,635 | 2,680 | 0 |

All four entities reconciled with **zero variance**.

### Weekly Log

- `weekly_logs/week06_log.md`

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to explain Data Quality rule structure, severity classification, SQL validation logic, reference checks, quarantine routing, reconciliation and payment-grain issues. |
| What we changed after AI suggestion | The validation logic was reviewed and adapted to the actual TripPulse Silver Candidate schemas, entity relationships, payment-attempt grain and project-specific Data Quality requirements. |
| What we verified manually | Candidate inputs, DQ rule results, failed-record counts, Trusted Silver and Quarantine outputs, payment-grain validation, entity-level reconciliation and final validation results were executed and checked in Databricks. |
| What we can explain without AI | We can explain why Data Quality is applied after Silver Candidate transformation, how Critical and Major rules are used, why invalid records are quarantined, how Trusted Silver is formed, why payment grain matters, and how Candidate = Trusted + Quarantine reconciliation demonstrates controlled record handling. |

---

## 7. Next Week Preparation

- Use only governed Trusted Silver outputs as inputs for the Gold layer.
- Define Gold dimensions, facts, summary tables and KPI grains for TripPulse.
- Build business metrics for ride demand, completion, cancellation, driver operations, surge behaviour and payment reliability.
- Validate Gold aggregations against Trusted Silver records.
- Preserve the required lineage from Trusted Silver into Gold so dashboard metrics can be traced back to governed data.
