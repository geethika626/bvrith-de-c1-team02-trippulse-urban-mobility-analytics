# Data Dictionary

**Week:** 2  
**Purpose:** Define raw, reference, Silver, and streaming fields used in the TripPulse Urban Mobility Analytics project.
---

## 1. Source File Catalog

| File Name | Grain | Purpose | Number of Rows | Notes |
|---|---|---|---:|---|
| trips.parquet | One row per trip request | Stores ride request and complete trip lifecycle information | 250875 | Batch source file |
| drivers.json | One row per driver snapshot | Stores driver and vehicle information | 2800 | Reference/master data |
| zones.csv | One row per fictional zone | Stores zone and demand information | 120 | Reference data |
| payments.csv | One row per payment attempt | Stores payment transaction details | 180315 | One trip can have multiple payment attempts |
| ride_request_event_drop_01.json | One row per event | Streaming ride request events | Small sample | JSON Lines file (Streaming simulation) |
| ride_request_event_drop_02.json | One row per event | Incremental streaming ride events | Small sample | JSON Lines file (Streaming simulation) |

---

## 2. Raw File Schema: trips.parquet

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| trip_id | string | Yes | TRIP-000001 | Unique trip identifier |
| request_ts | timestamp | Yes | 2026-07-15 09:15:00 | Ride request timestamp |
| driver_accept_ts | timestamp | No | 2026-07-15 09:17:20 | Driver accepted timestamp |
| pickup_ts | timestamp | No | 2026-07-15 09:20:15 | Pickup timestamp |
| dropoff_ts | timestamp | No | 2026-07-15 09:45:10 | Drop-off timestamp |
| cancel_ts | timestamp | No | 2026-07-15 09:18:00 | Cancellation timestamp |
| record_created_ts | timestamp | Yes | 2026-07-15 09:15:01 | Record creation timestamp |
| driver_id | string | Conditional | DRV-000001 | Assigned driver ID |
| pickup_zone_id | string | Yes | ZON-001 | Pickup zone |
| dropoff_zone_id | string | Yes | ZON-015 | Drop-off zone |
| service_type | string | Yes | Sedan | Ride service category |
| trip_status | string | Yes | COMPLETED | Current trip status |
| cancellation_reason | string | No | Customer Cancelled | Reason for trip cancellation |
| estimated_distance_km | double | Yes | 12.5 | Estimated trip distance |
| actual_distance_km | double | No | 12.9 | Actual distance travelled |
| estimated_fare_inr | double | Yes | 250.00 | Estimated trip fare |
| final_fare_inr | double | No | 265.50 | Final fare charged |
| surge_multiplier | double | Yes | 1.5 | Dynamic pricing multiplier |
---

## 3. Raw File Schema: drivers.json

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| driver_id | string | Yes | DRV-000001 | Unique driver identifier |
| home_zone_id | string | Yes | ZON-999 | Driver's assigned home zone |
| onboard_date | timestamp | Yes | 2024-03-15T00:00:00.000 | Driver onboarding date |
| vehicle_type | string | Yes | Sedan | Vehicle category |
| service_type | string | Yes | Sedan | Type of service offered |
| driver_status | string | Yes | Active | Current driver status |
| rating | double | Yes | 3.96 | Driver rating score |
| lifetime_completed_trips | integer | Yes | 2135 | Total completed trips |
| last_status_update_ts | timestamp | Yes | 2026-01-28T15:05:00.000Z | Last driver status update timestamp |
| source_record_version | integer | Yes | 1 | Source record version |

---



## 4. Raw File Schema: payments.csv

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| payment_id | string | Yes | PAY-00000001 | Unique payment identifier |
| trip_id | string | Yes | TRP-2026XXXX | Associated trip identifier |
| attempt_number | integer | Yes | 1 | Payment attempt number |
| payment_ts | timestamp | Yes | 2026-01-01 18:25:00 | Payment transaction timestamp |
| payment_method | string | Yes | UPI | Payment method used |
| payment_status | string | Yes | Success | Status of payment transaction |
| amount_inr | double | Yes | 184.95 | Transaction amount |
| failure_reason | string | No | Timeout | Failure reason (if any) |
| is_final_attempt | boolean | Yes | TRUE | Indicates final payment attempt |
| payment_reference | string | Yes | TPREF-00000001 | Unique payment reference number |


## 4. Reference File Schema: zones.csv

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| zone_id | string | Yes | ZON-001 | Unique zone identifier |
| zone_name | string | Yes | TPC Zone | Name of the fictional city zone |
| zone_type | string | Yes | Residential | Zone classification |
| city_code | string | Yes | TPC | Fictional city code |
| demand_band | string | Yes | Low | Demand category |
| is_active | boolean | Yes | TRUE | Indicates whether the zone is active |
| effective_from | timestamp | Yes | 2026-01-01 | Date from which the zone definition is effective |

---

## 5. Canonical Silver Table Design

| Silver Field | Type | Source Mapping | Business Meaning |
|--------------|------|----------------|------------------|
| trip_id | string | trips.trip_id | Unique trip identifier |
| request_date | date | date(trips.request_ts) | Date used for analytics and reporting |
| driver_id | string | trips.driver_id | Assigned driver's identifier |
| pickup_zone_name | string | Join zones on pickup_zone_id | Pickup location of the trip |
| dropoff_zone_name | string | Join zones on dropoff_zone_id | Destination location of the trip |
| service_type | string | trips.service_type | Type of ride requested |
| trip_status | string | upper(trips.trip_status) | Standardized trip status |
| estimated_distance_km | double | trips.estimated_distance_km | Estimated travel distance |
| actual_distance_km | double | trips.actual_distance_km | Actual completed travel distance |
| estimated_fare_inr | double | trips.estimated_fare_inr | Estimated trip fare |
| final_fare_inr | double | trips.final_fare_inr | Final fare charged to the customer |
| payment_status | string | Join payments on trip_id | Status of payment transaction |
| surge_multiplier | double | trips.surge_multiplier | Dynamic pricing factor |
| driver_rating | double | Join drivers on driver_id | Driver's latest rating |
| is_completed_trip | boolean | Derived from trip_status | Indicates whether the trip was successfully completed |


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
