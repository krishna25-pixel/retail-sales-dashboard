# 📊 Retail Sales Performance Dashboard — Excel & Power BI

An end-to-end retail analytics project that turns raw multi-channel sales data into an interactive
Power BI dashboard and a formula-driven Excel MIS report — built for business-review-style
decision-making across **Product Category**, **Geography**, and **Sales Channel**.

---

## 🎯 Objective

- Design a star-schema data model from raw retail transactions.
- Build a fully interactive Power BI dashboard with drill-downs across category, region, and channel.
- Build an Excel MIS summary report with live KPI cards, RAG-status flags, and a gap/opportunity matrix.
- Translate the numbers into structured, insight-led recommendations for leadership review.

---

## 🗂️ Data Model (Star Schema)

| Table | Grain / Description |
|---|---|
| `Fact_Sales` | 18,000 transaction rows — 2 years (Jan 2023 – Dec 2024) |
| `Dim_Product` | 160 SKUs across 6 categories / 30 sub-categories |
| `Dim_Geography` | 30 stores across 19 cities, 4 regions |
| `Dim_Channel` | Online / In-Store / Wholesale |
| `Dim_Date` | Full 2-year calendar table (marked as Date Table for time intelligence) |

**Relationships:** all four dimension tables connect to `Fact_Sales` in a single-direction,
many-to-one star schema (see [`docs/data_model.png`](docs/screenshots/data_model.png)).

---

## 🧮 Key DAX Measures

Full list in [`docs/DAX_Measures_Library.md`](docs/DAX_Measures_Library.md). Highlights:

- **Volume & Profitability:** Total Revenue, Gross Margin %, Average Order Value, Discount Rate %
- **Time Intelligence:** YoY / MoM Growth %, MTD / QTD / YTD Revenue
- **Contribution & Ranking:** Category / Region / Channel Contribution %, Store Rank
- **Business Review:** RAG Status (Red/Amber/Green growth flag)

---

## 📈 Power BI Dashboard

| Page | What it shows |
|---|---|
| **Overview** | KPI cards, monthly revenue trend, category split, channel mix |
| **Category Deep-Dive** | Category × Sub-Category breakdown, top/bottom performers, margin view |
| **Geography Deep-Dive** | Region/state ranking, store-level drill-down |
| **Channel & Trends** | Channel mix over time, AOV trend, seasonality |

Screenshots: [`docs/screenshots/`](docs/screenshots/)

---

## 📑 Excel MIS Report

A fully formula-driven (`SUMIFS`/`INDEX-MATCH`, zero hardcoded values) workbook:

- **Executive_Summary** — KPI cards + auto-generated key insights
- **Category_MIS / Geography_MIS / Channel_MIS** — Revenue, Margin %, Contribution %, YoY Growth %, RAG status
- **Category_Region_Matrix** — heat-mapped revenue grid with auto-flagged underperformance gaps

File: [`excel/MIS_Summary_Report.xlsx`](excel/MIS_Summary_Report.xlsx)

---

## 💡 Key Business Insights

*(update this section as the dashboard build progresses)*

- Electronics is the largest category by revenue contribution but growth is moderating.
- Beauty & Personal Care, Grocery, and Sports & Outdoors show negative YoY growth — priority gap for leadership review.
- Online channel share has grown from ~25% to ~40% over two years — a structural shift worth acting on.
- November–December show a strong festive-season spike across all categories.

---

## 🛠️ Tech Stack

- **Excel** — Power Query, Power Pivot, DAX, PivotTables, conditional formatting
- **Power BI Desktop** — data modeling, DAX measures, interactive report pages
- **Python** (data generation only) — pandas, openpyxl

---

## 📁 Repository Structure

```
retail-sales-dashboard/
├── README.md
├── data/                      # Raw star-schema CSVs
├── excel/                     # Excel workbooks (star schema + MIS report)
├── powerbi/                   # .pbix file
└── docs/
    ├── DAX_Measures_Library.md
    └── screenshots/
```

---

## 🚀 How to Explore This Project

1. **Power BI:** Open `powerbi/Retail_Sales_Dashboard.pbix` in Power BI Desktop.
2. **Excel:** Open `excel/MIS_Summary_Report.xlsx` — all values recalculate live from the `Data` sheet.
3. **Raw data:** Available in `data/` if you want to rebuild the model from scratch.

---

## 📌 Status

🚧 Work in progress — building this incrementally, page by page. See commit history for day-by-day progress.

