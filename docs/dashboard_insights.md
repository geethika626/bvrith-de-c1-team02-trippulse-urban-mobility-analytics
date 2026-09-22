# Dashboard Insights

**Week:** 9  
**Purpose:** Explain the evidence-backed observations from the Power BI dashboard and document Gold-table traceability, validation, reconciliation, and limitations.

---

## 1. Dashboard Pages

| Page | Purpose | Main Visuals |
|---|---|---|
| PBI-01: Ride Operations Overview | Understand trusted ride fulfilment efficiency and operational losses | KPI cards, daily requests vs completions, trip outcome by service type, top pickup zones |
| PBI-02: Zone Demand and Surge | Understand concentrated zone demand, surge exposure and weaker fulfilment | KPI cards, zone demand matrix, surge-band comparison, top pickup zones, service-type response time |
| PBI-03: Driver and Payment Reliability | Understand fictional driver assignment outcomes and payment-attempt reliability | KPI cards, driver reliability vs assigned volume, payment success trend, driver cancellation counts, payment status/retry distribution |

**Note:** Streaming event reporting is not included in Week 09. Streaming simulation is planned for Week 10.

---

## 2. Key Insights

### PBI-01 — Ride Operations Overview

**Key Insight:**  
Requests remain consistently high across the selected period, while completion is around 64%, with cancellation and unfulfilled requests accounting for the remaining operational losses.

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
Requests remain concentrated across top pickup zones, with surge trips around 55% and completion around 64% in the selected Gold slice.

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
Driver reliability is around 72.9% with average response time of about 3.7 minutes, while payment attempt success is 56.1% with an average of 1.16 attempts per trip.

**Observed KPI values for 01 Jan 2026–14 Jan 2026:**

- Driver Reliability Rate: 72.91%
- Average Driver Response: 3.67 minutes
- Average Trip Duration: 47.44 minutes
- Payment Attempt Success Rate: 56.11%
- Average Attempts per Trip: 1.16

**Limitation:**  
Driver metrics are fictional educational measures and should not be interpreted as real worker evaluation; payment success is measured at the payment-attempt level.

---

## 3. How the Dashboard Uses Gold Tables

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

## 4. Power BI Validation and Reconciliation

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

### Reconciliation Evidence

#### PBI-01

Selected date range: **01 Jan 2026–14 Jan 2026**

| KPI | Gold | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |

#### PBI-02

Selected date range: **01 Jan 2026–14 Jan 2026**

| KPI | Gold | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.1217% | 64.12% | PASS |
| Cancellation Rate | 23.8230% | 24% | PASS |
| Surge Trip Share | 55.2808% | 55.28% | PASS |
| Average Final Fare | ₹446.6148 | ₹446.61 | PASS |

#### PBI-03 — Driver Slice

Selected date range: **01 Jan 2026–14 Jan 2026**  
Driver: **DRV-000008**

| KPI | Gold | Power BI | Status |
|---|---:|---:|---|
| Driver Reliability Rate | 88.8889% | 88.89% | PASS |
| Average Driver Response Minutes | 3.7630 | 3.76 | PASS |
| Average Trip Duration | 35.8188 minutes | 35.82 | PASS |

#### PBI-03 — Payment Method Slice

Selected date range: **01 Jan 2026–14 Jan 2026**  
Payment Method: **UPI**

| KPI | Gold | Power BI | Status |
|---|---:|---:|---|
| Payment Attempt Success Rate | 36.2996% | 36.30% | PASS |
| Average Attempts per Trip | 1.4051 | 1.41 | PASS |

**Reconciliation note:**  
Average Attempts per Trip was reconciled using the same approved Power BI calculation, `AVERAGE(avg_attempts_per_trip)`, so the Gold result and Power BI result agree after rounding.

---

## 5. Cross-Page Consistency

For the common date range **01 Jan 2026–14 Jan 2026**, PBI-01 and PBI-02 use the same governed trip request, completion, and cancellation measures.

Observed values:

- Total Trip Requests: **37,892**
- Completion Rate: **64.12%**
- Cancellation Rate: **approximately 24%**

PBI-03 uses driver-performance and payment-attempt grains, so its page-specific measures are interpreted separately from the trip-request KPIs.

---

## 6. Synthetic Data Limitation

TripPulse is an educational synthetic-data project. Dashboard observations describe patterns present in the generated Gold data and should not be interpreted as real-world operational, driver-performance, revenue, or payment-business conclusions.

Driver-related metrics are fictional educational measures and should not be interpreted as evaluations of real workers.

Payment success is measured at the payment-attempt level and should not be interpreted as a direct measure of overall customer payment behaviour.

Surge and fulfilment patterns in the synthetic dataset show observed associations only and do not establish causal relationships.

---

## 7. Evidence and Screenshots

Final dashboard screenshots:

- `screenshots/week09_PBI-01_Ride_Operations_Overview.png`
- `screenshots/week09_PBI-02_Zone_Demand_and_Surge.png`
- `screenshots/week09_PBI-03_Driver_and_Payment_Reliability.png`

The reconciliation evidence includes selected Gold queries for:

- PBI-01 KPI reconciliation
- PBI-02 KPI reconciliation
- PBI-03 driver slice reconciliation
- PBI-03 payment-method slice reconciliation

The PBI-01 reconciliation evidence was already captured during Week 08, while the PBI-02 and PBI-03 reconciliation evidence was captured during Week 09.

---

## 8. Week 09 Completion Status

The approved Week 09 Power BI implementation and validation activities are complete.

Completed:

- Three approved dashboard pages
- Gold-only reporting model
- Approved KPI cards and visuals
- Required filters and interaction testing
- Cross-page consistency validation
- Evidence-backed insights and limitations
- Selected KPI and visual reconciliation against Gold
- Driver and payment-attempt grain separation
- Final dashboard screenshots
- Reconciliation evidence

Repository activity:

- `weekly_logs/week09_log.md` updated and committed.
- `dashboard/README.md` updated and committed.
- `notebooks/06_powerbi_export.ipynb` updated and committed.
- Final dashboard screenshots and reconciliation evidence added.
- Final PBIX stored separately because of the file-size limit.
- Week 09 dashboard implementation, validation, documentation and evidence are complete.
