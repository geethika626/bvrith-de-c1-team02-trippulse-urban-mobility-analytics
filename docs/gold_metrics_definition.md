# Gold Metrics Definition

**Week:** 7  
**Purpose:** Define dashboard-ready Gold tables and KPI formulas.

---

## 1. Gold Table Catalog

| Gold Table Name | Grain | Source Table(s) | Purpose |
|---|---|---|---|
| `dim_date` | One row per calendar date | Generated calendar | Provides date attributes for Gold facts and aggregations |
| `dim_zone` | One row per zone | `silver_trippulse_zones_trusted` | Provides zone details for trip and demand analysis |
| `dim_driver` | One row per driver | `silver_trippulse_drivers_trusted` | Provides driver details for driver-performance analysis |
| `fact_trip` | One row per trip request | `silver_trippulse_trips_trusted` | Stores trusted trip-level measures and flags |
| `fact_payment_attempt` | One row per payment attempt | `silver_trippulse_payments_trusted` | Stores payment-attempt-level measures and status |
| `agg_trip_operations_daily` | One row per date × service type | `fact_trip` | Daily trip operations and completion/cancellation metrics |
| `agg_zone_demand_daily` | One row per date × pickup zone × service type | `fact_trip` | Daily demand and operational metrics by zone |
| `agg_driver_performance_daily` | One row per date × driver | `fact_trip` | Daily driver performance and reliability metrics |
| `agg_surge_impact_daily` | One row per date × pickup zone × service type × surge band | `fact_trip` | Measures the operational impact of surge |
| `agg_payment_reliability_daily` | One row per date × payment method | `fact_payment_attempt` | Daily payment success and reliability metrics |

---

## 2. KPI Definitions

| KPI Name | Formula | Grain | Dashboard Page | Notes |
|---|---|---|---|---|
| KPI-01 Total Trip Requests | `SUM(trip_count)` | Daily / Weekly | Trip Operations | Total number of trusted trip requests |
| KPI-02 Completion Rate | `SUM(completed_trips) / NULLIF(SUM(trip_requests), 0)` | Daily / Weekly | Trip Operations | Percentage of trip requests that were completed |
| KPI-03 Cancellation Rate | `SUM(cancelled_trips) / NULLIF(SUM(trip_requests), 0)` | Daily / Weekly | Trip Operations | Percentage of trip requests that were cancelled |
| KPI-04 Unfulfilled Rate | `SUM(unfulfilled_trips) / NULLIF(SUM(trip_requests), 0)` | Daily / Weekly | Trip Operations | Percentage of trip requests that were not fulfilled |
| KPI-05 Average Driver Response Minutes | `AVG(response_seconds) / 60.0` | Daily / Weekly | Trip Operations | Average driver response time in minutes |
| KPI-06 Average Trip Duration Minutes | `AVG(trip_duration_seconds) / 60.0` | Daily / Weekly | Trip Operations | Average trip duration in minutes |
| KPI-07 Average Final Fare INR | `AVG(final_fare_inr)` | Daily / Weekly | Trip Operations | Average final fare for completed trips with valid fare |
| KPI-08 Surge Trip Share | `SUM(surge_trip_count) / NULLIF(SUM(trip_requests), 0)` | Daily / Weekly / Zone | Surge Impact | Percentage of trip requests classified as surge trips |
| KPI-09 Driver Reliability Rate | `SUM(completed_trips) / NULLIF(SUM(assigned_requests), 0)` | Daily / Weekly / Driver | Driver Performance | Completed trips divided by assigned requests |
| KPI-10 Payment Attempt Success Rate | `SUM(successful_attempts) / NULLIF(SUM(payment_attempts), 0)` | Daily / Weekly / Payment Method | Payment Reliability | Percentage of payment attempts that were successful |

### Zero-Denominator Rule

If the denominator of a KPI is zero, the KPI returns `NULL` rather than `0%`.

This prevents a zero population from being interpreted as a valid 0% performance result.

---

## 3. Validation Checks

Before using Gold tables in Power BI, verify:

- Gold row counts are reasonable.
- No unexpected nulls exist in key dashboard fields.
- KPI totals match manual spot checks.
- Power BI connects to Gold outputs only.
- Metric definitions are documented clearly.
