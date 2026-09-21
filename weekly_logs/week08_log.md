# Week 08 Log — Gold Hand-off and Power BI Foundation

**Week:** 8  
**Date range:** 11th September 2026 - 18th September 2026   
**Team:** Data Nexus / 02  
**Project:** TripPulse Urban Mobility Analytics

---

## 1. Sprint Goal

Create validated student-owned Gold exports, establish the Power BI model using governed Gold outputs, and implement PBI-01 — Ride Operations Overview.

Validate the Gold-to-Power BI hand-off through schema, grain, relationship and selected KPI reconciliation checks.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Select approved Gold tables for Power BI | Team | Done | Week 08 Power BI export notebook |
| Create deterministic Gold export slices | Team | Done | `data_sample/gold_exports/` |
| Validate Gold export schema, columns and grain | Team | Done | Week 08 export validation notebook |
| Build Power BI Gold-only model | Team | Done | `dashboard/powerbi_dashboard.pbix` |
| Implement PBI-01 Ride Operations Overview | Team | Done | Power BI dashboard screenshot |
| Reconcile PBI-01 measures with Gold | Team | Done | Databricks Gold reconciliation queries |
| Update dashboard README | Team | Done | `dashboard/README.md` |

---

## 3. Key Decisions

- Use only governed Gold outputs for Power BI and avoid direct connections to raw, Bronze or Silver data.
- Use a deterministic 01-Jan-2026 to 14-Jan-2026 Gold slice for the PBI-01 dashboard.
- Use the approved PBI-01 decision question, KPI cards, visuals and filters from the TripPulse dashboard blueprint.
- Keep streaming/event data outside the Week 08 batch Power BI scope.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| No blocking issue remained after Gold-to-Power BI validation | No impact on Week 08 closure | None |

---

## 5. Evidence Added to GitHub

- `dashboard/README.md` updated with Power BI source and usage information.
- `notebooks/06_powerbi_export.ipynb` / Week 08 Gold export notebook prepared.
- Gold export files prepared under `data_sample/gold_exports/`.
- `dashboard/powerbi_dashboard.pbix` prepared for the Power BI dashboard.
- PBI-01 dashboard screenshot/evidence added to `screenshots/`.

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to assist with notebook structure, Power BI source mapping, validation logic, documentation wording and troubleshooting. |
| What we changed after AI suggestion | The team reviewed the suggestions and adapted the notebook, Power BI layout and documentation to match the approved TripPulse manual and the actual implementation. |
| What we verified manually | Gold table availability, export validation, Power BI values, selected Gold reconciliation results, dashboard filters and PBI-01 visual outputs were verified manually. |
| What we can explain without AI | The team can explain the Gold-to-Power BI hand-off, deterministic export process, Gold-only sourcing, Power BI model, PBI-01 decision question, filters, visuals and KPI reconciliation. |

---

## 7. Next Week Preparation

- Complete PBI-02 — Zone Demand and Surge.
- Complete PBI-03 — Driver and Payment Reliability.
- Reconcile selected KPIs and visual totals for the remaining dashboard pages.
- Document evidence-backed insights and limitations for all dashboard pages.
