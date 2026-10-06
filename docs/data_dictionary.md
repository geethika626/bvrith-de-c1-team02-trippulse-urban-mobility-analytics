# Data Dictionary

Week: 2  
**Purpose:** Define the approved raw, reference, Silver Candidate, and streaming fields used in the TripPulse Urban Mobility Analytics project.

> **Important:** The source datasets are fictional/synthetic. No real passenger, driver, payment-account, licence, phone, or other real-person identity information is represented.

---

# 1. Source File Catalog

| File Name | Format | Grain | Primary Key | Purpose |
|---|---|---|---|---|
| `zones.csv` | CSV | One row per fictional zone | `zone_id` | Zone reference/master data |
| `drivers.json` | JSON | One row per fictional driver snapshot | `driver_id` | Driver reference/master data |
| `trips.parquet` | Parquet | One row per trip request at final batch-known state | `trip_id` | Central ride-request and trip lifecycle data |
| `payments.csv` | CSV | One row per payment attempt | `payment_id` | Payment transaction attempts |
| `ride_request_event_drop_01.json` | JSON Lines | One row per incremental lifecycle event | `event_id` | Streaming event drop 01 |
| `ride_request_event_drop_02.json` | JSON Lines | One row per incremental lifecycle event | `event_id` | Streaming event drop 02 |

---

# 2. Source Grain and Relationship Contract

| Source | Grain | Primary Key | Foreign Keys |
|---|---|---|---|
| `zones.csv` | One row per fictional zone | `zone_id` | None |
| `drivers.json` | One row per fictional driver snapshot | `driver_id` | `home_zone_id → zones.zone_id` |
| `trips.parquet` | One row per ride request at final batch-known state | `trip_id` | `driver_id → drivers.driver_id`; `pickup_zone_id → zones.zone_id`; `dropoff_zone_id → zones.zone_id` |
| `payments.csv` | One row per payment attempt | `payment_id` | `trip_id → trips.trip_id` |
| Event drops | One row per incremental lifecycle transition | `event_id` | `trip_id`, `driver_id`, zone references |

## Relationship Rules

- One zone can have many drivers.
- One zone can be the pickup zone for many trips.
- One zone can be the drop-off zone for many trips.
- One driver can be associated with many trips.
- A trip may have zero to three payment attempts.
- Every payment attempt must resolve to a valid trip.
- A driver reference is optional for trips before driver assignment.
- **Trip is the central analytical request grain.**
- `PaymentAttempt` and `RideRequestEvent` must not be counted as trip requests.

---

# 3. Raw File Schema — `zones.csv`

## Grain

**One row per fictional operating zone.**

| Field | Data Type | Required? | Key / Role | Business Meaning | Example |
|---|---|---|---|---|---|
| `zone_id` | string | Yes | PK | Fictional operating-zone identifier | `ZON-001` |
| `zone_name` | string | Yes | Attribute | Human-readable fictional zone label | `Madhapur Central` |
| `zone_type` | string | Yes | Attribute | Operating context of the zone | `commercial` |
| `city_code` | string | Yes | Business key part | Code for the fictional TripPulse city | `TPC` |
| `demand_band` | string | Yes | Attribute | Planning band used to shape synthetic request frequency | `high` |
| `is_active` | boolean | Yes | Attribute | Whether the zone can receive new generated activity | `true` |
| `effective_from` | date | Yes | Attribute | Date from which the zone definition applies | `2026-01-01` |

## DQ Expectations

| Field | Validation |
|---|---|
| `zone_id` | Pattern `ZON-[0-9]{3}`, unique |
| `zone_name` | 1–80 characters; unique within `city_code` |
| `zone_type` | `residential`, `commercial`, `transit_hub`, `education`, `mixed_use`, `airport` |
| `city_code` | `TPC` in the pilot baseline |
| `demand_band` | `low`, `medium`, `high` |
| `is_active` | Boolean |
| `effective_from` | Within approved source date range |

---

# 4. Raw File Schema — `drivers.json`

## Grain

**One row per fictional driver snapshot.**

