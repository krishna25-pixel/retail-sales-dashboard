# DAX Measures Library — Retail Sales Performance Dashboard

Paste these into **Power Pivot** (Excel: Power Pivot tab → Measures → New Measure) or
**Power BI** (Model view → New Measure), after the model relationships below are in place.

## 0. Model Relationships (set these first)
| From | To | Cardinality |
|---|---|---|
| Fact_Sales[ProductID] | Dim_Product[ProductID] | Many-to-One |
| Fact_Sales[StoreID] | Dim_Geography[StoreID] | Many-to-One |
| Fact_Sales[ChannelID] | Dim_Channel[ChannelID] | Many-to-One |
| Fact_Sales[Date] | Dim_Date[Date] | Many-to-One |

Mark **Dim_Date** as a Date Table: Table tools → Mark as Date Table → column = `Date`.

---

## 1. Core Volume Measures
```DAX
Total Revenue = SUM(Fact_Sales[NetRevenue])

Total Gross Revenue = SUM(Fact_Sales[GrossRevenue])

Total Units Sold = SUM(Fact_Sales[UnitsSold])

Total Cost = SUM(Fact_Sales[TotalCost])

Total Discount = SUM(Fact_Sales[DiscountAmount])

Order Count = DISTINCTCOUNT(Fact_Sales[OrderID])
```

## 2. Profitability & Efficiency
```DAX
Gross Profit = [Total Revenue] - [Total Cost]

Gross Margin % = DIVIDE([Gross Profit], [Total Revenue])

Average Order Value (AOV) = DIVIDE([Total Revenue], [Order Count])

Discount Rate % = DIVIDE([Total Discount], [Total Gross Revenue])

Revenue per Unit = DIVIDE([Total Revenue], [Total Units Sold])
```

## 3. Time Intelligence (requires Dim_Date marked as Date Table)
```DAX
Revenue LY =
CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Dim_Date[Date]))

YoY Growth % =
DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY])

Revenue MTD =
TOTALMTD([Total Revenue], Dim_Date[Date])

Revenue QTD =
TOTALQTD([Total Revenue], Dim_Date[Date])

Revenue YTD =
TOTALYTD([Total Revenue], Dim_Date[Date])

Revenue Prior Month =
CALCULATE([Total Revenue], DATEADD(Dim_Date[Date], -1, MONTH))

MoM Growth % =
DIVIDE([Total Revenue] - [Revenue Prior Month], [Revenue Prior Month])

Rolling 3-Month Revenue =
CALCULATE(
    [Total Revenue],
    DATESINPERIOD(Dim_Date[Date], LASTDATE(Dim_Date[Date]), -3, MONTH)
)
```

## 4. Contribution & Ranking (Category / Region / Channel)
```DAX
Category Contribution % =
DIVIDE([Total Revenue], CALCULATE([Total Revenue], ALL(Dim_Product[Category])))

Region Contribution % =
DIVIDE([Total Revenue], CALCULATE([Total Revenue], ALL(Dim_Geography[Region])))

Channel Contribution % =
DIVIDE([Total Revenue], CALCULATE([Total Revenue], ALL(Dim_Channel[ChannelName])))

Category Rank =
RANKX(ALL(Dim_Product[Category]), [Total Revenue], , DESC)

Store Rank =
RANKX(ALL(Dim_Geography[StoreID]), [Total Revenue], , DESC)

Top N Flag (Store) =
IF([Store Rank] <= 5, "Top 5", "Other")
```

## 5. Gap / Target Analysis
> Assumes a manual `Target Revenue` input, e.g. via a disconnected "Target Growth %" parameter table,
> or a static target per Category/Region added as a small assumptions table.
```DAX
Target Revenue = [Revenue LY] * 1.15   -- example: 15% growth target vs last year

Revenue vs Target = [Total Revenue] - [Target Revenue]

Revenue vs Target % = DIVIDE([Revenue vs Target], [Target Revenue])

RAG Status =
VAR GapPct = [Revenue vs Target %]
RETURN
    SWITCH(
        TRUE(),
        GapPct >= 0, "Green",
        GapPct >= -0.10, "Amber",
        "Red"
    )
```

## 6. Channel Mix
```DAX
Online Revenue = CALCULATE([Total Revenue], Dim_Channel[ChannelName] = "Online")

InStore Revenue = CALCULATE([Total Revenue], Dim_Channel[ChannelName] = "In-Store")

Wholesale Revenue = CALCULATE([Total Revenue], Dim_Channel[ChannelName] = "Wholesale")

Online Mix % = DIVIDE([Online Revenue], [Total Revenue])
```

## 7. KPI Card Formatting Tips
- Format all `% ` measures as Percentage, 1 decimal.
- Format currency measures with thousands separator, no decimals for cards (`#,##0`), 2 decimals in detail tables.
- Add conditional formatting (icons ▲▼) to YoY Growth % and MoM Growth % in Power BI table/matrix visuals via
  Format pane → Conditional formatting → Icons.

---

### How to use this file
1. Open Power Pivot (Excel) or Model view (Power BI).
2. Build the relationships in section 0.
3. Create each measure exactly as named above — later measures (Section 3+) reference earlier ones, so build top to bottom.
4. Test each measure against a PivotTable/visual with Category on rows before moving to the next section.
