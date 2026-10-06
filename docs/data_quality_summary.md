# Data Quality Summary
**Project:** Trippulse - Urban Mobility Analysis  
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
| DQ-TRIP-004 | `TRP-20260119-000117` | `TRIP_STATUS_CONDITION_INVALID` — The `completed` status contradicts the missing drop-off timestamp. | Record retained in `quarantine_trips` with **Major** severity and failure metadata. |
| DQ-TRIP-005 | `TRP-20260227-000078` | `TRIP_SERVICE_ASSIGNMENT_INVALID` — The assigned driver/service combination failed the approved service assignment validation. | Record retained in `quarantine_trips` with **Major** severity and failure metadata. |
| DQ-TRIP-006 | `TRP-20260120-000179` | `TRIP_DISTANCE_INVALID` — The `estimated_distance_km` value is `-5`, which violates the approved distance validation. | Record retained in `quarantine_trips` with **Major** severity and failure metadata. |
| DQ-TRIP-007 | `TRP-20260312-000563` | `TRIP_FARE_SURGE_INVALID` — The fare/surge validation failed; `surge_multiplier` is `4.5`. | Record retained in `quarantine_trips` with **Major** severity and failure metadata. |
| DQ-TRIP-008 | `TRP-20260127-000246` | `TRIP_WINDOW_OR_LINEAGE_INVALID` — The `request_ts` is `2025-12-31`, outside the approved Jan–Mar 2026 request window. | Record retained in `quarantine_trips` with **Major** severity and failure metadata. |
| DQ-PAY-001 | `PAY-000000314` | `PAYMENT_KEY_OR_TRIP_INVALID` — The payment references trip `TRP-20260401-999999`, which failed payment key/trip integrity validation. | Record retained in `quarantine_payments` with **Critical** severity and failure metadata. |
| DQ-PAY-002 | `PAY-000000081` | `PAYMENT_LOGIC_INVALID` — The payment attempt/status/failure-reason/final-attempt logic failed validation. | Record retained in `quarantine_payments` with **Major** severity and failure metadata. |

---

## 3. What Should Block Gold Metrics?

The following DQ rules prevent affected records from entering Trusted Silver. Therefore, those records must not contribute to Gold metrics.

### Critical Rules

- **DQ-ZON-001 and DQ-DRV-001:** Invalid zone or driver records can corrupt reference integrity and dependent joins used for mobility and driver analysis.

- **DQ-TRIP-001:** Invalid or duplicate trip identifiers can affect trip counts and create unstable joins.

- **DQ-TRIP-002:** Invalid trip references can break relationships between trips, drivers and zones.

- **DQ-TRIP-003:** Invalid timestamp chronology can directly affect response-time, wait-time and duration-based KPIs.

- **DQ-PAY-001:** Invalid payment identity or trip references can affect payment reconciliation.

### Major Rules

- **DQ-TRIP-004:** Invalid trip status information can distort completion and cancellation metrics.

- **DQ-TRIP-005:** Invalid driver-service assignments can affect operational analysis.

- **DQ-TRIP-006:** Invalid distance values can affect distance and fare-related analysis.

- **DQ-TRIP-007:** Invalid fare or surge values can distort pricing and revenue-related metrics.

- **DQ-TRIP-008:** Invalid reporting-window or lineage information can reduce reporting accuracy and reproducibility.

- **DQ-PAY-002:** Invalid payment attempt or reconciliation logic can affect payment-related metrics.

Gold metrics must consume governed **Trusted Silver** data only. Records routed to Quarantine are excluded from Gold metric calculations until they are corrected, revalidated and successfully promoted through the approved Trusted Silver flow.

**Week 06 does not create Gold tables.** The purpose of this stage is to establish trusted and quarantined outputs so that downstream Gold metrics are calculated only from governed data.

---

## 4. Entity-Level Reconciliation

The final entity-level reconciliation between Silver Candidate, Trusted Silver and Quarantine is:

| Entity | Candidate Rows | Trusted Rows | Quarantine Rows | Variance |
|---|---:|---:|---:|---:|
| Zones | 120 | 120 | 0 | 0 |
| Drivers | 2,800 | 2,793 | 7 | 0 |
| Trips | 250,875 | 241,654 | 9,221 | 0 |
| Payments | 180,315 | 177,635 | 2,680 | 0 |

All four entities reconcile with **zero variance**.

This confirms that records routed to Trusted Silver and Quarantine account for the complete Silver Candidate input without unexplained row loss.

---

## 5. Quality Summary

TripPulse Week 06 applied the approved Data Quality rules to the Silver Candidate data.

The final routing preserves failed records in Quarantine instead of silently deleting them.

The largest entity-level quarantine volume occurs in the Trips dataset, where **9,221 records** are routed to Quarantine. This is followed by the Payments dataset with **2,680 quarantined records**, while **7 driver records** are quarantined and all **120 zone records** pass the final entity-level reconciliation.

A single physical record can fail multiple DQ rules, so individual rule failure counts should not be added together to determine the number of distinct quarantined records.

During payment DQ processing, an initial reconciliation issue was identified. The issue was caused by a one-to-many join to duplicate trip IDs, which multiplied payment rows during DQ processing. The join was corrected to validate trip existence against distinct trip IDs, restoring the expected one-to-one payment row boundary.

The most important failures for downstream dashboards are Critical key, reference and timestamp failures, along with Major trip and payment logic failures, because these can affect record counts, joins and KPI calculations.

Week 06 establishes the governed Trusted Silver and Quarantine outputs required for reliable downstream analytics. Gold tables are not created as part of this week's scope.

---

## 6. Business Impact

The identified data-quality failures can affect downstream TripPulse analytics in several ways:

- Missing or duplicate trip identifiers can cause incorrect trip counts.
- Invalid driver or zone references can produce incomplete joins.
- Invalid timestamps can distort response-time and trip-duration KPIs.
- Incorrect trip statuses can affect completion and cancellation rates.
- Invalid service assignments can affect driver and operational analysis.
- Invalid distance values can affect distance-related metrics.
- Invalid fare or surge values can affect pricing and revenue-related metrics.
- Invalid payment references can affect payment reconciliation.
- Invalid payment-attempt logic can affect payment reliability metrics.

Therefore, only governed Trusted Silver records are eligible for downstream Gold processing.

---

## 7. Week 06 Completion Status

The Week 06 Data Quality stage is complete.

- [x] Data Quality rules applied
- [x] Rule-level pass/fail results documented
- [x] Failed-record examples documented
- [x] Critical and Major business impacts documented
- [x] Failed records routed to Quarantine
- [x] Trusted Silver outputs established
- [x] Entity-level reconciliation completed
- [x] Zero reconciliation variance confirmed
- [x] Payment DQ join/reconciliation issue resolved
- [x] Gold-blocking rules documented
- [x] Gold tables not created in Week 06, as required by the workflow