No driver name, phone number, licence number, or real identity attribute is included.

| Field | Data Type | Required? | Key / Role | Business Meaning | Example |
|---|---|---|---|---|---|
| `driver_id` | string | Yes | PK | Fictional driver identifier | `DRV-000001` |
| `home_zone_id` | string | Yes | FK | Primary fictional operating zone | `ZON-024` |
| `onboard_date` | date | Yes | Attribute | Synthetic driver onboarding date | `2025-06-15` |
| `vehicle_type` | string | Yes | Attribute | Vehicle category | `bike` |
| `service_type` | string | Yes | Attribute | Primary service capability | `bike_taxi` |
| `driver_status` | string | Yes | Attribute | Driver operating status | `active` |
| `rating` | decimal(3,2) | Optional | Measure | Synthetic driver quality score | `4.62` |
| `lifetime_completed_trips` | integer | Yes | Measure | Synthetic completed-trip count | `1824` |
| `last_status_update_ts` | timestamp | Yes | Attribute | Latest driver status update timestamp | `2026-03-30T18:45:00+05:30` |
| `source_record_version` | integer | Yes | Technical attribute | Source snapshot version | `1` |

## DQ Expectations

| Field | Validation |
|---|---|
| `driver_id` | Unique; valid driver identifier |
| `home_zone_id` | Must resolve to `zones.zone_id` |
| `onboard_date` | Valid date |
| `vehicle_type` | Controlled vehicle category |
| `service_type` | Must be compatible with vehicle type |
| `driver_status` | Controlled status |
| `rating` | Valid range; optional |
| `lifetime_completed_trips` | Non-negative integer |
| `last_status_update_ts` | Valid timestamp |
| `source_record_version` | Positive/integer version |

---

# 5. Raw File Schema — `trips.parquet`

## Grain

**One row per ride request at its final batch-known state.**

This is the **central analytical request grain**.

| Field | Data Type | Required? | Key / Role | Business Meaning | Example |
|---|---|---|---|---|---|
| `trip_id` | string | Yes | PK | Unique trip request identifier | `TRP-20260214-000321` |
| `request_ts` | timestamp | Yes | Attribute | Time the ride was requested | `2026-02-14 08:17:22` |
| `driver_accept_ts` | timestamp | Optional | Attribute | Time driver accepted the request | `2026-02-14 08:19:00` |
| `pickup_ts` | timestamp | Optional | Attribute | Time passenger was picked up | `2026-02-14 08:22:00` |
| `dropoff_ts` | timestamp | Optional | Attribute | Time trip was completed | `2026-02-14 08:45:00` |
| `cancel_ts` | timestamp | Optional | Attribute | Time request was cancelled | `2026-02-14 08:20:00` |
| `driver_id` | string | Optional | FK | Assigned driver | `DRV-000847` |
| `pickup_zone_id` | string | Yes | FK | Pickup zone | `ZON-024` |
| `dropoff_zone_id` | string | Yes | FK | Drop-off zone | `ZON-078` |
| `service_type` | string | Yes | Attribute | Ride service category | `auto` |
| `trip_status` | string | Yes | Attribute | Final trip lifecycle status | `completed` |
| `cancellation_reason` | string | Optional | Attribute | Reason for cancellation | `rider_changed_plan` |
| `estimated_distance_km` | decimal(7,2) | Yes | Measure | Estimated route distance | `12.40` |
| `actual_distance_km` | decimal(7,2) | Optional | Measure | Actual travelled distance | `13.10` |
| `estimated_fare_inr` | decimal(10,2) | Yes | Measure | Estimated fare | `284.50` |
| `final_fare_inr` | decimal(10,2) | Optional | Measure | Final charged fare | `301.00` |
| `surge_multiplier` | decimal(4,2) | Yes | Measure | Demand multiplier at request time | `1.30` |
| `record_created_ts` | timestamp | Yes | Technical attribute | Generator batch-record creation timestamp | `2026-04-01T01:00:00+05:30` |

## Trip Lifecycle Rules

