# 🏠 Real Estate Analysis — Estate View 360 Dashboard

> A end-to-end data analytics project built with Python, Microsoft Excel, and Power BI — analyzing 500 real estate transactions across major Nigerian cities.

---

## 📌 Project Overview

This project simulates a real-world real estate analytics workflow for a Nigerian property firm. Starting from a raw flat file, the data was restructured into a **star schema**, loaded into **Power BI**, and transformed into an interactive dashboard called **Estate View 360**.

The project covers the full data analyst pipeline:
- Data generation and cleaning
- Star schema design (data modelling)
- DAX measures and KPI calculations
- Dashboard design and visualization
- Interactive HTML dashboard (web-based)

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| Python (openpyxl, xlsxwriter) | Data generation & star schema build |
| Microsoft Excel | Star schema output (fact + dimension tables) |
| Microsoft Power BI | Data model, DAX measures, interactive dashboard |
| HTML + Chart.js | Standalone interactive web dashboard |
| GitHub | Version control & portfolio hosting |

---

## 📂 Repository Contents

| File | Description |
|------|-------------|
| `RealEstate_Dataset.csv` | Raw flat file — 500 rows, 26 columns |
| `RealEstate_StarSchema.xlsx` | Star schema — 5 structured tables |
| `RealEstate_Dashboard.html` | Interactive web dashboard (open in any browser) |

---

## 📊 Dataset Description

The flat file contains **500 real estate transactions** across 7 Nigerian cities with 26 columns covering every aspect of a property deal.

**Cities covered:** Lagos · Abuja · Port Harcourt · Ibadan · Kano · Enugu · Benin City

**Key columns:**

| Column | Description |
|--------|-------------|
| TransactionID | Unique transaction identifier |
| PropertyType | Apartment, Duplex, Bungalow, Terrace House, Semi-Detached, Detached House, Penthouse |
| City / Area | Location of the property |
| Bedrooms / Bathrooms | Property size details |
| SizeSqm | Property size in square metres |
| TransactionType | Sale or Rent |
| Price | Transaction value in Nigerian Naira (₦) |
| ListingDate / ClosingDate | Deal timeline |
| DaysOnMarket | Number of days between listing and closing |
| Status | Completed, Pending, or Cancelled |
| AgentID / AgentName | Assigned agent (8 agents in total) |
| ClientName / ClientAge / ClientGender | Client demographic info |
| ClientIncomeLevel / ClientOccupation | Client financial profile |
| Amenities | Property features (pool, gym, parking, etc.) |
| Year / Month / Quarter | Time intelligence columns |

---

## 🗂️ Star Schema Design

The flat file was normalised into a **star schema** with 1 fact table and 4 dimension tables.

```
                    DimProperty
                        │
DimAgent ──── FactTransaction ──── DimClient
                        │
                    DimLocation
```

### Tables

**FactTransaction** *(500 rows)*
> The central table. Holds all measurable data and foreign keys linking to each dimension.

| Column | Type | Description |
|--------|------|-------------|
| TransactionID | PK | Unique identifier |
| PropertyID | FK | Links to DimProperty |
| ClientID | FK | Links to DimClient |
| AgentID | FK | Links to DimAgent |
| LocationID | FK | Links to DimLocation |
| TransactionType | Text | Sale or Rent |
| Price | Number | Transaction value (₦) |
| ListingDate | Date | Date property was listed |
| ClosingDate | Date | Date deal was closed |
| DaysOnMarket | Number | Days from listing to close |
| Status | Text | Completed / Pending / Cancelled |
| Year | Number | Transaction year |
| Month | Text | Transaction month |
| Quarter | Text | Q1 – Q4 |

---

**DimProperty** *(252 rows)*
> Unique properties with physical attributes.

Columns: PropertyID (PK), PropertyType, Bedrooms, Bathrooms, SizeSqm, Amenities

---

**DimClient** *(283 rows)*
> Unique clients with demographic information.

Columns: ClientID (PK), ClientName, ClientAge, ClientGender, ClientMaritalStatus, ClientIncomeLevel, ClientOccupation

---

**DimAgent** *(8 rows)*
> All 8 agents operating in the firm.

Columns: AgentID (PK), AgentName

| AgentID | Agent Name |
|---------|-----------|
| AGT001 | Chidi Okafor |
| AGT002 | Aisha Bello |
| AGT003 | Seun Fashola |
| AGT004 | Fatima Aliyu |
| AGT005 | Kelechi Obi |
| AGT006 | Tunde Adeyemi |
| AGT007 | Emeka Nwosu |
| AGT008 | Ngozi Eze |

---

**DimLocation** *(20 rows)*
> 20 unique city–area combinations.

Columns: LocationID (PK), City, Area

---

## 📐 KPIs & DAX Measures (Power BI)

