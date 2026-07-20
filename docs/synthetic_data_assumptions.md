# Synthetic Data Assumptions

**Week:** 2  
**Purpose:** Document how educational data is created.

---

## 1. Synthetic Data Boundary

This project uses synthetic educational data generated for learning, experimentation and analytical purposes only. The dataset does not represent any real ride-hailing platform, drivers, customers or geographical locations. No personally identifiable information (PII) has been included.

---

## 2. Domain Assumptions

| Area | Assumption |
|---|---|
| Geography / Scope | Fictional urban mobility platform operating across fictional city zones |
| Time Period | January 2026 – June 2026 |
| Source Systems | Trip Management System, Driver Management System, Payment System, Zone Reference System and Ride Event Streaming System |
| Service Types | Economy, Sedan and Premium ride services |
| Trip Status | Requested, Accepted, Picked Up, Completed and Cancelled |
| Payment Methods | UPI, Credit Card, Debit Card, Wallet and Net Banking |
| Reference Data | Fictional zones and driver information |
| Streaming Events | Ride requested, driver accepted, trip started, trip completed and trip cancelled events |

---

## 3. Data Volume Assumptions

| File | Approximate Rows | Reason |
|---|---:|---|
| trips.parquet | ~250,000 | Stores trip requests and ride lifecycle information |
| drivers.json | ~8,000 | Stores driver snapshots and service details |
| zones.csv | ~150 | Stable reference data for fictional city zones |
| payments.csv | ~230,000 | Stores payment attempts and transaction details |
| ride_request_event_drop_01.json | Small sample | Initial streaming event simulation |
| ride_request_event_drop_02.json | Small sample | Incremental streaming event simulation |

---

## 4. Controlled Data Quality Issues

| Issue Type | Approx. Share | Why Include It |
|---|---:|---|
| Duplicate Trip IDs | A small percentage of duplicate trip identifiers may be intentionally introduced | COUNT(*) - COUNT(DISTINCT trip_id) |
| Missing Driver IDs | Driver identifiers may be NULL before assignment or for cancelled trips | COUNT(driver_id IS NULL) |
| Invalid Zone References | Some trips may contain invalid pickup or drop-off zone identifiers | Anti-join with zones.csv |
| Timestamp Inconsistencies | Invalid ride progression timestamps may be introduced | pickup_ts < request_ts or dropoff_ts < pickup_ts |
| Payment Failures | Failed payment attempts are intentionally simulated | COUNT(payment_status='failed') |
| Invalid Fare Values | Negative or inconsistent fare values may be introduced for validation purposes | final_fare_inr < 0 |

---

## 5. Manual Verification

- Source file schemas were verified against the TripPulse data contract.
- Required fields are present in the source, reference, payment and streaming datasets.
- Trip identifiers follow the expected naming convention across the available datasets.
- Nullable fields are logically consistent with trip status values. Unfulfilled and cancelled trips may legitimately contain NULL values for driver assignment, pickup, drop-off, actual distance and final fare attributes.
- The dataset contains multiple ride service categories and trip status values that align with the TripPulse domain assumptions.
- Surge multiplier values are greater than or equal to 1.0 in the examined records.
- Sample datasets are maintained separately from the complete source datasets.
