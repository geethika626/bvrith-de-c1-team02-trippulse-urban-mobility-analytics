# Power BI Dashboard

The final Power BI dashboard was completed for Week 09.

The final PBIX contains the three approved dashboard pages:

- PBI-01 • Ride Operations Overview
- PBI-02 • Zone Demand and Surge
- PBI-03 • Driver and Payment Reliability

The dashboard uses validated Gold outputs only and was reconciled against
the governed Gold tables for the validation period 01-Jan-2026 to
14-Jan-2026.

## Power BI File-Size Rule

The final PBIX file is larger than 25 MB and is therefore not committed
to GitHub.

The final PBIX is stored separately for mentor review.

The repository contains the supporting dashboard screenshots, Gold export
evidence, dashboard documentation and validation results.

---

# Dashboard Pages

## PBI-01 • Ride Operations Overview

Purpose:

Provides an overview of ride demand, completion, cancellation,
unfulfilled requests and driver response performance.

Key dashboard components:

- Total Trip Requests
- Completion Rate
- Cancellation Rate
- Unfulfilled Rate
- Average Driver Response Minutes
- Daily Requests vs Completions
- Trip Outcome by Service Type
- Top 10 Pickup Zones by Trip Requests
- Key Insight

Filters:

- Date
- Service Type
- Pickup Zone Type

### Key Insight

Ride demand remains strong, but only 64.1% of requests are completed.
Cancellations and unfulfilled requests account for the remaining
operational losses, highlighting an opportunity to improve ride fulfilment.

### Filter Validation

PBI-01 filter behaviour was tested using:

- Date
- Service Type
- Pickup Zone Type

The filters correctly changed the corresponding dashboard visuals and KPI
values.

The dashboard was returned to the default filter state after testing.

---

# PBI-01 Measure Reconciliation

PBI-01 was reconciled against the governed Gold table:

`workspace.default.agg_trip_operations_daily`

## Validation Period

- Start date: 2026-01-01
- End date: 2026-01-14
- Gold source: `agg_trip_operations_daily`

## Reconciliation Results

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.12% | 64.12% | PASS |
| Cancellation Rate | 23.82% | 24% | PASS |

## Validation

The following Gold calculations were used:

- Total Trip Requests = `SUM(trip_requests)`
- Completion Rate = `SUM(completed_trips) / SUM(trip_requests)`
- Cancellation Rate = `SUM(cancelled_trips) / SUM(trip_requests)`

The Power BI values reconcile with the same filtered Gold slice for
01-Jan-2026 to 14-Jan-2026.

Cancellation Rate is displayed as 24% in Power BI because the Gold
value of 23.82% is rounded for display.

## Evidence

Databricks Gold validation confirmed:

- Total Trip Requests = 37,892
- Completed Trips = 24,297
- Completion Rate = 64.1217%
- Cancelled Trips = 9,027
- Cancellation Rate = 23.8230%

---

# PBI-02 • Zone Demand and Surge

Purpose:

Provides zone-level demand analysis, surge exposure and fulfilment
performance across pickup zones.

Key dashboard components:

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

### Key Insight

Demand is concentrated across a small group of pickup zones, while 55.3%
of requests occur under surge conditions. Monitoring high-demand zones can
help identify opportunities to improve fulfilment.

### Filter Validation

PBI-02 filter behaviour was tested using:

- Date
- Zone ID
- Zone Type
- Surge Band

The filters correctly changed the corresponding dashboard visuals and KPI
values.

The dashboard was returned to the default filter state after testing.

---

# PBI-02 Validation

PBI-02 was reconciled using the approved Gold tables:

- `workspace.default.agg_zone_demand_daily`
- `workspace.default.agg_surge_impact_daily`
- `workspace.default.dim_zone`
- `workspace.default.dim_date`

## Validation Period

- Start date: 2026-01-01
- End date: 2026-01-14

## Reconciliation Results

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |
| Surge Trip Share | 55.2808% | 55.28% | PASS |
| Average Final Fare | ₹446.6148 | ₹446.61 | PASS |

## Validation

The following Gold calculations were used:

- Total Trip Requests = `SUM(trip_requests)`
- Completion Rate = `SUM(completed_trips) / SUM(trip_requests)`
- Cancellation Rate = weighted calculation using `cancellation_rate`
  and `trip_requests`
