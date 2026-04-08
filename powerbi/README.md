# Power BI Dashboard

## Overview
Four-page interactive dashboard built on weekly SAP Deepdive data connected via SharePoint.

## Pages
- **Dashboard** — LV Planned, Actuals, Forecast Accuracy, Delay Flag Count, Forecast Shift Days
- **Budget Dashboard** — Total Exposure, Critical Projects Count, exposure by PM
- **Financial KPIs** — IGM%, Cost Overrun%, Billed%, Unbilled Exposure, VIPP Consistency Check, cost structure donut
- **ML Risk Model** — Random Forest risk scores, high-risk WBS table, risk distribution

## Files
- `DAX_Measures.md` — all DAX measures with formulas
- `Power_Query.md` — ETL pipeline connecting SharePoint to Power BI

## Data Refresh
Automated daily refresh at 11:30 PM via Power BI Service.

## Requirements
- Microsoft Power BI Desktop
- Access to Signify EMEA SharePoint (corporate credentials required)
