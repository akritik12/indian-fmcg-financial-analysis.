# Power BI Dashboard Guide

This guide builds a 2-page interactive dashboard from the two CSV files in `data/`.

| File | Rows | Columns |
| --- | ---: | --- |
| `fmcg_ratios_long.csv` | 505 | Company, Ticker, FiscalYear, YearEnd, Category, Ratio, Unit, Value |
| `fmcg_financials_long.csv` | 750 | Company, Ticker, FiscalYear, YearEnd, Statement, LineItem, Value_INR_Cr |

Both files are in **long format**: one row per company, year, and metric. Power BI slicers and visuals work best with this layout.

---

## 1. Load the data

1. **Home → Get Data → Text/CSV**, then load `fmcg_ratios_long.csv`.
2. Repeat for `fmcg_financials_long.csv`.
3. In **Power Query** (Transform Data), check the column types:
   * `YearEnd` → Date
   * `Value` and `Value_INR_Cr` → Decimal Number
   * All other columns → Text
4. Click **Close & Apply**.

## 2. Create the model

Create a small **Company** table so one slicer filters both fact tables. Go to **Modeling → New Table**:

```DAX
Company = DISTINCT ( fmcg_ratios_long[Company] )
```

Create a **Year** table the same way:

```DAX
Year = DISTINCT ( fmcg_financials_long[FiscalYear] )
```

In **Model view**, drag relationships (one-to-many, single direction):
* `Company[Company]` → `fmcg_ratios_long[Company]` and `fmcg_financials_long[Company]`
* `Year[FiscalYear]` → `fmcg_ratios_long[FiscalYear]` and `fmcg_financials_long[FiscalYear]`

Always use the columns from `Company` and `Year` in slicers and axes.

## 3. DAX measures

Create each one with **Home → New Measure**.

```DAX
-- Generic ratio value (use with a Ratio slicer or filter)
Ratio Value = AVERAGE ( fmcg_ratios_long[Value] )

-- Specific ratios
Operating Margin = CALCULATE ( [Ratio Value], fmcg_ratios_long[Ratio] = "Operating Margin" )
Net Profit Margin = CALCULATE ( [Ratio Value], fmcg_ratios_long[Ratio] = "Net Profit Margin" )
ROE = CALCULATE ( [Ratio Value], fmcg_ratios_long[Ratio] = "ROE" )
ROCE = CALCULATE ( [Ratio Value], fmcg_ratios_long[Ratio] = "ROCE" )
Asset Turnover = CALCULATE ( [Ratio Value], fmcg_ratios_long[Ratio] = "Asset Turnover" )
Cash Conversion Cycle = CALCULATE ( [Ratio Value], fmcg_ratios_long[Ratio] = "Cash Conversion Cycle" )
Debt to Equity = CALCULATE ( [Ratio Value], fmcg_ratios_long[Ratio] = "Debt to Equity" )
FCF Margin = CALCULATE ( [Ratio Value], fmcg_ratios_long[Ratio] = "FCF Margin" )

-- Financial line items (₹ crore)
Sales = CALCULATE ( SUM ( fmcg_financials_long[Value_INR_Cr] ), fmcg_financials_long[LineItem] = "Sales" )
Net Profit = CALCULATE ( SUM ( fmcg_financials_long[Value_INR_Cr] ), fmcg_financials_long[LineItem] = "Net Profit" )
Free Cash Flow = CALCULATE ( SUM ( fmcg_financials_long[Value_INR_Cr] ), fmcg_financials_long[LineItem] = "Free Cash Flow" )

-- Revenue CAGR FY21 → FY26
Revenue CAGR =
VAR StartSales = CALCULATE ( [Sales], REMOVEFILTERS ( 'Year' ), 'Year'[FiscalYear] = "FY21" )
VAR EndSales   = CALCULATE ( [Sales], REMOVEFILTERS ( 'Year' ), 'Year'[FiscalYear] = "FY26" )
RETURN DIVIDE ( EndSales, StartSales ) ^ ( 1 / 5 ) - 1
```

**Formatting:** select each measure and set the format in **Measure tools**. Use **Percentage** (1 decimal) for margins, ROE, ROCE, FCF Margin, and Revenue CAGR. Use **Decimal** (2 places) for Asset Turnover and Debt to Equity. Use **Whole number** for Cash Conversion Cycle and the ₹ crore measures.

## 4. Page 1: Peer Overview

| Position | Visual | Fields |
| --- | --- | --- |
| Top | **Slicer** (tile style) | `Company[Company]` |
| Top right | **Slicer** (dropdown) | `Year[FiscalYear]`, default FY26 |
| Row 1 | 4 × **Card** | Sales, Net Profit, ROE, Operating Margin |
| Row 2 left | **Clustered bar chart** | Y: `Company`, X: `ROE` |
| Row 2 right | **Clustered bar chart** | Y: `Company`, X: `Revenue CAGR` |
| Row 3 | **Matrix** | Rows: `Company`; Values: Operating Margin, ROE, ROCE, Asset Turnover, Debt to Equity, Cash Conversion Cycle, FCF Margin |

For the matrix, add **conditional formatting → Background color** on ROE and ROCE (green = high), and on Cash Conversion Cycle (green = low).

## 5. Page 2: Trends & DuPont

| Position | Visual | Fields |
| --- | --- | --- |
| Top | **Slicer** | `Company[Company]` |
| Row 1 left | **Line chart** | X: `Year[FiscalYear]`, Y: Operating Margin, Legend: `Company` |
| Row 1 right | **Line chart** | X: `Year[FiscalYear]`, Y: ROE, Legend: `Company` |
| Row 2 left | **Clustered column chart** | X: `Year[FiscalYear]`, Y: Cash Conversion Cycle, Legend: `Company` |
| Row 2 right | **Table** (DuPont) | Filter `Category` to Profitability and DuPont; show `Company`, `FiscalYear`, `Ratio`, `Value` |

**Tip:** set the Year slicer on Page 2 to exclude FY21, because growth and return ratios start in FY22.

## 6. Finishing touches

* Add a text box to each page with the title and "Source: Screener.in, consolidated, ₹ crore".
* **View → Themes**: pick one theme and keep the same colour for each company on every page.
* Add a text box with one key insight per page (see the README findings).
* Save as `dashboard/fmcg_dashboard.pbix`, then export screenshots to `outputs/` for the README.
