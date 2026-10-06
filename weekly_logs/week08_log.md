# Week 08 Log — Gold Hand-off and Power BI Foundation

**Week:** 8  
**Date range:** 28 August 2026 – 03 September 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Sprint Goal

The goal of Week 08 was to prepare the governed TripPulse Gold outputs for
Power BI consumption and establish the initial Power BI dashboard foundation.

The work focused on validating Gold schemas, row counts, deterministic date
slices, table grains and exported Gold data, reading the exported data back
for validation, confirming the final Gold hand-off, establishing the
Power BI model using governed Gold outputs, implementing PBI-01 — Ride
Operations Overview, and reconciling selected dashboard measures against
the Gold layer.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Validated the approved Gold table schemas and required fields | Team | Done | `screenshots/week08_01_gold_schema_01.png` |
| Validated Gold table row counts | Team | Done | `screenshots/week08_02_gold_counts.png` |
| Validated the deterministic 01-Jan-2026 to 14-Jan-2026 date slice | Team | Done | `screenshots/week08_03_date_slice_validation.png` |
| Validated exported Gold schemas and required columns | Team | Done | `screenshots/week08_04_export_schema_validation.png` |
| Validated Gold table grains and key structure | Team | Done | `screenshots/week08_05_grain_validation.png` |
| Created deterministic Gold export files for Power BI hand-off | Team | Done | `screenshots/week08_06_gold_exports.png` |
| Read back the exported Gold data and validated the exported outputs | Team | Done | `screenshots/week08_07_export_readback.png` |
| Completed final Gold hand-off validation | Team | Done | `screenshots/week08_08_final_validation.png` |
| Built the Power BI model using governed Gold outputs | Team | Done | `screenshots/week08_09_powerbi_model.png` |
| Implemented PBI-01 — Ride Operations Overview | Team | Done | `screenshots/week08_10_pbi01_page.png` |
| Reconciled PBI-01 KPI values against Gold | Team | Done | `screenshots/week08_11_pbi01_reconciliation.png` |
| Reconciled the cancellation-rate calculation against Gold | Team | Done | `screenshots/week08_12_cancellation_reconciliation.png` |
| Prepared the final Power BI dashboard file | Team | Done | `dashboard/TripPulse_Dashboard_Final.pbix` |
| Updated Power BI dashboard documentation | Team | Done | `dashboard/README.md` |

---

## 3. Key Decisions

- Used only governed Gold outputs as the source boundary for Power BI.
- Prepared deterministic Gold exports for the approved validation period
  of **01-Jan-2026 to 14-Jan-2026**.
- Validated Gold schemas, required columns, row counts and declared grains
  before using the outputs for Power BI.
- Read back the exported Gold data to confirm that the hand-off files
  contained the expected data.
- Kept streaming/event data outside the Week 08 batch Power BI scope.
- Established the Power BI model using the approved Gold outputs and
  required dimensions.
- Implemented the first approved dashboard page:
  **PBI-01 — Ride Operations Overview**.
- Used the approved PBI-01 decision question, KPI cards, visuals and
  filters.
- Reconciled PBI-01 dashboard values against the corresponding Gold
  calculations for the same validation period.
- Verified the cancellation-rate calculation separately against the Gold
  output.
- Kept Silver Candidate, Trusted Silver and Quarantine tables outside the
  Power BI reporting layer.
- Preserved the Gold-to-Power BI traceability so that dashboard KPIs could
  be validated against their owning Gold outputs.

---

## 4. Blockers / Risks

| Blocker / Risk | Impact | Resolution / Handling |
|---|---|---|
| Gold outputs must retain their declared business grain before Power BI consumption | Incorrect grain could lead to incorrect dashboard aggregations | Validated Gold table grains and key structure before the Power BI hand-off |
| Power BI KPI values must match the governed Gold calculations | Incorrect measure definitions could produce misleading dashboard results | Reconciled PBI-01 KPI values against Gold outputs for the approved validation period |
| Exported Gold files need to preserve the expected schema and records | Incorrect exports could break the Power BI hand-off | Validated export schema and read back the exported data |
| Date filtering must use a deterministic validation period | Different date ranges could produce inconsistent reconciliation results | Used the controlled period from 01-Jan-2026 to 14-Jan-2026 |
| Power BI should not consume lower-layer data directly | Raw, Bronze or Silver data could bypass the governed Gold layer | Maintained Gold-only reporting for the Power BI dashboard |

---

## 5. Evidence Added to GitHub

### Power BI Dashboard

- `dashboard/TripPulse_Dashboard_Final.pbix`
- `dashboard/README.md`

### Week 08 Screenshots

- `screenshots/week08_01_gold_schema_01.png`
- `screenshots/week08_02_gold_counts.png`
- `screenshots/week08_03_date_slice_validation.png`
- `screenshots/week08_04_export_schema_validation.png`
- `screenshots/week08_05_grain_validation.png`
- `screenshots/week08_06_gold_exports.png`
- `screenshots/week08_07_export_readback.png`
- `screenshots/week08_08_final_validation.png`
- `screenshots/week08_09_powerbi_model.png`
- `screenshots/week08_10_pbi01_page.png`
- `screenshots/week08_11_pbi01_reconciliation.png`
- `screenshots/week08_12_cancellation_reconciliation.png`

### Dashboard Documentation

- `dashboard/README.md`
- `docs/dashboard_insights.md`

### Weekly Log

- `weekly_logs/week08_log.md`

### PBI-01 Reconciliation

The Week 08 PBI-01 validation used the governed Gold output for the
01-Jan-2026 to 14-Jan-2026 validation period.

Key validated values included:

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.12% | 64.12% | PASS |
| Cancellation Rate | 23.82% | 24% | PASS |

The cancellation rate is displayed as 24% in Power BI because the
underlying Gold value is 23.82% and Power BI rounds the displayed value.

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to assist with Gold-to-Power BI hand-off structure, schema and grain validation approaches, Power BI source mapping, dashboard organization, reconciliation logic and documentation. |
| What we changed after AI suggestion | The team reviewed and adapted the suggestions to the actual TripPulse Gold outputs, validation period, Power BI model, PBI-01 requirements and project evidence. |
| What we verified manually | Gold schemas, row counts, date slice, export schemas, table grains, exported data readback, final Gold validation, Power BI model, PBI-01 dashboard values and Gold reconciliation results were manually checked. |
| What we can explain without AI | We can explain why Power BI uses governed Gold outputs, why Gold grain must be validated before dashboard use, how deterministic exports support reproducibility, how the PBI-01 KPIs are calculated, and how the dashboard values are reconciled against Gold. |

---

## 7. Next Week Preparation

- Complete PBI-02 — Zone Demand and Surge.
- Complete PBI-03 — Driver and Payment Reliability.
- Reconcile the important KPI values and selected slices for the remaining
  dashboard pages.
- Test the required Power BI filters and interactions.
- Document dashboard insights, limitations and Gold traceability.
- Maintain the Gold-only Power BI source boundary while refining the final
  dashboard.
- Preserve the validated Gold calculations and model relationships during
  Week 09 dashboard refinement.
