# Business Insights Dashboard for Security Incidents

This repository contains my Power BI project for analyzing and visualizing **security incident data**. I developed this dashboard as part of an academic internship/assignment to practice data modeling, DAX, and professional reporting. The work in this repo is authored and implemented by **Cyxtion**.

---

## Project Motivation

I chose security incidents because real-world defenders need quick, actionable views to prioritize response and reduce risk. Building this dashboard helped me connect raw incident logs with measurable outcomes (like MTTR), visualize relationships between affected systems, and create a concise package stakeholders can use to improve detection and remediation.

---

## Key Features

* **Data Cleaning & Modeling**: Multiple source tables imported, cleaned, and combined into a multi-table model with meaningful relationships and date tables for time-intelligence.
* **Interactive Visuals**: Time series for trends, stacked/segmented bars for severity, chord chart for incident-to-asset relationships, geo map for location-based insights, and KPI cards for summary metrics.
* **DAX Measures**: Custom measures such as MTTR (Mean Time to Resolve), Total Incidents, Open vs Resolved counts, Resolution Rate, and rolling aggregations (7/30/90-day windows).
* **Professional Theme & Layout**: A consistent theme, font sizing, grid layout, and navigation (tabs/bookmarks) for stakeholder-ready presentation.
* **Supplementary Deliverables**: Short written report (summary, methods, key findings) and a slide deck to present the main takeaways.

## Setup & How to Use

1. Clone the repo.
2. Open `pbix/Security_Incidents_Dashboard.pbix` in Power BI Desktop (recommended recent version).
3. If you want to re-point to local CSVs, use `Transform data` -> `Data source settings` and update the file paths to the files in `/data/`.
4. Review the Data Model to confirm relationships are intact (Date table should be marked as a Date table for time-intelligence).
5. Use the main report navigation to explore trends, drill-through details, and the chord chart relationships.

---

## Important DAX Measures (examples)

* **MTTR (Mean Time to Resolve)**

```
MTTR =
VAR ResolvedTable = FILTER(Incidents, Incidents[Status] = "Resolved")
RETURN
DIVIDE(
    SUMX(ResolvedTable, DATEDIFF(Incidents[OpenedAt], Incidents[ResolvedAt], MINUTE)),
    COUNTROWS(ResolvedTable)
)
```

* **Total Incidents**: `Total Incidents = COUNTROWS(Incidents)`
* **Resolution Rate**: `Resolution Rate = DIVIDE([Resolved Incidents], [Total Incidents])`
* **Rolling 30-day Incidents**: uses `DATESINPERIOD` over the Date table for period aggregation.

Refer to `DAX.md` for full definitions and explanation of each measure.

---

## Data Cleaning Notes

* Parsed timestamps to UTC and normalized to a single timezone for comparisons.
* Standardized severity labels (Low / Medium / High / Critical).
* Imputed or flagged missing `ResolvedAt` timestamps — unresolved incidents are excluded from MTTR calculations and included in open-incident KPIs.
* Created lookup tables for `Asset`, `Region`, and `IncidentCategory` to reduce cardinality and improve performance.

---

## Dashboard Pages (Overview)

1. **Executive Summary**: Top KPIs and trend sparkline
2. **Trends & Time Analysis**: Monthly/weekly incident volume and moving averages
3. **Severity & Impact**: Distribution by severity and affected asset groups
4. **Response Performance**: MTTR, median/percentile response times, SLA breach counts
5. **Relationships**: Chord/arc diagram linking incident origin to affected systems
6. **Geo Insights**: Map and region-level drill-downs
7. **Data & Model**: Hidden page showing model diagram and source metadata

---

## Insights & Findings (example)

* Peak incident volumes occur during specific business windows; shifting analyst rosters could help reduce MTTR during those hours.
* A small subset of asset types account for a majority of high-severity incidents — prioritizing patching and monitoring here yields quick wins.
* Repeat incidents tied to single change requests indicate a process gap in change validation.

(Use the `short_report.md` for polished, cited findings to include in submissions.)

---

## How This Demonstrates Ownership

This project, from data cleaning to final visual design, was implemented and authored by **Cyxtion**. The `pbix` file, DAX measures, and supporting docs were created and adjusted locally to reflect the dataset and assignment requirements. If you need any specific commit messages or additional author metadata for submission, add them to the repo or tell me what format you want for contributor proof.
