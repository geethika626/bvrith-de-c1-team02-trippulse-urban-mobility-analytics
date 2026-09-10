# Week 07 Log — Gold Model, KPI Implementation and Reconciliation

**Week:** 7  
**Date range:** 04th September 2026 - 10th September 2026  
**Team:** Data Nexus / 02  
**Project:** TripPulse — Urban Mobility Analytics

---

## 1. Sprint Goal

This week focused on building the TripPulse Gold layer from Trusted Silver data.

The main work included creating the Gold dimensions and facts, building the five
approved daily summary tables, implementing KPI-01 through KPI-10, and validating
the Gold layer through uniqueness, referential integrity and reconciliation checks.

Gold was built using Trusted Silver only, without directly using Bronze, Silver
Candidate or Quarantine tables.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Build `dim_date` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `dim_zone` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `dim_driver` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `fact_trip` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `fact_payment_attempt` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `agg_trip_operations_daily` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `agg_zone_demand_daily` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `agg_driver_performance_daily` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `agg_surge_impact_daily` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Build `agg_payment_reliability_daily` | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Implement KPI-01 through KPI-10 | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Validate dimension and fact uniqueness | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Validate referential integrity | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Reconcile Gold facts to Trusted Silver | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Reconcile summaries to governing facts | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Perform manual KPI spot checks | Team | Done | `notebooks/05_gold_aggregations.ipynb` |
| Update Gold metric definitions | Team | Done | `docs/gold_metrics_definition.md` |

---

## 3. Key Decisions

- Gold was built only from the Week-06 Trusted Silver tables.
- The Gold model keeps dimensions, trip facts and payment-attempt facts at
  separate grains.
- `fact_trip` remains at one row per trip request.
- `fact_payment_attempt` remains at one row per payment attempt.
- Payment attempts are not allowed to multiply trip-level KPI counts.
- Batch summaries are calculated from their respective governing facts.
- KPI rates use the approved numerator and denominator definitions.
- Zero denominators return `NULL` rather than `0%`.
- Fact-to-Trusted-Silver and summary-to-fact reconciliation checks were used
  before treating the Gold layer as complete.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| No unresolved blocker documented after the Gold validation and reconciliation checks | None | None |

**Risk considered:** Joining payment attempts directly to trip-level data could
multiply trip rows and produce incorrect KPIs. This was controlled by keeping
`fact_trip` and `fact_payment_attempt` at their approved grains and calculating
trip-level metrics independently.

---

## 5. Evidence Added to GitHub

- `notebooks/05_gold_aggregations.ipynb` updated with Gold table creation,
  KPI logic, uniqueness checks, referential-integrity checks and reconciliation
  queries.
- `docs/gold_metrics_definition.md` updated with Gold grains, keys, source
  lineage, KPI formulas and zero-denominator behaviour.
- Useful Gold validation/schema screenshots added under `screenshots/`.
- `weekly_logs/week07_log.md` updated with Week 7 work, decisions, validation,
  ownership and AI transparency.

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to assist with reviewing the Gold table structure, KPI implementation, reconciliation logic and documentation. |
| What we changed after AI suggestion | The suggestions were adapted to the TripPulse Week-7 Gold structure and the Trusted Silver tables used by the project. |
| What we verified manually | We manually checked Gold source tables, table grains, keys, joins, KPI formulas, zero-denominator handling, uniqueness, referential integrity, fact-to-Trusted-Silver reconciliation and summary-to-fact reconciliation. |
| What we can explain without AI | Each student can explain their assigned Gold work, including the relevant grain, keys, formulas, joins, validation checks and reconciliation results. |

---

## 7. Next Week Preparation

- Prepare the validated Gold outputs for the Power BI hand-off.
- Use Gold-only sources for the Power BI model and first report page.
- Prepare the Gold export/source register and PBI-01 reconciliation.
- Confirm that the Gold model is stable before dashboard development.
