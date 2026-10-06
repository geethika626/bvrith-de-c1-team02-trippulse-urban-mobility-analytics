# Dashboard Insights

**Week:** 9  
**Project:** TripPulse - Urban Mobility Analytics  
**Purpose:** Explain the evidence-backed observations from the Power BI dashboard and document Gold-table traceability, validation, reconciliation, and limitations.

---

## 1. Dashboard Pages

| Page | Purpose | Main Visuals |
|---|---|---|
| PBI-01: Ride Operations Overview | Understand ride fulfilment efficiency and operational losses | KPI cards, daily requests vs completions, trip outcome by service type, top pickup zones |
| PBI-02: Zone Demand and Surge | Understand concentrated zone demand, surge exposure and fulfilment | KPI cards, zone demand matrix, surge-band comparison, top pickup zones, service-type response-time comparison |
| PBI-03: Driver and Payment Reliability | Understand driver assignment outcomes and payment-attempt reliability | KPI cards, driver reliability vs assigned volume, payment success trend, driver cancellation counts, payment status/retry distribution |

**Note:** Streaming event reporting is not included in Week 09. Streaming simulation is planned for Week 10.

---

## 2. Key Insights

### PBI-01 — Ride Operations Overview

**Key Insight:**  
Requests remain consistently high across the selected period, while approximately 64% of requests are completed. The remaining requests are represented primarily by cancellations and unfulfilled requests, highlighting an opportunity to improve ride fulfilment.

**Observed KPI values for 01 Jan 2026–14 Jan 2026:**

- Total Trip Requests: 37,892
- Completion Rate: 64.12%
- Cancellation Rate: 23.82%
- Unfulfilled Rate: 12.06%
- Average Driver Response: 3.67 minutes

**Limitation:**  
The dashboard is based on the selected Gold slice and does not include streaming event data.

---

### PBI-02 — Zone Demand and Surge

**Key Insight:**  
Requests remain concentrated across a small group of pickup zones, with surge trips accounting for approximately 55% of requests in the selected Gold slice. With overall completion at approximately 64%, high-demand zones should be monitored for fulfilment performance.

**Observed KPI values for 01 Jan 2026–14 Jan 2026:**

- Total Trip Requests: 37,892
- Completion Rate: 64.12%
- Cancellation Rate: approximately 24%
- Surge Trip Share: 55.28%
- Average Final Fare: ₹446.61

**Limitation:**  
The dashboard is based on the selected Gold slice and does not include streaming event data. Surge and fulfilment patterns in this synthetic dataset should not be interpreted as causal relationships.

---

### PBI-03 — Driver and Payment Reliability

**Key Insight:**  
Driver reliability is approximately 72.9%, with an average response time of about 3.7 minutes. Payment attempt success is approximately 56.1%, with an average of 1.16 attempts per trip. These measures identify driver responsiveness and payment reliability as areas for operational attention within the synthetic dataset.

**Observed KPI values for 01 Jan 2026–14 Jan 2026:**

- Driver Reliability Rate: 72.91%
- Average Driver Response: 3.67 minutes
- Average Trip Duration: 47.44 minutes
- Payment Attempt Success Rate: 56.11%
- Average Attempts per Trip: 1.16

**Limitation:**  
Driver metrics are fictional educational measures and should not be interpreted as real worker evaluation. Payment success is measured at the payment-attempt level.

---

## 3. Dashboard KPI Summary

The following KPI values are displayed in the final Power BI dashboard for the selected date range **01 Jan 2026–14 Jan 2026**.

