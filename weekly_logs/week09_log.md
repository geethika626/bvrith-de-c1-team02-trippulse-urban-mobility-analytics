# Week 09 Log — Dashboard Completion and Insights

**Week:** 9  
**Date range:** 18 September 2026 - 25 September 2026  
**Team:** Data Nexus / 02  
**Project:** TripPulse — Urban Mobility Analytics

---

## 1. Sprint Goal

Complete the three approved Power BI dashboard pages using validated Gold outputs only.  
Validate filters, interactions, cross-page consistency and selected KPI values against matching Gold queries, and document evidence-backed insights and limitations.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Completed PBI-01 Ride Operations Overview | Team | Done | Power BI dashboard screenshot |
| Completed PBI-02 Zone Demand and Surge | Team | Done | Power BI dashboard screenshot |
| Completed PBI-03 Driver and Payment Reliability | Team | Done | Power BI dashboard screenshot |
| Implemented approved KPI cards and visuals | Team | Done | Final PBIX |
| Added required filters and slicers | Team | Done | Final PBIX |
| Tested PBI-01 filters and interactions | Team | Done | Dashboard validation |
| Tested PBI-02 filters and interactions | Team | Done | Dashboard validation |
| Tested PBI-03 filters and interactions | Team | Done | Dashboard validation |
| Verified cleared/default filter state | Team | Done | Dashboard validation |
| Checked cross-page KPI consistency | Team | Done | PBI-01/PBI-02 validation |
| Reconciled PBI-01 selected KPIs with Gold | Team | Done | Gold query evidence |
| Reconciled PBI-02 selected KPIs with Gold | Team | Done | Gold query evidence |
| Reconciled PBI-03 driver slice with Gold | Team | Done | Driver slice reconciliation |
| Reconciled PBI-03 payment-method slice with Gold | Team | Done | Payment reconciliation |
| Added evidence-backed dashboard insights | Team | Done | `docs/dashboard_insights.md` |
| Documented synthetic-data limitations | Team | Done | `docs/dashboard_insights.md` |
| Updated dashboard README | Team | Done | `dashboard/README.md` |
| Updated Week 09 Power BI export notebook | Team | Done | `notebooks/06_powerbi_export.ipynb` |
| Prepared final dashboard screenshots | Team | Done | `screenshots/` |
| Final PBIX maintained separately because of file-size limit | Team | Done | Separate PBIX file |

---

## 3. Key Decisions

- Power BI reporting uses validated Gold outputs only; raw, Bronze, Silver Candidate and quarantine layers are not used as reporting sources.
- Week 09 contains exactly three approved dashboard pages: PBI-01, PBI-02 and PBI-03.
- Streaming event reporting is not included in Week 09 and is reserved for the controlled streaming simulation planned for Week 10.
- PBI-02 surge trip share was implemented using the Gold `surge_trip_count` and `trip_requests` values rather than directly summing the pre-calculated percentage column.
- PBI-03 driver and payment metrics were kept at their respective grains: driver-performance metrics use driver/trip information, while payment success is measured at payment-attempt level.
- Driver metrics are treated as fictional educational measures and are not presented as real worker evaluations.
- Surge and fulfilment patterns are reported as observed patterns in synthetic data and are not treated as causal relationships.
- The final PBIX is stored separately because it exceeds the GitHub repository file-size limit.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| Final PBIX file exceeds the GitHub repository file-size limit | PBIX cannot be stored directly in the repository | PBIX stored separately for mentor review |
| Synthetic dataset | Dashboard observations cannot be interpreted as real-world operational or business conclusions | Documented limitation in dashboard insights |
| Payment metrics use payment-attempt grain | Payment success should not be interpreted as overall customer payment behaviour | Documented in dashboard insights |
| Driver metrics are fictional | Driver metrics should not be interpreted as real worker evaluation | Documented in dashboard insights |

---

## 5. Evidence Added to GitHub

- `docs/dashboard_insights.md` updated with dashboard insights, Gold traceability, validation and reconciliation evidence.
- `dashboard/README.md` updated with PBIX storage information and KPI reconciliation results.
- `notebooks/06_powerbi_export.ipynb` updated for the Week 09 Power BI Gold export.
- Final PBI-01 dashboard screenshot added to `screenshots/`.
- Final PBI-02 dashboard screenshot added to `screenshots/`.
- Final PBI-03 dashboard screenshot added to `screenshots/`.
- PBI-02 KPI reconciliation evidence added.
- PBI-03 driver-slice reconciliation evidence added.
- PBI-03 payment-method reconciliation evidence added.
- PBI-01 reconciliation evidence was already captured during Week 08.
- Final PBIX is stored separately because of the file-size limit.

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to assist with Power BI measure formulation, dashboard structure, troubleshooting, documentation and reconciliation-query preparation. |
| What we changed after AI suggestion | DAX measures and dashboard calculations were adjusted based on the actual Gold table schema and the behaviour observed in Power BI. |
| What we verified manually | Gold query results, Power BI KPI values, filters, slicers, visual behaviour, cross-page consistency and reconciliation results were manually checked. |
| What we can explain without AI | The team can explain the Gold tables, KPI definitions, dashboard pages, filters, DAX measures, reconciliation process, validation results and synthetic-data limitations. |

---

## 7. Next Week Preparation

- Prepare for Week 10 controlled streaming simulation.
- Review the approved Streaming Event Design before implementing streaming-related work.
- Keep streaming work separate from the completed Week 09 batch Gold model.
- Prepare the required environment and notebooks for controlled streaming simulation.
