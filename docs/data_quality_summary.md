# Data Quality Summary

**Week:** 6  
**Purpose:** Summarize data quality rules, failures and business impact.

---

## 1. Quality Rule Results

| Rule ID | Rule Name | Severity | Passed Count | Failed Count | Business Impact |
|---|---|---|---:|---:|---|
| DQ-ZON-001 | Zone reference validity | Critical | 120 | 0 | Invalid zones can break dependent joins and zone-based analysis. |
| DQ-DRV-001 | Driver reference validity | Critical | 2793 | 7 | Invalid drivers can affect driver eligibility and performance analysis. |
| DQ-TRIP-001 | Trip key completeness and uniqueness | Critical | 248500 | 2375 | Missing or duplicate trip IDs can cause double counting and unstable joins. |
| DQ-TRIP-002 | Trip reference integrity | Critical | 248775 | 2100 | Invalid driver or zone references affect trip relationships and joins. |
| DQ-TRIP-003 | Trip timestamp chronology | Critical | 248875 | 2000 | Invalid timestamp order can make response and duration KPIs incorrect. |
| DQ-TRIP-004 | Trip status consistency | Major | 249782 | 1093 | Incorrect status information can distort completion and cancellation metrics. |
| DQ-TRIP-005 | Service and driver compatibility | Major | 249511 | 1364 | Invalid driver-service assignments affect operational analysis. |
| DQ-TRIP-006 | Distance validity | Major | 249623 | 1252 | Invalid distance values affect distance and fare analysis. |
| DQ-TRIP-007 | Fare and surge validity | Major | 249623 | 1252 | Invalid fare or surge values can distort pricing and revenue-related metrics. |
| DQ-TRIP-008 | Declared window and lineage | Major | 249730 | 1145 | Invalid dates or lineage reduce reporting accuracy and reproducibility. |
| DQ-PAY-001 | Payment key and trip integrity | Critical | 178925 | 1390 | Invalid payment IDs or trip references affect payment reconciliation. |
| DQ-PAY-002 | Payment attempt and reconciliation logic | Major | 177635 | 2680 | Invalid payment attempts or final-payment logic can affect payment metrics. |

**Note:** A single physical record can fail multiple DQ rules. Therefore, the sum of rule failures can be greater than the number of distinct quarantined records.

---

## 2. Failed Record Examples

| Rule ID | Sample Record ID | Failure Reason | Action / Handling |
|---|---|---|---|
| DQ-DRV-001 | `DRV-000001` | `DRIVER_REFERENCE_INVALID` — The driver's `home_zone_id` (`ZON-999`) is an invalid or unresolved zone reference. | Record retained in `quarantine_drivers` with **Critical** severity and failure metadata. |
| DQ-TRIP-001 | `TRP-20260103-000414` | `TRIP_KEY_INVALID` — The trip key failed the required trip ID validation. | Record retained in `quarantine_trips` with **Critical** severity and failure metadata. |
| DQ-TRIP-002 | `TRP-20260227-000078` | `TRIP_REFERENCE_ORPHAN` — The driver/reference could not be resolved to a trusted reference. | Record retained in `quarantine_trips` with **Critical** severity and failure metadata. |
| DQ-TRIP-003 | `TRP-20260119-000117` | `TRIP_TIMESTAMP_SEQUENCE_INVALID` — The completed trip has a missing `dropoff_ts`, violating the required lifecycle timestamp sequence. | Record retained in `quarantine_trips` with **Critical** severity and failure metadata. |
| DQ-TRIP-004 | `TRP-20260119-000117` | `TRIP_STATUS_CONDITION_INVALID` — The `completed` status contradicts the missing drop-off timestamp. | Record retained in `quarantine_trips` with **Critical** severity and failure metadata. |
| DQ-TRIP-005 | `TRP-20260227-000078` | `TRIP_SERVICE_ASSIGNMENT_INVALID` — The assigned driver/service combination failed the approved service assignment validation. | Record retained in `quarantine_trips` with **Critical** severity and failure metadata. |
| DQ-TRIP-006 | `TRP-20260120-000179` | `TRIP_DISTANCE_INVALID` — The `estimated_distance_km` value is `-5`, which violates the approved distance validation. | Record retained in `quarantine_trips` with **Major** severity and failure metadata. |
| DQ-TRIP-007 | `TRP-20260312-000563` | `TRIP_FARE_SURGE_INVALID` — The fare/surge validation failed; `surge_multiplier` is `4.5`. | Record retained in `quarantine_trips` with **Major** severity and failure metadata. |
| DQ-TRIP-008 | `TRP-20260127-000246` | `TRIP_WINDOW_OR_LINEAGE_INVALID` — The `request_ts` is `2025-12-31`, outside the approved Jan–Mar 2026 request window. | Record retained in `quarantine_trips` with **Major** severity and failure metadata. |
| DQ-PAY-001 | `PAY-000000314` | `PAYMENT_KEY_OR_TRIP_INVALID` — The payment references trip `TRP-20260401-999999`, which failed payment key/trip integrity validation. | Record retained in `quarantine_payments` with **Critical** severity and failure metadata. |
| DQ-PAY-002 | `PAY-000000081` | `PAYMENT_LOGIC_INVALID` — The payment attempt/status/failure-reason/final-attempt logic failed validation. | Record retained in `quarantine_payments` with **Major** severity and failure metadata. |