| Dashboard Page | KPI | Displayed Value |
|---|---|---:|
| PBI-01 | Total Trip Requests | 37.9K |
| PBI-01 | Completion Rate | 64.1% |
| PBI-01 | Cancellation Rate | 23.8% |
| PBI-01 | Unfulfilled Rate | 12.1% |
| PBI-01 | Average Driver Response Minutes | 3.67 |
| PBI-02 | Total Trip Requests | 37.9K |
| PBI-02 | Completion Rate | 64.1% |
| PBI-02 | Cancellation Rate | 23.8% |
| PBI-02 | Surge Trip Share | 55.28% |
| PBI-02 | Average Final Fare | ₹446.61 |
| PBI-03 | Driver Reliability Rate | 72.91% |
| PBI-03 | Average Driver Response Minutes | 3.67 |
| PBI-03 | Average Trip Duration | 47.44 |
| PBI-03 | Payment Attempt Success Rate | 56.11% |
| PBI-03 | Average Attempts per Trip | 1.16 |

---

## 4. How the Dashboard Uses Gold Tables

| Dashboard Page | Gold Table Used | Important Fields |
|---|---|---|
| PBI-01 | `agg_trip_operations_daily` | `operation_date`, `trip_requests`, `completed_trips`, `cancelled_trips`, `unfulfilled_trips`, `avg_response_minutes` |
| PBI-01 | `agg_zone_demand_daily` | `operation_date`, `pickup_zone_id`, `service_type`, `trip_requests` |
| PBI-01 | `dim_date`, `dim_zone` | Date and pickup-zone filtering |
| PBI-02 | `agg_zone_demand_daily` | `operation_date`, `pickup_zone_id`, `service_type`, `trip_requests`, `avg_response_minutes` |
| PBI-02 | `agg_surge_impact_daily` | `operation_date`, `surge_band`, `trip_requests`, `surge_trip_count`, `completion_rate`, `avg_final_fare_inr` |
| PBI-02 | `dim_date`, `dim_zone` | Date, zone and zone-type filtering |
| PBI-03 | `agg_driver_performance_daily` | `operation_date`, `driver_id`, `assigned_requests`, `completed_trips`, `avg_response_minutes`, `avg_trip_duration_minutes`, `driver_cancellations` |
| PBI-03 | `agg_payment_reliability_daily` | `payment_date`, `payment_method`, `payment_attempts`, `successful_attempts`, `failed_attempts`, `avg_attempts_per_trip` |
| PBI-03 | `dim_driver`, `dim_date` | Driver and date filtering |

**Reporting rule:** Power BI reporting uses validated Gold outputs only. Raw/source, Bronze, Silver Candidate, quarantine, and other non-approved layers are not used as reporting sources.

---

## 5. Power BI Validation and Reconciliation

- [x] Dashboard connects to approved Gold outputs only.
- [x] PBI-01, PBI-02 and PBI-03 filters were tested.
- [x] Driver ID filter on PBI-03 was tested.
- [x] Service Type filter on PBI-03 was tested.
- [x] Payment Method filter on PBI-03 was tested.
- [x] Date filter on PBI-03 was tested.
- [x] Cleared/default filter state was verified.
- [x] PBI-01 and PBI-02 shared KPI values were checked under the same date context.
- [x] PBI-01 selected KPI values were reconciled with matching Gold queries.
- [x] PBI-02 selected KPI values were reconciled with matching Gold queries.
- [x] PBI-03 driver slice was reconciled with matching Gold queries.
- [x] PBI-03 payment-method slice was reconciled with matching Gold queries.
- [x] Dashboard contains exactly three approved Week 09 pages.
- [x] Insight and limitation text is included on all three pages.
- [x] Final dashboard screenshots are prepared for the `screenshots/` folder.
- [x] Final PBIX contains all three approved Week 09 pages and is stored separately because of the file-size limit.
- [x] Final Week 09 log is updated in `weekly_logs/week09_log.md`.

---

## 6. Reconciliation Evidence

### 6.1 PBI-01 — Ride Operations Overview

Selected date range: **01 Jan 2026–14 Jan 2026**

| KPI | Gold | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |

---

### 6.2 PBI-02 — Zone Demand and Surge

Selected date range: **01 Jan 2026–14 Jan 2026**

| KPI | Gold | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |
| Surge Trip Share | 55.2808% | 55.28% | PASS |
| Average Final Fare | ₹446.6148 | ₹446.61 | PASS |

---

### 6.3 PBI-03 — Driver Slice

