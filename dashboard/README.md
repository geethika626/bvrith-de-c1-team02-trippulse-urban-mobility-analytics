# Power BI Dashboard Folder

Save the final Power BI file here.

Expected file:

```text
dashboard/powerbi_dashboard.pbix
```

Rules:

- Power BI must connect to Gold outputs only.
- Do not connect dashboard visuals directly to raw source files.
- Save dashboard screenshots in `screenshots/`.
- Explain dashboard insights in `docs/dashboard_insights.md`.

## Power BI File-Size Rule

## PBIX Storage

The final Power BI dashboard file is larger than 25 MB and is therefore not uploaded to GitHub.

The final PBIX contains the completed Week 09 Power BI dashboard with all three approved pages:

- PBI-01 • Ride Operations Overview
- PBI-02 • Zone Demand and Surge
- PBI-03 • Driver and Payment Reliability

The final PBIX is stored separately for mentor review.

The repository contains the supporting screenshots, Gold export evidence, dashboard documentation and validation results.

## PBI-01 Measure Reconciliation

PBI-01 was reconciled against the governed Gold table
`workspace.default.agg_trip_operations_daily`.

### Validation Period

- Start date: 2026-01-01
- End date: 2026-01-14
- Gold source: `agg_trip_operations_daily`

### Reconciliation Results

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.12% | 64.12% | PASS |
| Cancellation Rate | 23.82% | 24% | PASS |

### Validation

The following Gold calculations were used:

- Total Trip Requests = `SUM(trip_requests)`
- Completion Rate = `SUM(completed_trips) / SUM(trip_requests)`
- Cancellation Rate = `SUM(cancelled_trips) / SUM(trip_requests)`

The Power BI values reconcile with the same filtered Gold slice for
01-Jan-2026 to 14-Jan-2026.

Cancellation Rate is displayed as 24% in Power BI because the Gold
value of 23.82% is rounded for display.

### Evidence

Databricks Gold validation confirmed:

- Total Trip Requests = 37,892
- Completed Trips = 24,297
- Completion Rate = 64.1217%
- Cancelled Trips = 9,027
- Cancellation Rate = 23.8230%

## PBI-02 Validation

PBI-02 was reconciled using the approved Gold tables:

- `workspace.default.agg_zone_demand_daily`
- `workspace.default.agg_surge_impact_daily`
- `workspace.default.dim_zone`
- `workspace.default.dim_date`

### Validation Period

- Start date: 2026-01-01
- End date: 2026-01-14

### Reconciliation Results

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |
| Surge Trip Share | 55.2808% | 55.28% | PASS |
| Average Final Fare | ₹446.6148 | ₹446.61 | PASS |

### Validation

The following Gold calculations were used:

- Total Trip Requests = `SUM(trip_requests)`
- Completion Rate = `SUM(completed_trips) / SUM(trip_requests)`
- Cancellation Rate = weighted calculation using `cancellation_rate` and `trip_requests`
- Surge Trip Share = `SUM(surge_trip_count) / SUM(trip_requests)`
- Average Final Fare = weighted calculation using `avg_final_fare_inr`, `trip_requests`, and `completion_rate`

All five selected PBI-02 KPI values reconcile with the corresponding
Gold calculations for 01-Jan-2026 to 14-Jan-2026.

### Filter Validation

PBI-02 filter behaviour was tested using:

- Date
- Zone ID
- Zone Type
- Service Type
- Surge Band

The filters correctly changed the corresponding dashboard visuals and KPI values.

The dashboard was returned to the default filter state after testing.

## PBI-03 Validation

PBI-03 was reconciled using the approved Gold tables:

- `workspace.default.agg_driver_performance_daily`
- `workspace.default.agg_payment_reliability_daily`
- `workspace.default.dim_driver`
- `workspace.default.dim_date`

### Validation Period

- Start date: 2026-01-01
- End date: 2026-01-14

### Driver Slice Reconciliation

Driver slice used:

`DRV-000008`

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Driver Reliability Rate | 88.8889% | 88.89% | PASS |
| Average Driver Response Minutes | 3.7630 | 3.76 | PASS |
| Average Trip Duration | 35.8188 minutes | 35.82 minutes | PASS |

The selected driver slice was reconciled against
`workspace.default.agg_driver_performance_daily` for
01-Jan-2026 to 14-Jan-2026.

### Payment Method Slice Reconciliation

Payment method slice used:

`UPI`

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Payment Attempt Success Rate | 36.2996% | 36.30% | PASS |
| Average Attempts per Trip | 1.4051 | 1.41 | PASS |

The selected payment-method slice was reconciled against
`workspace.default.agg_payment_reliability_daily` for
01-Jan-2026 to 14-Jan-2026.

Average Attempts per Trip was reconciled using the same Power BI
calculation:

`AVERAGE(avg_attempts_per_trip)`

The Gold result and Power BI value agree after rounding.

### Filter Validation

PBI-03 filter behaviour was tested using:

- Date
- Driver ID
- Service Type
- Payment Method

All four filters were tested individually, and the dashboard returned to
the default state after clearing the selections.

Driver metrics are fictional educational measures and should not be
interpreted as real worker evaluation. Payment success is measured at
the payment-attempt level.

## Cross-Page Consistency

For the common date range 01-Jan-2026 to 14-Jan-2026, PBI-01 and PBI-02
use the same governed trip request, completion, and cancellation
measures.

Observed values:

- Total Trip Requests = 37,892
- Completion Rate = 64.12%
- Cancellation Rate = approximately 24%

PBI-03 uses driver-performance and payment-attempt grains, so its
page-specific measures are interpreted separately from the trip-request
KPIs.

## Dashboard Reporting Rule

Power BI reporting uses validated Gold outputs only.

The dashboard does not use:

- Raw source files
- Bronze tables
- Silver Candidate tables
- Quarantine tables

The approved Gold tables and dimensions are used for the Week 09
dashboard pages.

## Week 09 Dashboard Status

The Week 09 Power BI implementation and reconciliation activities are
complete.

Completed:

- Three approved dashboard pages
- Gold-only reporting
- Approved KPI cards and visuals
- Required filters
- Filter and interaction testing
- Cross-page consistency validation
- Evidence-backed insights and limitations
- PBI-01 KPI reconciliation
- PBI-02 KPI reconciliation
- PBI-03 driver slice reconciliation
- PBI-03 payment-method slice reconciliation

The final PBIX is stored separately because it exceeds the repository
file-size limit.

Supporting screenshots, Gold export evidence, dashboard documentation,
and validation results are maintained in the repository.
