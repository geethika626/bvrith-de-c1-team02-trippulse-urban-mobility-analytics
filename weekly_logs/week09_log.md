# Week 09 Log — Dashboard Completion and Insights

**Week:** 9  
**Date range:** 18th September 2026 - 25th September 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse — Urban Mobility Analytics

---

## 1. Sprint Goal

The goal of Week 09 was to complete and validate the three approved
TripPulse Power BI dashboard pages using governed Gold outputs only.

The work focused on finalizing the dashboard pages, validating the Power BI
model and filter interactions, reconciling selected dashboard KPIs and
slices against the corresponding Gold calculations, checking cross-page
consistency, and documenting evidence-backed insights and limitations.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Completed PBI-01 — Ride Operations Overview | Team | Done | `screenshots/week09_01_ride_operations_overview.png` |
| Completed PBI-02 — Zone Demand and Surge | Team | Done | `screenshots/week09_02_zone_demand_surge.png` |
| Completed PBI-03 — Driver and Payment Reliability | Team | Done | `screenshots/week09_03_driver_payment_reliability.png` |
| Tested dashboard filters and interactions | Team | Done | `screenshots/week09_04_filter_validation.png` |
| Reconciled PBI-02 trip requests, completion rate and cancellation rate against Gold | Team | Done | `screenshots/week09_05_pbi02_trip_requests_completion_cancellation.png` |
| Reconciled PBI-02 surge trip share and average final fare against Gold | Team | Done | `screenshots/week09_06_pbi02_surge_fare_reconciliation.png` |
| Reconciled PBI-03 driver metrics against Gold | Team | Done | `screenshots/week09_07_pbi03_driver_reconciliation.png` |
| Reconciled PBI-03 payment-method metrics against Gold | Team | Done | `screenshots/week09_09_pbi03_payment_reconciliation.png` |
| Completed KPI cards and approved dashboard visuals | Team | Done | `dashboard/powerbi_dashboard.pbix` |
| Completed required dashboard filters and slicers | Team | Done | `screenshots/week09_04_filter_validation.png` |
| Validated the final Power BI model | Team | Done | `dashboard/powerbi_dashboard.pbix` |
| Documented dashboard insights and limitations | Team | Done | `docs/dashboard_insights.md` |
| Updated dashboard README | Team | Done | `dashboard/README.md` |
| Maintained the final Power BI dashboard in the repository | Team | Done | `dashboard/powerbi_dashboard.pbix` |
| Reviewed dashboard readability and accessibility across all three pages | Team | Done | Final Power BI dashboard |


---

## 3. Key Decisions

- Used only validated Gold outputs and approved Gold dimensions for Power BI
  reporting.
- Completed exactly three approved dashboard pages:
  - PBI-01 — Ride Operations Overview
  - PBI-02 — Zone Demand and Surge
  - PBI-03 — Driver and Payment Reliability
- Used the common validation period of **01-Jan-2026 to 14-Jan-2026** for
  Gold-to-Power BI reconciliation.
- Kept trip-request, driver-performance and payment-attempt measures at
  their appropriate business grains.
- PBI-02 surge trip share was calculated using the Gold
  `surge_trip_count` and `trip_requests` values rather than directly
  summing percentage values.
- Used Gold reconciliation queries to validate selected dashboard KPI
  values and filtered slices.
- Tested the required dashboard filters and interactions and returned the
  dashboard to its default state after testing.
- Kept raw, Bronze, Silver Candidate and Quarantine data outside the
  Power BI reporting layer.
- Treated driver-performance metrics as fictional educational measures and
  not as real worker evaluations.
- Treated surge and fulfilment patterns as observations from synthetic
  educational data rather than causal real-world conclusions.
- Maintained the final PBIX inside the repository under the `dashboard/`
  directory.

---

## 4. Blockers / Risks