Selected date range: **01 Jan 2026–14 Jan 2026**  
Driver: **DRV-000008**

| KPI | Gold | Power BI | Status |
|---|---:|---:|---|
| Driver Reliability Rate | 88.8889% | 88.89% | PASS |
| Average Driver Response Minutes | 3.7630 | 3.76 | PASS |
| Average Trip Duration | 35.8188 minutes | 35.82 | PASS |

---

### 6.4 PBI-03 — Payment Method Slice

Selected date range: **01 Jan 2026–14 Jan 2026**  
Payment Method: **UPI**

| KPI | Gold | Power BI | Status |
|---|---:|---:|---|
| Payment Attempt Success Rate | 36.2996% | 36.30% | PASS |
| Average Attempts per Trip | 1.4051 | 1.41 | PASS |

**Reconciliation note:**  
Average Attempts per Trip was reconciled using the same approved Power BI calculation, `AVERAGE(avg_attempts_per_trip)`, so the Gold result and Power BI result agree after rounding.

---

## 7. Cross-Page Consistency

For the common date range **01 Jan 2026–14 Jan 2026**, PBI-01 and PBI-02 use the same governed trip-request, completion and cancellation measures.

Observed values:

- Total Trip Requests: **37,892**
- Completion Rate: **64.12%**
- Cancellation Rate: **approximately 24%**

PBI-03 uses driver-performance and payment-attempt grains, so its page-specific measures are interpreted separately from the trip-request KPIs.

---

## 8. Gold-to-Dashboard Traceability

The dashboard follows the traceability chain:

**Power BI Visual → Measure / Field → Owning Gold Table → Gold Validation**

### 8.1 PBI-01 — Ride Operations Overview

| Dashboard KPI / Visual | Gold Table | Gold Field / Measure |
|---|---|---|
| Total Trip Requests | `agg_trip_operations_daily` | `trip_requests` |
| Completion Rate | `agg_trip_operations_daily` | `completed_trips` / `trip_requests` |
| Cancellation Rate | `agg_trip_operations_daily` | `cancelled_trips` / `trip_requests` |
| Unfulfilled Rate | `agg_trip_operations_daily` | `unfulfilled_trips` / `trip_requests` |
| Average Driver Response Minutes | `agg_trip_operations_daily` | `avg_response_minutes` |
| Daily Requests vs Completions | `agg_trip_operations_daily` | `trip_requests`, `completed_trips` |
| Trip Outcome by Service Type | Approved Gold trip/service output | Service type and trip outcome measures |
| Top Pickup Zones by Trip Requests | `agg_zone_demand_daily` | `pickup_zone_id`, `trip_requests` |

---

### 8.2 PBI-02 — Zone Demand and Surge

| Dashboard KPI / Visual | Gold Table | Gold Field / Measure |
|---|---|---|
| Total Trip Requests | `agg_zone_demand_daily` | `trip_requests` |
| Completion Rate | Approved Gold trip outcome output | Completion measures |
| Cancellation Rate | Approved Gold trip outcome output | Cancellation measures |
| Surge Trip Share | `agg_surge_impact_daily` | `surge_trip_count`, `trip_requests` |
| Average Final Fare | `agg_surge_impact_daily` | `avg_final_fare_inr` |
| Zone Demand by Date | `agg_zone_demand_daily` | `operation_date`, `pickup_zone_id`, `trip_requests` |
| Surge Band Requests vs Completion | `agg_surge_impact_daily` | `surge_band`, `trip_requests`, completion measures |
| Top Pickup Zones by Demand | `agg_zone_demand_daily` | `pickup_zone_id`, `trip_requests` |
| Service-Type Response-Time Comparison | Approved Gold response-time output | `service_type`, response-time measure |

---

### 8.3 PBI-03 — Driver and Payment Reliability

