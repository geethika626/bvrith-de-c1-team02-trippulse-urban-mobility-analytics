# Problem Charter

**Week:** 1  
**Owner(s):** Ms.N.Geethika, Ms.Badugu Sameeksha, Ms.Shaik Sameera   
**Project:** TripPulse: Urban Mobility Analytics

---

## 1. Problem Context

TripPulse is an Urban Mobility Analytics project modeled on ride-hailing platforms such as Ola, Uber, and Rapido. It uses synthetic trip, driver, zone, and payment data to analyze ride demand, fares, cancellations, surge pricing, and driver reliability. Raw data may contain inconsistencies such as invalid fares, missing driver IDs, incorrect timestamps, and duplicate records. The final dashboard and metrics will help stakeholders make informed operational and business decisions.

- Real-world operation represented:
    - Ride-hailing and urban mobility operations.

- Types of data generated:
    - Trip data
    - Driver profiles
    - Zone information
    - Payment records
    - Ride request streaming events

- Why raw data is not enough:
    - Raw data may contain errors, missing values, duplicate events, and invalid records that can lead to inaccurate insights.

- Who will use the final dashboard?
    - City Operations Lead
    - Driver Operations Manager
    - Pricing/Surge Analyst
    - Customer Experience Lead

---

## 2. Engineering Problem

The project must transform multiple raw source files (trips.parquet, drivers.json, zones.csv, and payments.csv) into trusted Bronze, Silver, Data Quality, and Gold outputs using Databricks and Spark SQL. The pipeline will clean, validate, and standardize data before generating business metrics and dashboard-ready outputs in Power BI. A streaming simulation will process ride request events to generate live ride metrics.

---

## 3. Users / Stakeholders

| User / Stakeholder | What they need from the data |
|---|---|
| City Operations Lead | Analyze ride demand across city zones and peak hours |
| Driver Operations Manager | Monitor driver acceptance, cancellations, and completion rates |
| Pricing / Surge Analyst | Analyze surge pricing and ride acceptance trends |
| Customer Experience Lead | Identify ride failures, payment issues, and pickup delays |

---

## 4. Scope Inclusions

- Synthetic dataset generation
- Bronze layer ingestion
- Silver data standardization and joins
- Gold metric generation
- Power BI dashboard creation
- Streaming simulation using JSON events
- GitHub documentation and weekly evidence
---

## 5. Scope Exclusions

- No production ride-booking application
- No navigation or ride dispatch system
- No real customer or driver data
- No real Ola/Uber/Rapido datasets
- No payment gateway integration
- No copied internet project submissions
- No fake screenshots or unexplained AI-generated work

---

## 6. Success Criteria

By the end of 12 weeks, the project is successful if:

- The complete data pipeline can be explained end-to-end.
- Bronze, Silver, Data Quality, and Gold layers are successfully implemented.
- Gold tables generate trusted business metrics.
- Power BI dashboards visualize Gold outputs only.
- The streaming simulation successfully produces live ride metrics.
- All team members can explain the project architecture and workflow.
- GitHub contains weekly logs, screenshots, documentation, and submission files.
