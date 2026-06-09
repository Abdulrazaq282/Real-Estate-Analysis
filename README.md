# Real Estate Analysis — Estate View 360 Dashboard

## Overview
Power BI dashboard analyzing **500 real estate transactions** across major Nigerian cities.

## Tools Used
- Python (data generation & star schema design)
- Microsoft Power BI (dashboard & DAX measures)
- Excel (star schema output)

## Dataset
- 500 transactions across Lagos, Abuja, Port Harcourt, Ibadan, Kano, Enugu, Benin City
- 26 columns covering property, client, agent, and transaction details

## Star Schema
| Table | Rows | Description |
|-------|------|-------------|
| FactTransaction | 500 | Core transaction data with FK links |
| DimProperty | 252 | Property type, size, amenities |
| DimClient | 283 | Client demographics |
| DimAgent | 8 | Agent details |
| DimLocation | 20 | City and area |

## KPIs
- Total Revenue
- Total Transactions
- Completion Rate
- Average Property Price
- Rental vs Sale Split
- Average Days on Market
- Commission Earned
- Revenue by Agent

## Dashboard Preview
Estate View 360 Analysis Dashboard — built with consistent purple theme, card KPIs, bar charts, donut chart, line chart, and property type slicer.
