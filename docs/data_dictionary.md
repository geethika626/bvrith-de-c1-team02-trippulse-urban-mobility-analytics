# Data Dictionary

**Week:** 2  
**Purpose:** Define raw, reference, Silver, and streaming fields.

---

## 1. Source File Catalog

| File Name | Grain | Purpose | Approx. Rows | Notes |
|---|---|---|---:|---|
| trips.parquet | One row per trip request | Stores ride request and trip lifecycle information | ~250,000 | Batch source file |
| drivers.json | One row per driver snapshot | Stores driver and vehicle information | ~8,000 | Reference/master data |
| zones.csv | One row per fictional zone | Stores zone and demand information | ~150 | Reference data |
| payments.csv | One row per payment attempt | Stores payment transaction details | ~230,000 | One trip can have multiple payment attempts |
| ride_request_event_drop_01.json | One row per event | Streaming simulation | Small sample | JSON Lines file |
| ride_request_event_drop_02.json | One row per event | Incremental streaming simulation | Small sample | JSON Lines file |
---

## 2. Raw File Schema: trips.parquet

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| trip_id | string | Yes | TRIP-000001 | Unique trip identifier |
| request_ts | timestamp | Yes | 2026-07-15 09:15:00 | Ride request timestamp |
| driver_id | string | Conditional | DRV-0001 | Assigned driver ID |
| pickup_zone_id | string | Yes | ZN-001 | Pickup zone |
| dropoff_zone_id | string | Yes | ZN-015 | Drop-off zone |
| trip_status | string | Yes | COMPLETED | Current trip status |
| estimated_distance_km | float | Yes | 12.5 | Estimated distance |
| estimated_fare_inr | float | Yes | 250.00 | Estimated fare |
| surge_multiplier | float | Yes | 1.5 | Surge pricing multiplier |

---

## 3. Raw File Schema: drivers.json

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| driver_id | string | Yes | DRV-000001 | Unique driver identifier |
| home_zone_id | string | Yes | ZON-999 | Driver's assigned home zone |
| onboard_date | timestamp | Yes | 2024-03-15T00:00:00.000 | Driver onboarding date |
| vehicle_type | string | Yes | sedan | Vehicle category |
| service_type | string | Yes | sedan | Type of service offered |
| driver_status | string | Yes | active | Current driver status |
| rating | float | Yes | 3.96 | Driver rating score |
| lifetime_completed_trips | integer | Yes | 2135 | Total completed trips |
| last_status_update_ts | timestamp | Yes | 2026-01-28T15:05:00.000Z | Last driver status update timestamp |
| source_record_version | integer | Yes | 1 | Source record version number |
| effective_from | timestamp | Yes | 2026-01-01 | Date from which the zone definition is effective |

---



## 4. Raw File Schema: payments.csv

| Field Name | Data Type | Required? | Example | Description |
|------------|-----------|-----------|---------|-------------|
| payment_id | string | Yes | PAY-00000001 | Unique payment identifier |
| trip_id | string | Yes | TRP-2026XXXX | Associated trip identifier |
| attempt_number | integer | Yes | 1 | Payment attempt number for a trip |
| payment_ts | timestamp | Yes | 2026-01-01 18:25:00 | Payment transaction timestamp |
| payment_method | string | Yes | upi | Payment method used |
| payment_status | string | Yes | success | Status of the payment transaction |
| amount_inr | float | Yes | 184.95 | Transaction amount in INR |
| failure_reason | string | No | timeout | Reason for payment failure, if any |
| is_final_attempt | boolean | Yes | TRUE | Indicates whether this is the final payment attempt |
| payment_reference | string | Yes | TPREF-00000001 | Unique payment reference number |

## 4. Reference File Schema: zones.csv

| Field Name | Data Type | Required? | Example | Description |
|------------|-----------|-----------|---------|-------------|
| zone_id | string | Yes | ZON-001 | Unique zone identifier |
| zone_name | string | Yes | TPC Zone | Name of the fictional city zone |
| zone_type | string | Yes | residential | Zone classification |
| city_code | string | Yes | TPC | Fictional city code |
| demand_band | string | Yes | low | Demand category of the zone |
| is_active | boolean | Yes | TRUE | Indicates whether the zone is active |
| effective_from | timestamp | Yes | 2026-01-01 | Date from which the zone definition is effective |

---

## 5. Canonical Silver Table Design

| Silver Field | Type | Source Mapping | Business Meaning |
|--------------|------|---------------|----------------|
| trip_id | string | trips.trip_id | Unique trip identifier |
| request_date | date | date(trips.request_ts) | Date used for analytics and reporting |
| driver_id | string | trips.driver_id | Assigned driver's identifier |
| pickup_zone_name | string | join zones on pickup_zone_id | Pickup location of the trip |
| dropoff_zone_name | string | join zones on dropoff_zone_id | Destination location of the trip |
| service_type | string | trips.service_type | Type of ride requested |
| trip_status | string | upper(trips.trip_status) | Standardized trip status |
| estimated_distance_km | double | trips.estimated_distance_km | Estimated travel distance |
| actual_distance_km | double | trips.actual_distance_km | Actual completed travel distance |
| estimated_fare_inr | double | trips.estimated_fare_inr | Estimated trip fare |
| final_fare_inr | double | trips.final_fare_inr | Final fare charged to the customer |
| payment_status | string | join payments on trip_id | Status of payment transaction |
| surge_multiplier | double | trips.surge_multiplier | Dynamic pricing factor |
| driver_rating | double | join drivers on driver_id | Driver's latest rating |
| is_completed_trip | boolean | derived from trip_status | Indicates whether the trip was successfully completed |
```

---

## 6. Streaming Event Schema: ride_request_event_drop_01.json

| Field Name | Data Type | Required? | Example | Description |
|------------|-----------|-----------|---------|-------------|
| event_id | string | Yes | EVT-20260401-000001 | Unique event identifier |
| schema_version | string | Yes | 1.0 | Version of the streaming event schema |
| event_ts | timestamp | Yes | 2026-03-31T18:31:10.000Z | Event timestamp |
| event_type | string | Yes | ride_requested | Type of ride lifecycle event |
| trip_id | string | Yes | TRP-20260331-000132 | Associated trip identifier |
| driver_id | string | Yes | DRV-001461 | Assigned driver identifier |

---

## 7. Streaming Event Schema: ride_request_event_drop_02.json

| Field Name | Data Type | Required? | Example | Description |
|------------|-----------|-----------|---------|-------------|
| event_id | string | Yes | EVT-20260401-000051 | Unique event identifier |
| schema_version | string | Yes | 1.0 | Version of the streaming event schema |
| event_ts | timestamp | Yes | 2026-03-31T18:55:30.000Z | Event timestamp |
| event_type | string | Yes | driver_accepted | Type of ride lifecycle event |
| trip_id | string | Yes | TRP-20260331-000506 | Associated trip identifier |
| driver_id | string | Yes | DRV-002003 | Assigned driver identifier |

---
