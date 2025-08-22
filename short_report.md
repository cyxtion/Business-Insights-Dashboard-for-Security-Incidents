# Short Report — Business Insights Dashboard for Security Incidents

## Dataset Source & Description
Synthetic dataset generated for instructional purposes. Includes 5,000+ security incidents linked to assets, users, threat actors, vulnerabilities, and a calendar dimension. Data spans 2023-01-01 to 2025-08-20.

## Business Problem
Leadership needs a unified, interactive view of security incidents to understand trends, prioritize remediation, and reduce mean time to resolve (MTTR). The dashboard answers: which vectors dominate, which units are most impacted, how risk shifts over time, and where MTTR can be improved.

## Cleaning & Integrity
- Removed duplicates.
- Standardized categorical values and data types.
- Imputed missing categorical fields with "Unknown".
- Capped extreme MTTR values at the 99th percentile.
- Enforced referential integrity across foreign keys.

## Feature Engineering & Transformations
- Severity score (1–4), MTTR buckets, and composite Risk Score.
- Extracted date parts and hour for temporal analyses.
- Applied log transform within risk formula to handle skewed alert counts.

## Initial Descriptive Insights
- Total incidents: 5000
- High/Critical share: 24.66%
- Average MTTR: 9.55 hours (median 7.0)
- Top vectors and day/hour heatmap highlight operational hotspots.

## Visuals Included
- Monthly incident trend
- Severity distribution
- Top attack vectors
- Avg MTTR by severity
- Incidents by day of week vs hour (heatmap)