| Dashboard KPI / Visual | Gold Table | Gold Field / Measure |
|---|---|---|
| Driver Reliability Rate | `agg_driver_performance_daily` | Driver reliability measure |
| Average Driver Response Minutes | `agg_driver_performance_daily` | `avg_response_minutes` |
| Average Trip Duration | `agg_driver_performance_daily` | `avg_trip_duration_minutes` |
| Payment Attempt Success Rate | `agg_payment_reliability_daily` | `successful_attempts`, `payment_attempts` |
| Average Attempts per Trip | `agg_payment_reliability_daily` | `avg_attempts_per_trip` |
| Driver Reliability vs Assigned Volume | `agg_driver_performance_daily` | `assigned_requests`, driver reliability measure |
| Payment Success Trend by Method | `agg_payment_reliability_daily` | `payment_method`, payment success measures |
| Driver Cancellation Counts | `agg_driver_performance_daily` | `driver_cancellations` |
| Payment Status and Retry Distribution | `agg_payment_reliability_daily` | Payment status and retry measures |

---

## 9. Synthetic Data Limitation

TripPulse is an educational synthetic-data project. Dashboard observations describe patterns present in the generated Gold data and should not be interpreted as real-world operational, driver-performance, revenue or payment-business conclusions.

Driver-related metrics are fictional educational measures and should not be interpreted as evaluations of real workers.

Payment success is measured at the payment-attempt level and should not be interpreted as a direct measure of overall customer payment behaviour.

Surge and fulfilment patterns in the synthetic dataset show observed associations only and do not establish causal relationships.

---

## 10. Dashboard Limitations

- The dashboard uses the selected Gold outputs for reporting.
- No raw, Bronze or Silver tables are used as direct Power BI reporting sources.
- The dashboard contains three approved Week 09 pages.
- Streaming or live-event reporting is not included in Week 09 and belongs to the Week 10 streaming simulation.
- KPI values depend on the selected date and filter context.
- PBI-03 driver and payment metrics use different analytical grains from the trip-level KPIs on PBI-01 and PBI-02.
- Payment metrics are interpreted at the payment-attempt level.
- Driver metrics are based on the synthetic TripPulse dataset.
- Surge and fulfilment patterns should be treated as descriptive observations rather than causal conclusions.

---

## 11. Dashboard Evidence

| Evidence | File |
|---|---|
| PBI-01 Ride Operations Overview | `screenshots/week09_PBI-01_Ride_Operations_Overview.png` |
| PBI-02 Zone Demand and Surge | `screenshots/week09_PBI-02_Zone_Demand_and_Surge.png` |
| PBI-03 Driver and Payment Reliability | `screenshots/week09_PBI-03_Driver_and_Payment_Reliability.png` |
| Week 09 Power BI notebook | `notebooks/06_powerbi_export.ipynb` |
| Week 09 activity log | `weekly_logs/week09_log.md` |

The reconciliation evidence includes selected Gold queries for:

- PBI-01 KPI reconciliation
- PBI-02 KPI reconciliation
- PBI-03 driver-slice reconciliation
- PBI-03 payment-method reconciliation

The PBI-01 reconciliation evidence was already captured during Week 08, while the PBI-02 and PBI-03 reconciliation evidence was captured during Week 09.

---

## 12. Week 09 Completion Status

The approved Week 09 Power BI implementation and validation activities are complete.

### Completed

- [x] Three approved dashboard pages
- [x] Gold-only reporting model
- [x] Approved KPI cards and visuals
- [x] Required filters and interaction testing
- [x] Cross-page consistency validation
- [x] Evidence-backed insights and limitations
- [x] Selected KPI and visual reconciliation against Gold
- [x] Driver and payment-attempt grain separation
- [x] Final dashboard screenshots
- [x] Reconciliation evidence
- [x] Gold-to-dashboard traceability documentation

### Repository Activity

- [x] `weekly_logs/week09_log.md` updated and committed.
- [x] `dashboard/README.md` updated and committed.
- [x] `notebooks/06_powerbi_export.ipynb` updated and committed.
- [x] Final dashboard screenshots added.
- [x] Reconciliation evidence added.
- [x] Final PBIX stored separately because of the file-size limit.
- [x] Week 09 dashboard implementation, validation, documentation and evidence are complete.