| Field | DQ Expectation |
|---|---|
| `trip_id` | Unique; approved `TRP-YYYYMMDD-NNNNNN` pattern |
| `request_ts` | Within approved source date window |
| `driver_accept_ts` | Null for unfulfilled/certain cancellations; otherwise >= `request_ts` |
| `pickup_ts` | Required for completed trips; >= `driver_accept_ts` |
| `dropoff_ts` | Required only for completed trips; > `pickup_ts` |
| `cancel_ts` | Required for cancelled trips; >= `request_ts`; null for completed |
| `driver_id` | Optional, but must resolve when present |
| `pickup_zone_id` | Must resolve to an active zone |
| `dropoff_zone_id` | Must resolve to an active zone |
| `service_type` | `bike_taxi`, `auto`, `mini`, `sedan` |
| `trip_status` | `completed`, `cancelled_by_rider`, `cancelled_by_driver`, `unfulfilled` |
| `cancellation_reason` | Controlled reason; null for completed |
| `estimated_distance_km` | `0.3–80.0 km` |
| `actual_distance_km` | `0.3–100.0 km` for completed trips |
| `estimated_fare_inr` | `₹20–₹5,000` |
| `final_fare_inr` | `₹20–₹6,000` for completed trips |
| `surge_multiplier` | `1.00–3.00` |
| `record_created_ts` | At/after source lifecycle timestamps |

---

# 6. Raw File Schema — `payments.csv`

## Grain

**One row per payment attempt.**

A single trip can have multiple payment attempts.

| Field | Data Type | Required? | Key / Role | Business Meaning | Example |
|---|---|---|---|---|---|
| `payment_id` | string | Yes | PK | Deterministic payment-attempt identifier | `PAY-000000321` |
| `trip_id` | string | Yes | FK | Trip linked to the payment attempt | `TRP-20260214-000321` |
| `attempt_number` | integer | Yes | Business key part | Sequential payment attempt number | `1` |
| `payment_ts` | timestamp | Yes | Attribute | Time payment attempt occurred | `2026-02-14T08:52:05+05:30` |
| `payment_method` | string | Yes | Attribute | Synthetic payment channel | `upi` |
| `payment_status` | string | Yes | Attribute | Payment outcome | `success` |
| `amount_inr` | decimal(10,2) | Yes | Measure | Payment attempt amount | `301.00` |
| `failure_reason` | string | Optional | Attribute | Reason for failed payment | `bank_declined` |
| `is_final_attempt` | boolean | Yes | Attribute | Indicates final known payment attempt | `true` |
| `payment_reference` | string | Yes | Attribute | Opaque synthetic payment reference | `TPREF-9B7D12A1` |

## Payment DQ Expectations

| Field | Validation |
|---|---|
| `payment_id` | Unique; pattern `PAY-[0-9]{9}` |
| `trip_id` | Must resolve to `trips.trip_id` |
| `attempt_number` | 1–3; unique with `trip_id` |
| `payment_ts` | Must be logically after trip request |
| `payment_method` | `upi`, `card`, `wallet`, `cash` |
| `payment_status` | `success`, `failed`, `pending`, `refunded` |
| `amount_inr` | `₹0–₹6,000` |
| `failure_reason` | Controlled reason; required for failed attempts |
| `is_final_attempt` | Exactly one final attempt per trip with attempts |
| `payment_reference` | Pattern `TPREF-[A-F0-9]{8}`; unique |

---

## 6. Streaming Event Schema

The TripPulse streaming data consists of ride lifecycle events stored in JSON event-drop files.

### Streaming Source Files

| File | Description |
|---|---|
| `ride_request_event_drop_01.json` | Streaming ride lifecycle event drop |
| `ride_request_event_drop_02.json` | Streaming ride lifecycle event drop |

### 6.1 Ride Request Event Schema

| Field | Data Type | Description |
|---|---|---|
| `event_id` | STRING | ## 6. Streaming Event Schema

The TripPulse streaming data consists of ride lifecycle events stored in JSON event-drop files.

### Streaming Source Files

