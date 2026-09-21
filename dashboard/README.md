# Power BI Dashboard Folder

Save the final Power BI file here.

Expected file:

```text
dashboard/powerbi_dashboard.pbix
```

Rules:

- Power BI must connect to Gold outputs only.
- Do not connect dashboard visuals directly to raw source files.
- Save dashboard screenshots in `screenshots/`.
- Explain dashboard insights in `docs/dashboard_insights.md`.

## Power BI File-Size Rule

Preferred submission is the PBIX file plus screenshots.

If the `.pbix` file becomes too large to manage cleanly in GitHub, keep the final screenshots and dashboard insight notes in this repo, and add a short note here explaining where the PBIX is stored for mentor review.

Do not keep uploading multiple heavy PBIX versions into GitHub.

## PBI-01 Measure Reconciliation

PBI-01 was reconciled against the governed Gold table
`workspace.default.agg_trip_operations_daily`.

### Validation Period

- Start date: 2026-01-01
- End date: 2026-01-14
- Gold source: `agg_trip_operations_daily`

### Reconciliation Results

| Measure | Gold Validation | Power BI | Status |
|---|---:|---:|---|
| Total Trip Requests | 37,892 | 37.892K | PASS |
| Completion Rate | 64.12% | 64.12% | PASS |
| Cancellation Rate | 23.82% | 24% | PASS |

### Validation

The following Gold calculations were used:

- Total Trip Requests = `SUM(trip_requests)`
- Completion Rate = `SUM(completed_trips) / SUM(trip_requests)`
- Cancellation Rate = `SUM(cancelled_trips) / SUM(trip_requests)`

The Power BI values reconcile with the same filtered Gold slice for
01-Jan-2026 to 14-Jan-2026.

Cancellation Rate is displayed as 24% in Power BI because the Gold
value of 23.82% is rounded for display.

### Evidence

Databricks Gold validation confirmed:

- Total Trip Requests = 37,892
- Completed Trips = 24,297
- Completion Rate = 64.1217%
- Cancelled Trips = 9,027
- Cancellation Rate = 23.8230%

Therefore, the selected PBI-01 measures successfully reconcile to
the governed Gold data for the selected date range.