---

## 3. What Should Block Gold Metrics?

The following DQ rules prevent affected records from entering Trusted Silver. Therefore, those records must not contribute to Gold metrics:

- **DQ-ZON-001 and DQ-DRV-001:** Invalid zone or driver records can corrupt reference integrity and dependent joins used for mobility and driver analysis.

- **DQ-TRIP-001 to DQ-TRIP-003:** Invalid trip identifiers, unresolved references, or invalid timestamp chronology can directly affect trip counts, lifecycle metrics, response times, wait times, and duration-based KPIs.

- **DQ-TRIP-004 to DQ-TRIP-008:** Invalid trip status, service assignment, distance, fare/surge values, reporting-window values, or lineage can produce unreliable operational and financial metrics.

- **DQ-PAY-001 and DQ-PAY-002:** Invalid payment identity, trip references, attempt logic, payment status, amount, timing, or final-attempt reconciliation can affect payment-related metrics.

Gold metrics must consume governed **Trusted Silver** data only. Records routed to Quarantine are excluded from Gold metric calculations until they are corrected, revalidated, and successfully promoted through the approved Trusted Silver flow.

**Week 06 does not create Gold tables.** The purpose of this stage is to establish trusted and quarantined outputs so that downstream Gold metrics are calculated only from governed data.

---

## 4. Quality Summary

TripPulse Week 06 applied the approved Data Quality rules to the Silver Candidate data.

The final routing preserves failed records in Quarantine instead of silently deleting them.

The final entity-level reconciliation is:

- Zones: **120 Candidate = 120 Trusted + 0 Quarantine**
- Drivers: **2,800 Candidate = 2,793 Trusted + 7 Quarantine**
- Trips: **250,875 Candidate = 241,654 Trusted + 9,221 Quarantine**
- Payments: **180,315 Candidate = 177,635 Trusted + 2,680 Quarantine**

All four entities reconcile with **zero variance**.

During payment DQ processing, an initial reconciliation issue was identified. The issue was caused by a one-to-many join to duplicate trip IDs, which multiplied payment rows during DQ processing. The join was corrected to validate trip existence against distinct trip IDs, restoring the expected one-to-one payment row boundary.

The most important failures for downstream dashboards are Critical key, reference and timestamp failures, along with Major trip and payment logic failures, because these can affect record counts, joins and KPI calculations.

Week 06 establishes the governed Trusted Silver and Quarantine outputs required for reliable downstream analytics. Gold tables are not created as part of this week's scope.
