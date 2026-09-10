# Week 06 Log — Data Quality

**Week:** 6  
**Date range:** 14th August 2026 - 21st August 2026  
**Team:** Data Nexus / Team02  
**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Sprint Goal

Implement and validate the Data Quality layer for the Silver Candidate data.

Apply Critical and Major DQ rules, route valid records to Trusted Silver and invalid records to Quarantine, and document the validation results for reliable downstream Gold analytics.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Implemented Data Quality checks for zones | Team | Done | `notebooks/04_data_quality_checks.ipynb` |
| Implemented Data Quality checks for drivers | Team | Done | `notebooks/04_data_quality_checks.ipynb` |
| Implemented Data Quality checks for trips | Team | Done | `notebooks/04_data_quality_checks.ipynb` |
| Implemented Data Quality checks for payments | Team | Done | `notebooks/04_data_quality_checks.ipynb` |
| Validated zone and driver reference integrity | Team | Done | DQ validation results |
| Validated trip key, reference and timestamp integrity | Team | Done | DQ validation results |
| Validated trip status, service, distance and fare/surge rules | Team | Done | DQ validation results |
| Validated payment key and reconciliation logic | Team | Done | DQ validation results |
| Created Trusted Silver and Quarantine outputs | Team | Done | DQ notebook and validation results |
| Reconciled Candidate, Trusted and Quarantine record counts | Team | Done | `docs/data_quality_summary.md` |
| Documented DQ failures and business impact | Team | Done | `docs/data_quality_summary.md` |

---

## 3. Key Decisions

- Applied separate Data Quality rules for zones, drivers, trips and payments instead of using a single generic validation rule.
- Classified DQ rules into **Critical** and **Major** severity levels based on their potential impact on downstream analytics.
- Records failing validation are routed to **Quarantine** instead of being silently deleted.
- Only records that pass the required DQ checks are allowed into **Trusted Silver** and subsequent Gold metric calculations.
- During payment validation, a one-to-many join caused duplicate payment rows. The logic was corrected to validate payment references against distinct trip IDs, restoring the expected one-to-one payment row boundary.
- Gold metrics were not created in Week 06 because the focus of this sprint was establishing governed Trusted Silver and Quarantine outputs.

---

## 4. Blockers / Risks

| Blocker / Risk | Impact | Resolution / Help Needed |
|---|---|---|
| Initial payment reconciliation issue caused by duplicate trip IDs in the join | Payment rows were temporarily multiplied during DQ processing | Corrected the validation join to use distinct trip IDs |
| Multiple DQ failures across trip and payment records | Invalid records could affect downstream KPIs if not quarantined | Records were routed to Quarantine and excluded from Trusted Silver |
| No other major blockers encountered | None | Not required |

---

## 5. Evidence Added to GitHub

- `notebooks/04_data_quality_checks.ipynb`
- `docs/data_quality_summary.md`
- DQ validation results for zones, drivers, trips and payments
- Trusted Silver and Quarantine outputs
- Entity-level reconciliation results

### Final Reconciliation

| Entity | Candidate | Trusted | Quarantine | Variance |
|---|---:|---:|---:|---:|
| Zones | 120 | 120 | 0 | 0 |
| Drivers | 2,800 | 2,793 | 7 | 0 |
| Trips | 250,875 | 241,654 | 9,221 | 0 |
| Payments | 180,315 | 177,635 | 2,680 | 0 |

All four entities reconciled with **zero variance**.

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI was used to assist with Data Quality rule structure, SQL logic, validation queries, debugging and documentation. |
| What we changed after AI suggestion | We reviewed and adapted the suggested logic to match the project's Silver Candidate schema, DQ requirements and Trusted/Quarantine workflow. |
| What we verified manually | DQ rule results, failed records, severity classifications, row counts, Trusted and Quarantine outputs, payment reconciliation and entity-level counts were manually verified. |
| What we can explain without AI | We can explain the purpose of each DQ rule, the Critical/Major classification, the Trusted Silver and Quarantine flow, reconciliation logic, and how invalid records are prevented from affecting Gold metrics. |

---

## 7. Next Week Preparation

- Begin creating Gold aggregation tables using only the governed Trusted Silver data.
- Define and validate business KPIs for demand, driver operations, surge pricing and mobility analytics.
- Prepare Gold metric definitions and ensure that quarantined records are excluded from downstream calculations.
