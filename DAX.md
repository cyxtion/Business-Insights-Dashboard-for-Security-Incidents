# DAX (Calculated Columns & Measures)

## Date Table (if not using `date_dim.csv`)
```DAX
Date = CALENDARAUTO()

Year = YEAR('Date'[Date])
Month = FORMAT('Date'[Date], "MMM")
Quarter = "Q" & FORMAT('Date'[Date], "Q")
Is Weekend = IF(WEEKDAY('Date'[Date], 2) > 5, 1, 0)
```

## Calculated Columns (Incidents)
```DAX
Severity Score = SWITCH(Incidents[severity],
    "Low", 1, "Medium", 2, "High", 3, "Critical", 4, BLANK()
)

MTTR Bucket = 
VAR h = Incidents[mttr_hours]
RETURN
SWITCH(TRUE(),
    h < 4, "<4h",
    h < 24, "4-24h",
    h < 72, "1-3d",
    h < 168, "3-7d",
    ">7d"
)
```

## Measures
```DAX
Total Incidents = DISTINCTCOUNT(Incidents[incident_id])

Avg MTTR (Hours) = AVERAGE(Incidents[mttr_hours])

High+Critical % = 
VAR hc = CALCULATE([Total Incidents], Incidents[severity] IN {"High","Critical"})
RETURN DIVIDE(hc, [Total Incidents])

Incidents (Last 28 Days) = 
CALCULATE([Total Incidents],
    DATESINPERIOD('date_dim'[date], MAX('date_dim'[date]), -28, DAY)
)

Incident Rate per 100 Assets = 
VAR inc = [Total Incidents]
VAR assets = DISTINCTCOUNT(Assets[asset_id])
RETURN DIVIDE(inc * 100, assets)

Avg Risk Score = AVERAGE(Incidents[risk_score])

Cost (Total) = SUM(Incidents[cost_usd])
```

## Visuals Mapping
- Cards: `Total Incidents`, `Avg MTTR (Hours)`, `High+Critical %`, `Cost (Total)`
- Line: `Total Incidents` by `date_dim[date]`
- Bar: `Avg MTTR (Hours)` by `Incidents[severity]`
- Bar: `Total Incidents` by `Incidents[attack_vector]`
- Matrix: `Total Incidents` by `Assets[business_unit]` and `Users[region]`
- Donut: `Total Incidents` by `Incidents[severity]`
- Heatmap: Build with Matrix using `date_dim[day_name]` vs `Incidents[hour]` and color by `[Total Incidents]`