The following measures were built in Power BI using DAX:

### Core KPIs

```dax
-- Total Revenue
Total Revenue = SUM(FactTransaction[Price])

-- Total Transactions
Total Transactions = COUNTROWS(FactTransaction)

-- Completion Rate
Completion Rate =
DIVIDE(
    CALCULATE(COUNTROWS(FactTransaction), FactTransaction[Status] = "Completed"),
    COUNTROWS(FactTransaction)
) * 100

-- Average Deal Price
Avg Deal Price = AVERAGE(FactTransaction[Price])

-- Average Days on Market
Avg Days on Market = AVERAGE(FactTransaction[DaysOnMarket])
```

### Sales & Rental KPIs

```dax
-- Total Sales Revenue
Total Sales Revenue =
CALCULATE(SUM(FactTransaction[Price]), FactTransaction[TransactionType] = "Sale")

-- Total Rental Revenue
Total Rental Revenue =
CALCULATE(SUM(FactTransaction[Price]), FactTransaction[TransactionType] = "Rent")

-- Rental Rate %
Rental Rate =
DIVIDE(
    CALCULATE(COUNTROWS(FactTransaction), FactTransaction[TransactionType] = "Rent"),
    COUNTROWS(FactTransaction)
) * 100

-- Commission Earned (5% of completed sales)
Commission Earned =
CALCULATE(SUM(FactTransaction[Price]), FactTransaction[Status] = "Completed") * 0.05
```

### Agent Performance

```dax
-- Revenue per Agent
Revenue per Agent =
DIVIDE([Total Revenue], DISTINCTCOUNT(FactTransaction[AgentID]))
```

### Month Sort (Calculated Column)

```dax
-- MonthNumber (for correct month ordering)
MonthNumber =
VAR m = FactTransaction[Month]
RETURN SWITCH(m,
    "January",1, "February",2, "March",3, "April",4,
    "May",5, "June",6, "July",7, "August",8,
    "September",9, "October",10, "November",11, "December",12
)
```

---

## 📈 Dashboard — Estate View 360

The Power BI dashboard was designed with a consistent **purple/dark theme** and includes:

### Visuals

| Visual | Description |
|--------|-------------|
| KPI Cards | Total Revenue, Transactions, Completion Rate, Avg Price, Avg Days on Market |
| Bar Chart | Revenue by City |
| Donut Chart | Sale vs Rent split |
| Horizontal Bar | Revenue by Agent (sorted descending) |
| Line Chart | Transactions by Month (sorted Jan–Dec) |
| Bar Chart | Distribution by Property Type |
| Donut Chart | Deal Status breakdown |
| Slicer | Filter by Property Type |

### Interactive Web Dashboard

A standalone `RealEstate_Dashboard.html` file is included. Open it in any browser — no installation needed.

Features:
- 4 live filters: City, Year, Transaction Type, Property Type
- All 6 charts update instantly on filter change
- Works completely offline

---

## 🚀 How to Use

### Option 1 — Power BI
1. Download `RealEstate_StarSchema.xlsx`
2. Open Power BI Desktop → **Get Data → Excel**
3. Load all 5 sheets (select all tables)
4. In the Model view, create relationships:
   - `FactTransaction[PropertyID]` → `DimProperty[PropertyID]`
   - `FactTransaction[ClientID]` → `DimClient[ClientID]`
   - `FactTransaction[AgentID]` → `DimAgent[AgentID]`
   - `FactTransaction[LocationID]` → `DimLocation[LocationID]`
5. Create DAX measures listed above and build your visuals

### Option 2 — Web Dashboard
1. Download `RealEstate_Dashboard.html`
2. Double-click to open in any browser (Chrome, Edge, Firefox)
3. Use the filters at the top to explore the data

### Option 3 — Raw Data
1. Download `RealEstate_Dataset.csv`
2. Open in Excel, Power BI, or any analytics tool
3. All 500 rows ready for analysis

---

## 💡 Key Insights

- **Lagos and Abuja** account for over 60% of total transaction revenue
- **Rental transactions** outnumber sales (~60% rent vs ~40% sale)
- The average property deal takes **~92 days** from listing to close
- **Completion rate** sits at approximately **71%** of all transactions
- **Detached Houses and Penthouses** generate the highest average deal values
- Agent **Chidi Okafor** and **Aisha Bello** consistently lead in revenue generation

---

## 👤 Author

**Abdulrazaq Shola**
Data Analyst | Power BI | Python | SQL

📧 abdulrazaqodeyemi56@gmail.com
🔗 [Portfolio](https://github.com/Abdulrazaq282/Abdulrazaq-s-Portfollio)

---

*Built as part of a data analytics portfolio project — demonstrating end-to-end skills from raw data to interactive dashboard.*
