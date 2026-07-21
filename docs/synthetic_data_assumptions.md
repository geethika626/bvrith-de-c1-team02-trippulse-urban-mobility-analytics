# Synthetic Data Assumptions

**Week:** 2  
**Purpose:** Document how synthetic data is generated and the assumptions followed for the TripPulse Urban Mobility Analytics project.

---

## 1. Synthetic Data Boundary

This project uses synthetic educational data generated for learning, experimentation and analytical purposes only. The dataset does not represent any real ride-hailing platform, drivers, customers or geographical locations. No personally identifiable information (PII) has been included.

---

## 2. Domain Assumptions

| Area | Assumption |
|---|---|
| Platform | Fictional ride-hailing platform named TripPulse providing urban mobility services |
| Geography / Scope | Operations are simulated across fictional city zones for educational purposes |
| Time Period | January 2026 – June 2026 |
| Source Systems | Trip Management System, Driver Management System, Payment Processing System, Zone Master Data and Ride Event Streaming System |
| Ride Categories | Economy, Sedan and Premium |
| Trip Status | Requested, Accepted, Picked Up, Completed and Cancelled |
| Payment Methods | UPI, Credit Card, Debit Card, Wallet and Net Banking |
| Reference Data | Synthetic driver records and fictional zone reference data |
| Streaming Events | Ride Requested, Driver Assigned, Trip Started, Trip Completed and Trip Cancelled |

---

## 3. Data Volume Assumptions

| File | Approximate Rows | Reason |
|---|---:|---|
| trips.parquet | ~250,000 | Stores trip booking details, ride lifecycle, distance, fare and trip status information |
| drivers.json | ~8,000 | Stores driver profiles, vehicle category and availability details |
| zones.csv | ~150 | Stores fictional city zone reference information |
| payments.csv | ~230,000 | Stores payment transactions, payment methods and payment status |
| ride_request_event_drop_01.json | Small sample | Simulates the first batch of ride request streaming events |
| ride_request_event_drop_02.json | Small sample | Simulates incremental ride request streaming events |

---

## 4. Controlled Data Quality Issues

| Issue Type | Approx. Share | Why Include It |
|---|---:|---|
| Duplicate Trip IDs | A small percentage of duplicate trip identifiers may be intentionally introduced | COUNT(*) - COUNT(DISTINCT trip_id) |
| Missing Driver IDs | Driver identifiers may be NULL before assignment or for cancelled trips | COUNT(driver_id IS NULL) |
| Invalid Zone References | Some trips may contain invalid pickup or drop-off zone identifiers | Anti-join with zones.csv |
| Timestamp Inconsistencies | Invalid ride progression timestamps may be introduced | pickup_ts < request_ts OR dropoff_ts < pickup_ts |
| Payment Failures | Failed payment attempts are intentionally simulated | COUNT(payment_status = 'failed') |
| Invalid Fare Values | Negative or inconsistent fare values may be introduced for validation purposes | final_fare_inr < 0 |

---

## 5. Manual Verification

- Source file schemas were verified against the TripPulse Urban Mobility Analytics data contract.
- Required fields are present in the trip, driver, payment, zone and streaming event datasets.
- Trip identifiers follow the expected naming convention across all available datasets.
- Nullable fields are logically consistent with trip status values. Cancelled and unassigned trips may legitimately contain NULL values for driver assignment, pickup time, drop-off time,      travelled distance and final fare.
- The dataset contains multiple ride categories and trip status values that align with the TripPulse domain assumptions.
- Surge multiplier values are greater than or equal to 1.0 in the validated records.
- Sample datasets are maintained separately from the complete source datasets for testing and analytical purposes.