| File | Description |
|---|---|
| `ride_request_event_drop_01.json` | Streaming ride lifecycle event drop |
| `ride_request_event_drop_02.json` | Streaming ride lifecycle event drop |

### 6.1 Ride Request Event Schema

| Field | Data Type | Description |
|---|---|---|
| `event_id` | STRING | ## 6. Streaming Event Schema

The TripPulse streaming data consists of ride lifecycle events stored in JSON event-drop files.

### Streaming Source Files

| File | Description |
|---|---|
| `ride_request_event_drop_01.json` | Streaming ride lifecycle event drop |
| `ride_request_event_drop_02.json` | Streaming ride lifecycle event drop |

### 6.1 Ride Request Event Schema

| Field | Data Type | Description |
|---|---|---|
| `event_id` | STRING | Unique identifier for the streaming event |
| `schema_version` | STRING | Version of the streaming event schema |
| `event_ts` | TIMESTAMP | Event timestamp |
| `event_type` | STRING | Type of ride lifecycle event |
| `trip_id` | STRING | Identifier of the associated trip |
| `driver_id` | STRING | Identifier of the associated driver; may be null for unfulfilled rides |
| `pickup_zone_id` | STRING | Pickup zone identifier |
| `dropoff_zone_id` | STRING | Drop-off zone identifier |
| `service_type` | STRING | Type of ride service |
| `status_from` | STRING | Previous status before the event; null for an initial `ride_requested` event |
| `status_to` | STRING | New status after the event |
| `surge_multiplier` | DECIMAL | Surge multiplier associated with the ride |
| `estimated_fare_inr` | DECIMAL | Estimated fare in INR |
| `producer_run_id` | STRING | Identifier of the producer/data-generation run |
| `event_sequence_no` | INTEGER | Sequence number of the event within the trip lifecycle |
| `unexpected_field` | STRING / NULL | Field used in the supplied data for schema-drift testing |

### 6.2 Example Event Lifecycle

The streaming data contains different ride lifecycle events.

A completed ride can follow this sequence:

```text
ride_requested
        ↓
driver_assigned
        ↓
driver_accepted
        ↓
pickup_started
        ↓
ride_completed
        ↓
payment_confirmed |
| `schema_version` | STRING | Version of the streaming event schema |
| `event_ts` | TIMESTAMP | Event timestamp |
| `event_type` | STRING | Type of ride lifecycle event |
| `trip_id` | STRING | Identifier of the associated trip |
| `driver_id` | STRING | Identifier of the associated driver; may be null for unfulfilled rides |
| `pickup_zone_id` | STRING | Pickup zone identifier |
| `dropoff_zone_id` | STRING | Drop-off zone identifier |
| `service_type` | STRING | Type of ride service |
| `status_from` | STRING | Previous status before the event; null for an initial `ride_requested` event |
| `status_to` | STRING | New status after the event |
| `surge_multiplier` | DECIMAL | Surge multiplier associated with the ride |
| `estimated_fare_inr` | DECIMAL | Estimated fare in INR |
| `producer_run_id` | STRING | Identifier of the producer/data-generation run |
| `event_sequence_no` | INTEGER | Sequence number of the event within the trip lifecycle |
| `unexpected_field` | STRING / NULL | Field used in the supplied data for schema-drift testing |

### 6.2 Example Event Lifecycle

The streaming data contains different ride lifecycle events.

A completed ride can follow this sequence:

```text
ride_requested
        ↓
driver_assigned
        ↓
driver_accepted
        ↓
pickup_started
        ↓
ride_completed
        ↓
