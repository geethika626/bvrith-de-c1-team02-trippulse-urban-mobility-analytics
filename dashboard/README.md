# TripPulse Power BI Dashboard

## Week 09 — Dashboard Refinement, Interactions and Insights

This folder contains the final TripPulse Power BI dashboard for Week 09.

The final dashboard contains the three approved Power BI pages:

- **PBI-01 • Ride Operations Overview**
- **PBI-02 • Zone Demand and Surge**
- **PBI-03 • Driver and Payment Reliability**

The dashboard uses validated Gold outputs and was reconciled against the
governed Gold tables for the validation period:

**01-Jan-2026 to 14-Jan-2026**

## Dashboard File

The final Power BI dashboard is included in this folder:

`dashboard/TripPulse_Dashboard_Final.pbix`

The repository also contains the supporting dashboard documentation,
screenshots, Gold validation evidence and reconciliation results.

---

# Power BI Source Rule

Power BI reporting uses validated Gold outputs only.

The dashboard does not directly use:

- Raw source files
- Bronze tables
- Silver Candidate tables
- Quarantine tables

The approved Gold tables and dimensions are used as the sources for the
Week 09 dashboard.

---

# Gold Sources

The Power BI model uses the following approved TripPulse Gold outputs:

| Gold Table | Purpose / Grain |
|---|---|
| `agg_trip_operations_daily` | Daily trip-operation summary |
| `agg_zone_demand_daily` | Daily zone-demand summary |
| `agg_surge_impact_daily` | Daily surge-impact summary |
| `agg_driver_performance_daily` | Daily driver-performance summary |
| `agg_payment_reliability_daily` | Daily payment-reliability summary |
| `dim_date` | Date dimension |
| `dim_driver` | Driver dimension |
| `dim_zone` | Zone dimension |

The Gold tables provide the governed metrics used by the dashboard KPI
cards and analytical visuals.

---

# Model and Relationships

The Power BI model uses the approved Gold summary tables together with the
required dimensions.

The model was reviewed in Power BI Model view to verify the relationships
between dimensions, fact-level data and Gold summary outputs.

The model preserves the intended grain of the Gold datasets and avoids
unsafe fact-to-fact relationships.

The main dimensions used by the dashboard are:

- `dim_date`
- `dim_driver`
- `dim_zone`

The main fact and summary outputs include:

- `fact_trip`
- `fact_payment_attempt`
- `agg_trip_operations_daily`
- `agg_zone_demand_daily`
- `agg_surge_impact_daily`
- `agg_driver_performance_daily`
- `agg_payment_reliability_daily`

---

# Dashboard Pages

## PBI-01 • Ride Operations Overview

### Purpose

Provides an overview of ride demand, completion, cancellation, unfulfilled
requests and driver response performance.

### Key Dashboard Components

- Total Trip Requests
- Completion Rate
- Cancellation Rate
- Unfulfilled Rate
- Average Driver Response Minutes
- Daily Requests vs Completions
- Trip Outcome by Service Type
- Top 10 Pickup Zones by Trip Requests
- Key Insight

### Filters

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

# PBI-01 Gold Reconciliation

PBI-01 was reconciled against:

`workspace.default.agg_trip_operations_daily`

## Validation Period

- Start date: `2026-01-01`
- End date: `2026-01-14`

## Reconciliation Results

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.12% | 64.12% | PASS |
| Cancellation Rate | 23.82% | 24% | PASS |

### Gold Validation

- Total Trip Requests = 37,892
- Completed Trips = 24,297
- Completion Rate = 64.1217%
- Cancelled Trips = 9,027
- Cancellation Rate = 23.8230%

The Power BI values reconcile with the same filtered Gold slice.
Cancellation Rate is displayed as 24% in Power BI because the Gold value
of 23.82% is rounded for display.

### Gold Calculations

- Total Trip Requests = `SUM(trip_requests)`
- Completion Rate = `SUM(completed_trips) / SUM(trip_requests)`
- Cancellation Rate = `SUM(cancelled_trips) / SUM(trip_requests)`

---

# PBI-02 • Zone Demand and Surge

### Purpose

Provides zone-level demand analysis, surge exposure and fulfilment
performance across pickup zones.

### Key Dashboard Components

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

### Filters

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

# PBI-02 Gold Reconciliation

PBI-02 was reconciled using:

- `workspace.default.agg_zone_demand_daily`
- `workspace.default.agg_surge_impact_daily`
- `workspace.default.dim_zone`
- `workspace.default.dim_date`

## Validation Period

- Start date: `2026-01-01`
- End date: `2026-01-14`

## Reconciliation Results

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |
| Surge Trip Share | 55.2808% | 55.28% | PASS |
| Average Final Fare | ₹446.6148 | ₹446.61 | PASS |

### Gold Calculations

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

### Purpose

Provides driver-performance and payment-attempt analysis to identify
operational reliability areas.

### Key Dashboard Components

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

### Filters

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

# PBI-03 Gold Reconciliation

PBI-03 was reconciled using:

- `workspace.default.agg_driver_performance_daily`
- `workspace.default.agg_payment_reliability_daily`
- `workspace.default.dim_driver`
- `workspace.default.dim_date`

## Validation Period

- Start date: `2026-01-01`
- End date: `2026-01-14`

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

For the common validation period of 01-Jan-2026 to 14-Jan-2026, PBI-01
and PBI-02 use the same governed trip-request, completion and cancellation
measures.

Observed values:

- Total Trip Requests = 37,892
- Completion Rate = 64.12%
- Cancellation Rate = approximately 24%

PBI-03 uses driver-performance and payment-attempt grains, so its
page-specific measures are interpreted separately from the trip-request
KPIs.

---

# Dashboard Reporting Rule

The final dashboard reports only from validated Gold outputs.

The dashboard does not use:

- Raw source files
- Bronze tables
- Silver Candidate tables
- Quarantine tables

This ensures that dashboard KPIs are based on governed downstream data.

---

# Dashboard Design

The three dashboard pages follow a consistent presentation structure:

1. TripPulse branding and page title
2. Decision question
3. Dashboard filters
4. KPI cards
5. Analytical visuals
6. Key insight

The dashboards were refined for consistent formatting, readability and
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

- Final Power BI dashboard
- Dashboard screenshots
- Gold export evidence
- KPI reconciliation evidence
- Filter validation evidence
- Dashboard documentation
- Week 09 validation results

### Dashboard File

`dashboard/TripPulse_Dashboard_Final.pbix`

### Screenshots

Stored under:

`screenshots/`

### Dashboard Documentation

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
- Power BI model relationship validation
- Cross-page consistency validation
- Evidence-backed insights and limitations
- PBI-01 KPI reconciliation
- PBI-02 KPI reconciliation
- PBI-03 driver slice reconciliation
- PBI-03 payment-method slice reconciliation
- Final dashboard refinement and presentation formatting

The final Power BI dashboard is included in this repository as:

`dashboard/TripPulse_Dashboard_Final.pbix`

Supporting screenshots, Gold export evidence, dashboard documentation and
validation results are also maintained in the repository.