| Blocker / Risk | Impact | Resolution / Handling |
|---|---|---|
| Different dashboard pages use different business grains | Direct comparison of all KPIs could produce misleading interpretations | Kept trip-request, driver-performance and payment-attempt measures at their appropriate grains |
| Power BI displays some percentage and currency values using rounded formatting | Displayed values can differ slightly from full Gold precision | Reconciled against the underlying Gold calculations and accepted normal display rounding |
| Payment metrics are measured at payment-attempt level | Payment success should not be interpreted as an overall customer payment rate | Documented the payment-attempt grain in dashboard documentation |
| Driver metrics are fictional educational measures | Values should not be interpreted as real worker evaluations | Documented the limitation in dashboard documentation |
| Synthetic data limits real-world interpretation | Dashboard insights should not be treated as production business conclusions | Documented synthetic-data limitations in `docs/dashboard_insights.md` |

---

## 5. Evidence Added to GitHub

### Power BI Dashboard

- `dashboard/powerbi_dashboard.pbix`
- `dashboard/README.md`

### Week 09 Screenshots

- `screenshots/week09_01_ride_operations_overview.png`
- `screenshots/week09_02_zone_demand_surge.png`
- `screenshots/week09_03_driver_payment_reliability.png`
- `screenshots/week09_04_filter_validation.png`
- `screenshots/week09_05_pbi02_trip_requests_completion_cancellation.png`
- `screenshots/week09_06_pbi02_surge_fare_reconciliation.png`
- `screenshots/week09_07_pbi03_driver_reconciliation.png`
- `screenshots/week09_09_pbi03_payment_reconciliation.png`

### Dashboard Documentation

- `docs/dashboard_insights.md`

### Power BI Export Notebook

- `notebooks/06_powerbi_export.ipynb`

### Weekly Log

- `weekly_logs/week09_log.md`

### Gold Reconciliation Evidence

#### PBI-01

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.12% | 64.12% | PASS |
| Cancellation Rate | 23.82% | 24% | PASS |

#### PBI-02

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |
| Surge Trip Share | 55.2808% | 55.28% | PASS |
| Average Final Fare | ₹446.6148 | ₹446.61 | PASS |

#### PBI-03 Driver Slice

Driver slice used:

`DRV-000008`

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Driver Reliability Rate | 88.8889% | 88.89% | PASS |
| Average Driver Response Minutes | 3.7630 | 3.76 | PASS |
| Average Trip Duration | 35.8188 minutes | 35.82 minutes | PASS |

#### PBI-03 Payment Method Slice

Payment method slice used:

`UPI`

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Payment Attempt Success Rate | 36.2996% | 36.30% | PASS |
| Average Attempts per Trip | 1.4051 | 1.41 | PASS |

All selected reconciliation values matched the corresponding Gold
calculations after normal Power BI display rounding.

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to assist with Power BI dashboard structure, measure formulation, filter and interaction testing, Gold reconciliation-query preparation, troubleshooting and documentation. |
| What we changed after AI suggestion | The team reviewed and adapted the suggestions to the actual TripPulse Gold schemas, dashboard requirements, business grains and observed Power BI behaviour. |
| What we verified manually | The final Power BI model, dashboard pages, filters, slicers, KPI values, Gold query results, payment-method slice, driver slice, cross-page consistency and dashboard documentation were manually checked. |
| What we can explain without AI | We can explain the purpose of each dashboard page, the Gold sources behind the KPIs, the business grain of each metric, how filters affect the dashboard, how Gold-to-Power BI reconciliation works, and the limitations of the synthetic dataset. |

---

## 7. Next Week Preparation

- Prepare for the controlled Week 10 streaming simulation.
- Review the approved Streaming Event Design before implementing
  streaming-related work.
- Keep streaming/event processing separate from the completed Week 09
  batch Gold and Power BI model.
- Prepare the required environment, notebooks and evidence structure for
  the controlled streaming simulation.
- Preserve the validated Week 09 Power BI dashboard as the baseline for the
  next project stage.
