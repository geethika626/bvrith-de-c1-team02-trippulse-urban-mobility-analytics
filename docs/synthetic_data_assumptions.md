# Synthetic Data Assumptions

**Project:** Trippulse - Urban Mobility Analysis
**Week:** 2  
**Purpose:** Document how synthetic data is generated and the assumptions followed for the TripPulse Urban Mobility Analytics project.

---

## 1. Synthetic Data Boundary

This project uses synthetic educational data generated for learning, experimentation and analytical purposes only. The dataset does not represent any real ride-hailing platform, drivers, customers or geographical locations. No personally identifiable information (PII) has been included.

The datasets are used to simulate operational ride-hailing data and support data exploration, validation, transformation and analytical use cases throughout the TripPulse project.

---

## 2. Domain Assumptions

| Area | Assumption |
|---|---|
| Platform | Fictional ride-hailing platform named TripPulse providing urban mobility services |
| Geography / Scope | Operations are simulated across fictional city zones for educational purposes |
| Time Period | The synthetic data represents the project-defined TripPulse analysis period |
| Source Systems | Synthetic operational datasets representing Trips, Drivers, Payments and Zone reference data |
| Ride Categories | Economy, Sedan and Premium |
| Trip Status | Requested, Accepted, Picked Up, Completed and Cancelled |
| Payment Methods | UPI, Credit Card, Debit Card, Wallet and Net Banking |
| Reference Data | Synthetic driver records and fictional zone reference data |
| Streaming Events | Sample ride lifecycle event datasets are provided separately as JSON event drops |

---

## 3. Source Data Assumptions

The TripPulse project uses four primary batch datasets:

- `trips.parquet`
- `drivers.json`
- `zones.csv`
- `payments.csv`

The project also contains streaming event-drop files used for streaming and event-processing scenarios:

- `ride_request_event_drop_01.json`
- `ride_request_event_drop_02.json`

The batch datasets represent operational entities, while the streaming event files represent incremental ride lifecycle events.

---

## 4. Data Volume Assumptions

The supplied datasets were profiled during Week 03. The observed physical row counts are:

| File | Observed Physical Rows | Purpose |
|---|---:|---|
| `trips.parquet` | 250,875 | Stores trip booking details, ride lifecycle, trip status, distance and fare information |
| `drivers.json` | 2,800 | Stores synthetic driver records and driver-related attributes |
| `zones.csv` | 120 | Stores fictional zone reference information |
| `payments.csv` | 180,315 | Stores payment attempts and payment-related attributes |
| `ride_request_event_drop_01.json` | Small event sample | Provides a sample batch of ride lifecycle events |
| `ride_request_event_drop_02.json` | Small event sample | Provides an incremental sample of ride lifecycle events |

These observed counts are used as the source-profile baseline for the TripPulse project.

---

## 5. Dataset Grain Assumptions

Each source dataset has a different business grain.

| Dataset | Grain |
|---|---|
| `zones.csv` | One row per zone record |
| `drivers.json` | One row per driver record |
| `trips.parquet` | One row per trip record |
| `payments.csv` | One row per payment attempt |
| `ride_request_event_drop_01.json` / `ride_request_event_drop_02.json` | One row per ride lifecycle event |

The different grains must be considered when joining datasets and calculating analytical metrics.

In particular, payment records represent payment attempts rather than independent trips. Therefore, joining Trips directly with Payments can multiply trip rows when a trip has multiple payment attempts.

Similarly, streaming event records represent lifecycle events and must not be counted directly as independent trip requests.

---

## 6. Controlled Data Quality Conditions

The supplied synthetic datasets contain data-quality conditions that are useful for validation and analytical testing.

| Issue Type | Observed / Assumed Condition | Validation Approach |
|---|---|---|
| Null Trip IDs | Some trip records contain a null `trip_id` | Check null values in the trip identifier |
| Duplicate Trip IDs | Duplicate `trip_id` values are present in the Trips dataset | Compare physical row count with distinct trip IDs |
| Missing Driver References | Some trip records do not have a valid driver reference | Validate Trips against Drivers using a reference check |
| Invalid Pickup Zone References | Some trip records do not have a valid pickup-zone reference | Validate `pickup_zone_id` against `zones.zone_id` |
| Invalid Drop-off Zone References | Drop-off zone references are checked against the Zones dataset | Validate `dropoff_zone_id` against `zones.zone_id` |
| Payment References | Payment records are validated against Trip records | Validate `payment.trip_id` against `trip.trip_id` |
| Multiple Payment Attempts | A trip can have multiple payment attempts | Use payment-attempt grain when analysing payments |
| Timestamp Relationships | Trip lifecycle timestamps are checked for logical ordering | Validate request, acceptance, pickup, drop-off and cancellation timestamps |

These conditions are treated as validation findings rather than silently removing the affected records during source exploration.

---