payment_confirmed |
| `schema_version` | STRING | Version of the streaming event schema |
| `event_ts` | TIMESTAMP | Event timestamp |
| `event_type` | STRING | Type of ride lifecycle event |
| `trip_id` | STRING | Identifier of the associated trip |
| `driver_id` | STRING | Identifier of the associated driver; may be null for unfulfilled rides |
| `pickup_zone_id` | STRING | Pickup zone identifier |
| `dropoff_zone_id` | STRING | Drop-off zone identifier |
| `service_type` | STRING | Type of ride service |
| `status_from` | STRING | Previous status before the event; null for an initial `ride_requested` event |
| `status_to` | STRING | New status after the event |
| `surge_multiplier` | DECIMAL | Surge multiplier associated with the ride |
| `estimated_fare_inr` | DECIMAL | Estimated fare in INR |
| `producer_run_id` | STRING | Identifier of the producer/data-generation run |
| `event_sequence_no` | INTEGER | Sequence number of the event within the trip lifecycle |
| `unexpected_field` | STRING / NULL | Field used in the supplied data for schema-drift testing |

### 6.2 Example Event Lifecycle

The streaming data contains different ride lifecycle events.

A completed ride can follow this sequence:

```text
ride_requested
        ↓
driver_assigned
        ↓
driver_accepted
        ↓
pickup_started
        ↓
ride_completed
        ↓
payment_confirmed

### Grain

**One row per incremental ride lifecycle transition.**

Both event drops follow the same event contract.

| Field | Data Type | Required? | Key / Role | Business Meaning | Example |
|---|---|---|---|---|---|
| `event_id` | string | Yes | PK | Unique event identifier | `EVT-20260401-000001` |
| `schema_version` | string | Yes | Technical attribute | Event contract version | `1.0` |
| `event_ts` | timestamp | Yes | Event-time attribute | Time event occurred | `2026-03-31T18:43:10Z` |
| `event_type` | string | Yes | Attribute | Lifecycle event type | `ride_requested` |
| `trip_id` | string | Yes | FK | Associated trip | `TRP-20260331-000355` |
| `driver_id` | string | Yes | Reference | Associated driver | `DRV-000815` |
| `pickup_zone_id` | string | Yes | Reference | Pickup zone | `ZON-023` |
| `dropoff_zone_id` | string | Yes | Reference | Drop-off zone | `ZON-119` |
| `service_type` | string | Yes | Attribute | Ride service category | `mini` |
| `status_from` | string | Optional | Lifecycle attribute | Previous lifecycle state | `requested` |
| `status_to` | string | Yes | Lifecycle attribute | New lifecycle state | `assigned` |
| `surge_multiplier` | decimal(4,2) | Yes | Measure | Demand multiplier | `1.50` |
| `estimated_fare_inr` | decimal(10,2) | Yes | Measure | Estimated fare | `664.74` |
| `producer_run_id` | string | Yes | Technical attribute | Event producer/run identifier | `P02-TRIPPULSE-SEED4202-V1` |
| `event_sequence_no` | integer | Yes | Sequence attribute | Event sequence within trip | `2` |

## Streaming Rules

- `event_id` must be unique.
- `schema_version` must match the approved event contract.
- `event_ts` is the event-time field.
- `trip_id` identifies the associated ride request.
- `event_sequence_no` preserves lifecycle ordering within a trip.
- `status_from → status_to` represents the lifecycle transition.
- Event records must not be counted as independent trip requests.
- Event drops must support idempotent processing.

---

# 8. Key Relationships

```text
                    ┌──────────────┐
                    │    ZONES     │
                    │   zone_id PK │
                    └──────┬───────┘
                           │
              ┌────────────┴─────────────┐
              │                          │
              ▼                          ▼
      ┌──────────────┐           ┌──────────────┐
      │   DRIVERS    │           │    TRIPS     │
      │ driver_id PK │──────────►│  trip_id PK  │
      │ home_zone FK │           │ driver_id FK │
      └──────────────┘           │ pickup_zone  │
                                 │ dropoff_zone │
                                 └──────┬───────┘
                                        │
                                        ▼
                                ┌────────────────┐
                                │    PAYMENTS    │
                                │ payment_id PK  │
                                │ trip_id FK     │
                                │ attempt_number │
                                └────────────────┘

                         ┌──────────────────────┐
                         │ RIDE REQUEST EVENTS  │
                         │ event_id PK          │
                         │ trip_id              │
                         │ driver_id            │
                         └──────────────────────┘