- Surge Trip Share = `SUM(surge_trip_count) / SUM(trip_requests)`
- Average Final Fare = weighted calculation using `avg_final_fare_inr`,
  `trip_requests`, and `completion_rate`

All five selected PBI-02 KPI values reconcile with the corresponding Gold
calculations for 01-Jan-2026 to 14-Jan-2026.

---

# PBI-03 • Driver and Payment Reliability

Purpose:

Provides driver-performance and payment-attempt analysis to identify
operational reliability areas.

Key dashboard components:

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

### Key Insight

Driver reliability is 72.9%, while payment success is 56.1%. The results
highlight driver responsiveness and payment reliability as key areas for
operational attention.

### Filter Validation

PBI-03 filter behaviour was tested using:

- Date
- Driver ID
- Service Type
- Payment Method

All four filters were tested individually, and the dashboard was returned
to the default state after clearing the selections.

Driver metrics are fictional educational measures and should not be
interpreted as real worker evaluation.

Payment success is measured at the payment-attempt level.

---

# PBI-03 Validation

PBI-03 was reconciled using the approved Gold tables:

- `workspace.default.agg_driver_performance_daily`
- `workspace.default.agg_payment_reliability_daily`
- `workspace.default.dim_driver`
- `workspace.default.dim_date`

## Validation Period

- Start date: 2026-01-01
- End date: 2026-01-14

## Driver Slice Reconciliation

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

## Payment Method Slice Reconciliation

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

---

# Cross-Page Consistency

For the common date range 01-Jan-2026 to 14-Jan-2026, PBI-01 and PBI-02
use the same governed trip request, completion and cancellation measures.

Observed values:

- Total Trip Requests = 37,892
- Completion Rate = 64.12%
- Cancellation Rate = approximately 24%

PBI-03 uses driver-performance and payment-attempt grains, so its
page-specific measures are interpreted separately from the trip-request
KPIs.

---

# Dashboard Reporting Rule

Power BI reporting uses validated Gold outputs only.

The dashboard does not use:

- Raw source files
- Bronze tables
- Silver Candidate tables
- Quarantine tables

The approved Gold tables and dimensions are used for the Week 09
dashboard pages.

---

# Dashboard Design

The three dashboard pages follow a consistent presentation structure:

1. TripPulse branding and page title
2. Decision question
3. Dashboard filters
4. KPI cards
5. Analytical visuals
6. Key insight

The dashboards were redesigned for consistent formatting, readability and
presentation quality while keeping the validated Gold measures unchanged.

---

# Dashboard Key Insights

## PBI-01 • Ride Operations Overview

Ride demand remains strong, but only 64.1% of requests are completed.
Cancellations and unfulfilled requests account for the remaining
operational losses, highlighting an opportunity to improve ride fulfilment.

## PBI-02 • Zone Demand and Surge

Demand is concentrated across a small group of pickup zones, while 55.3%
of requests occur under surge conditions. Monitoring high-demand zones can
help identify opportunities to improve fulfilment.

## PBI-03 • Driver and Payment Reliability

Driver reliability is 72.9%, while payment success is 56.1%. The results
highlight driver responsiveness and payment reliability as key areas for
operational attention.

---

# Evidence and Supporting Files

Supporting dashboard evidence is maintained in the repository.

These include:

- Dashboard screenshots
- Gold export evidence
- KPI reconciliation evidence
- Filter validation evidence
- Dashboard documentation
- Week 09 validation results

Screenshots are stored under:

`screenshots/`

Dashboard documentation is maintained under:

`docs/dashboard_insights.md`

---

# Week 09 Dashboard Status

The Week 09 Power BI implementation and reconciliation activities are
complete.

Completed:

- Three approved dashboard pages
- Gold-only reporting
- Validated KPI cards and approved dashboard visuals
- Required filters
- Filter and interaction testing
- Cross-page consistency validation
- Evidence-backed insights and limitations
- PBI-01 KPI reconciliation
- PBI-02 KPI reconciliation
- PBI-03 driver slice reconciliation
- PBI-03 payment-method slice reconciliation
- Final dashboard redesign and presentation formatting

The final PBIX is stored separately because it exceeds the repository
file-size limit.

Supporting screenshots, Gold export evidence, dashboard documentation,
and validation results are maintained in the repository.
