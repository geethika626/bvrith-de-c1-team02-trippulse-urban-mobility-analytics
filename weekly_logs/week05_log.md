# Week 05 Log — Silver Candidate Transformation

**Week:** 5  
**Date range:** 7th August 2026 - 13th August 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse — Urban Mobility Analytics

---

## 1. Sprint Goal

The goal of Week 5 was to transform the validated TripPulse Bronze Delta tables into Silver Candidate tables for zones, drivers, trips, and payments. The work focused on preserving Bronze-to-Candidate lineage and record coverage, applying appropriate type and field transformations, creating trip-level derived fields, preserving dataset grain, and validating null, lifecycle, and schema behaviour before the Week 6 Data Quality stage.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Created Silver Candidate tables for Zones, Drivers, Trips and Payments | Team | Done | `screenshots/week05_01_candidate_tables.png` |
| Reconciled Bronze row counts with Silver Candidate row counts | Team | Done | `screenshots/week05_02_bronze_candidate_counts.png` |
| Validated Bronze-to-Candidate lineage preservation | Team | Done | `screenshots/week05_03_lineage_preservation.png` |
| Created and validated Trip derived fields such as response, wait and trip-duration measures | Team | Done | `screenshots/week05_04_trip_derivations.png` |
| Validated Payment Candidate grain and payment-key behaviour | Team | Done | `screenshots/week05_05_payment_grain.png` |
| Checked null lifecycle fields and negative duration/response/wait conditions | Team | Done | `screenshots/week05_06_null_lifecycle.png` |
| Compared a Bronze Trip record with its Silver Candidate representation and derived fields | Team | Done | `screenshots/week05_07_before_after.png` |
| Inspected the Zones Silver Candidate schema | Team | Done | `screenshots/week05_08_zones_candidate_schema.png` |
| Inspected the Drivers Silver Candidate schema | Team | Done | `screenshots/week05_09_drivers_candidate_schema.png` |
| Documented the Silver Candidate transformation workflow | Team | Done | `notebooks/03_silver_transformations.ipynb` |

---

## 3. Key Decisions

- Used the four Week 4 Bronze Delta tables as the inputs for the Silver Candidate transformations.
- Created separate Silver Candidate tables for:
  - Zones
  - Drivers
  - Trips
  - Payments
- Kept the Silver Candidate layer separate from Trusted Silver. Data Quality, quarantine and Trusted Silver routing were reserved for Week 6.
- Preserved Bronze-to-Candidate lineage so that Candidate records can be traced back to their Bronze source records.
- Preserved Candidate row coverage during the Bronze-to-Candidate transformation and explicitly reconciled Bronze and Candidate counts.
- Created Trip-level derived fields including response time, wait time and trip duration where the required timestamps were available.
- Preserved null lifecycle fields where they are meaningful for the Trip lifecycle rather than treating every null as a transformation failure.
- Validated that negative response, wait and trip-duration values are identifiable for downstream Data Quality handling.
- Treated Payments as a separate-grain dataset because a trip can have multiple payment attempts.
- Validated payment identifiers and physical-versus-distinct row behaviour instead of assuming one payment row per trip.
- Applied the required Silver Candidate transformations without performing the Trusted Silver DQ/quarantine decisions that belong to Week 6.

---

## 4. Blockers / Risks

| Blocker / Risk | Impact | Resolution / Handling |
|---|---|---|
| Trips contain multiple lifecycle timestamps, including nullable cancellation/drop-off fields | Incorrect handling could create invalid derived durations or lifecycle information | Preserved lifecycle fields and explicitly checked null and negative duration conditions |
| Trips and Payments have different grains | Direct assumptions about one payment per trip could produce incorrect metrics | Validated Payment Candidate grain using payment IDs and physical-versus-distinct row checks |
| Candidate transformations must preserve Bronze lineage | Loss of source identifiers would make downstream DQ tracing difficult | Validated Bronze-to-Candidate lineage fields and record mapping |
| Silver Candidate is not the same as Trusted Silver | Applying DQ/quarantine decisions too early would mix Week 5 and Week 6 responsibilities | Kept Candidate transformations separate from Week 6 Data Quality processing |

---

## 5. Evidence Added to GitHub

### Notebook

- `notebooks/03_silver_transformations.ipynb`

### Screenshots

- `screenshots/week05_01_candidate_tables.png` — Silver Candidate tables created for the four TripPulse datasets
- `screenshots/week05_02_bronze_candidate_counts.png` — Bronze-to-Candidate row-count reconciliation
- `screenshots/week05_03_lineage_preservation.png` — Candidate lineage preservation validation
- `screenshots/week05_04_trip_derivations.png` — Trip derived-field validation
- `screenshots/week05_05_payment_grain.png` — Payment Candidate grain validation
- `screenshots/week05_06_null_lifecycle.png` — lifecycle null and negative-value validation
- `screenshots/week05_07_before_after.png` — Bronze-to-Candidate record comparison
- `screenshots/week05_08_zones_candidate_schema.png` — Zones Candidate schema
- `screenshots/week05_09_drivers_candidate_schema.png` — Drivers Candidate schema
- screenshots/week05_10_trips_candidate_schema.png — Trips Candidate schema
- screenshots/week05_11_payments_candidate_schema.png — Payments Candidate schema
- screenshots/week05_12_final_validation.png — Final Validation of Silver Candidate tables

### Weekly Log

- `weekly_logs/week05_log.md`

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to explain Silver Candidate transformation patterns, Bronze-to-Candidate reconciliation, lineage preservation, Trip derived-field calculations, Payment grain validation, lifecycle checks and schema validation. |
| What we changed after AI suggestion | The transformation and validation logic was adapted to the actual TripPulse Bronze schemas, business grains, lifecycle fields, payment-attempt structure and project-specific lineage requirements. |
| What we verified manually | Candidate table creation, Bronze-to-Candidate row counts, lineage fields, Trip derived values, Payment grain, lifecycle null conditions, Bronze-to-Candidate record comparison, and Candidate schemas were checked in Databricks. |
| What we can explain without AI | We can explain why Silver Candidate is created after Bronze, how Candidate transformations preserve lineage and record coverage, why Trip lifecycle timestamps require careful handling, why Payments have a different grain from Trips, and why Trusted Silver and quarantine decisions belong to the following Data Quality stage. |

---

## 7. Next Week Preparation

- Use the four Silver Candidate tables as inputs for Week 6 Data Quality processing.
- Apply the approved Data Quality rules to Zones, Drivers, Trips and Payments.
- Identify records that should be routed to Trusted Silver or Quarantine.
- Prepare entity-level reconciliation showing Candidate = Trusted Silver + Quarantine.
- Preserve Bronze and Silver lineage information so failed records can be traced and corrected.
- Prepare controlled rework and replay evidence for records that can be corrected and revalidated.