## 7. Week 03 Source Profiling Baseline

The Week 03 source profiling established the following identifier statistics:

| Source | Physical Rows | Non-Null PK | Distinct PK | Null PK Rows | Duplicate Key Rows |
|---|---:|---:|---:|---:|---:|
| Zones | 120 | 120 | 120 | 0 | 0 |
| Drivers | 2,800 | 2,800 | 2,800 | 0 | 0 |
| Trips | 250,875 | 250,250 | 249,375 | 625 | 875 |
| Payments | 180,315 | 180,315 | 180,000 | 0 | 315 |

The Trips dataset contains both null and duplicate trip identifiers. The Payments dataset also contains duplicate payment identifiers at the source-profile level.

These findings are retained as part of the source validation evidence.

---

## 8. Relationship Assumptions and Validation

The main relationships between the TripPulse datasets are:

| Relationship | Description |
|---|---|
| `drivers.home_zone_id → zones.zone_id` | Associates a driver with a home zone |
| `trips.driver_id → drivers.driver_id` | Associates a trip with a driver |
| `trips.pickup_zone_id → zones.zone_id` | Associates a trip with its pickup zone |
| `trips.dropoff_zone_id → zones.zone_id` | Associates a trip with its drop-off zone |
| `payments.trip_id → trips.trip_id` | Associates a payment attempt with a trip |

Week 03 relationship validation produced the following orphan-row results:

| Relationship | Orphan Rows |
|---|---:|
| Driver → Home Zone | 7 |
| Trip → Driver | 875 |
| Trip → Pickup Zone | 625 |
| Trip → Drop-off Zone | 0 |
| Payment → Trip | 762 |

These results demonstrate that the relationships need to be validated before they are used for downstream analytical processing.

---

## 9. Trip-to-Payment Overcount Assumption

The Trips and Payments datasets have different grains.

A trip may have multiple payment attempts. Therefore, directly joining Trips and Payments can produce multiple rows for a single trip.

The Week 03 overcount demonstration produced:

| Metric | Value |
|---|---:|
| Joined Rows | 271,122 |
| Distinct Trip Requests | 249,375 |
| Multiplication Rows | 21,747 |

This demonstrates that payment-level rows must not be treated as independent trips when calculating trip-level metrics.

Trip-level KPIs should therefore use the appropriate trip-level grain and aggregation logic.

---

## 10. Streaming Data Assumptions

The TripPulse streaming data is supplied as JSON event-drop files.

The streaming event schema includes fields such as:

- `event_id`
- `schema_version`
- `event_ts`
- `event_type`
- `trip_id`
- `driver_id`
- `pickup_zone_id`
- `dropoff_zone_id`
- `service_type`
- `status_from`
- `status_to`
- `surge_multiplier`
- `estimated_fare_inr`
- `producer_run_id`
- `event_sequence_no`

The streaming data represents ride lifecycle events rather than independent trip records.

The `event_sequence_no` field is used to represent the sequence of events within a trip lifecycle.

The supplied event data also provides cases that can be used for streaming data-quality validation, including duplicate event identifiers, event-sequence anomalies, schema-version changes and schema-drift testing.

---

## 11. Data Validation Assumptions

Source validation is performed before downstream transformation.

The validation focuses on:

1. Source file availability
2. Schema consistency
3. Physical row counts
4. Null identifier checks
5. Duplicate identifier checks
6. Distinct key counts
7. Foreign-key/reference validation
8. Dataset grain
9. Timestamp relationships
10. Trip-to-payment multiplication risk

Validation findings are documented rather than hidden from downstream processing.

---

## 12. Manual Verification

The Week 03 source profiling verified the following:

- All four primary batch source datasets were loaded and inspected.
- Schemas were inspected against the TripPulse data dictionary.
- Physical row counts and distinct identifier counts were profiled.
- Null and duplicate identifier conditions were identified.
- Driver, trip, zone and payment relationships were checked.
- Trip-to-payment overcount risk was demonstrated.
- The source datasets were analysed at their respective grains.
- A Bronze demonstration table was created for the Week 03 exercise.
- A downstream lineage demonstration view was created.
- Streaming event drops were treated separately from the Week 03 batch-source profiling.

---

## 13. Use of Synthetic Data

The synthetic datasets are intended for:

- Data engineering practice
- Source profiling
- Data-quality validation
- ETL/ELT pipeline development
- Analytical modelling
- KPI development
- Power BI reporting
- Streaming-event processing experiments

The data should not be interpreted as real-world operational, financial or geographic information.

---

## 14. Documentation and Maintenance

This document records the assumptions and source-profile characteristics used for the TripPulse project.

If the supplied synthetic data, source schemas or project assumptions change, the corresponding documentation and validation evidence should also be updated.

The source-profile results documented here provide the baseline for subsequent Bronze, Silver, Gold and analytical processing stages.
