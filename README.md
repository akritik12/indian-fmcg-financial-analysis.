# Indian FMCG Financial Statement & Ratio Analysis

![Excel](https://img.shields.io/badge/Excel-Financial%20Model-217346)
![Dashboard](https://img.shields.io/badge/Excel-Interactive%20Dashboard-217346)
![Finance](https://img.shields.io/badge/Domain-Financial%20Analysis-blue)
![Period](https://img.shields.io/badge/Period-FY21--FY26-lightgrey)

[![Excel Model](https://img.shields.io/badge/Open-Excel%20Model-217346?style=for-the-badge)](excel/FMCG_Financial_Analysis.xlsx)
[![Data](https://img.shields.io/badge/View-Data-blue?style=for-the-badge)](data)
[![Dashboard](https://img.shields.io/badge/View-Dashboard-F2C811?style=for-the-badge)](outputs/dashboard.png)

A 5-year financial comparison of five leading Indian FMCG companies (**HUL, ITC, Dabur, Britannia, and Marico**) using ratio analysis, DuPont analysis, and cash flow analysis. It is built entirely in **Excel** as a formula-driven model with an interactive dashboard.

---

## TL;DR

* **Britannia is the most efficient:** it has the highest ROE (54% in FY26) and ROCE (56%). This comes from asset turnover of 2.1x, more than double HUL's or ITC's.
* **ITC is the most profitable:** its operating margin is about 35%, but its cash conversion cycle is 164 days because it holds large leaf-tobacco inventories.
* **HUL runs on its suppliers' money:** it has a cash conversion cycle of −89 days, the best of the group. However, goodwill from the GSK merger keeps its ROE (about 20% in normal years) well below peers.
* **Marico is growing fastest:** revenue grew 11.1% a year, and 25.7% in FY26 alone. Its operating margin, however, fell to 17.1%, the lowest in the group.
* **Dabur is the weakest:** net profit grew just 2% a year, and ROE fell every year from 21.7% to 16.8%.
* **Watch the one-offs:** ITC's FY25 profit and HUL's FY26 profit both include large one-time gains. Raw ROE and profit growth for those years overstate the underlying performance.

---

## Business Question

> *Which Indian FMCG company has delivered the best combination of growth, profitability, efficiency, and financial strength over the last five years, and what drives the differences?*

---

## Data

| Detail | Value |
| --- | --- |
| Source | [Screener.in](https://www.screener.in) consolidated annual financials, compiled from company filings |
| Retrieved | 26 September 2026 |
| Period | FY21–FY26 (years ending 31 March) |
| Units | ₹ crore |
| Statements | Profit & Loss, Balance Sheet, Cash Flow, and working capital days |

| Company | Ticker | Screener Link |
| --- | --- | --- |
| Hindustan Unilever | HINDUNILVR | [link](https://www.screener.in/company/HINDUNILVR/consolidated/) |
| ITC | ITC | [link](https://www.screener.in/company/ITC/consolidated/) |
| Dabur India | DABUR | [link](https://www.screener.in/company/DABUR/consolidated/) |
| Britannia Industries | BRITANNIA | [link](https://www.screener.in/company/BRITANNIA/consolidated/) |
| Marico | MARICO | [link](https://www.screener.in/company/MARICO/consolidated/) |

### Data validation

* For all 30 company-years, **Sales − Expenses = Operating Profit**, **PBT = Operating Profit + Other Income − Interest − Depreciation**, and **Total Assets = Total Liabilities** (within ±₹2 cr of source rounding).
* Marico's FY26 revenue (₹13,611 cr) and net profit (₹1,813 cr) were confirmed against the company's [published results](https://www.investywise.com/marico-limited-q4-fy26-financial-results-and-dividend-announcement/).
* Every Excel ratio was checked against an independent Python calculation.

> **Why not Nestlé India?** Nestlé moved from a December to a March year-end, with a 15-month transition period, so its years don't line up with the other companies. Marico was used instead.

---

## Method

### Ratios calculated

| Category | Ratio | Formula |
| --- | --- | --- |
| Growth | Revenue / Net Profit Growth | This year ÷ last year − 1 |
| Growth | Revenue / Net Profit CAGR | (FY26 ÷ FY21)^(1/5) − 1 |
| Profitability | Operating Margin | Operating Profit ÷ Sales |
| Profitability | Net Profit Margin | Net Profit ÷ Sales |
| Profitability | ROE | Net Profit ÷ Average Shareholders' Equity |
| Profitability | ROCE | EBIT ÷ Average (Equity + Borrowings) |
| Profitability | ROA | Net Profit ÷ Average Total Assets |
| Efficiency | Asset Turnover | Sales ÷ Average Total Assets |
| Efficiency | Cash Conversion Cycle | Debtor Days + Inventory Days − Payable Days |
| Leverage | Debt / Equity | Borrowings ÷ Shareholders' Equity |
| Leverage | Interest Coverage | EBIT ÷ Interest |
| Cash Flow | CFO / Net Profit | Cash from Operations ÷ Net Profit |
| Cash Flow | FCF Margin | Free Cash Flow ÷ Sales |

**DuPont analysis** breaks ROE into three drivers:

```
ROE = Net Profit Margin × Asset Turnover × Equity Multiplier
      (profitability)     (efficiency)      (leverage)
```

Return ratios use **average** opening and closing balances, so they start in FY22.

### Excel model structure

| Sheet | Contents |
| --- | --- |
| **Notes** | Sources, how to use, colour legend, method notes, one-off items |
| **Dashboard** | Interactive one-page dashboard: pick a company and year from dropdowns to update the KPI cards, trend charts, DuPont breakdown, and peer table |
| **Financials** | Raw statements for each company (blue = input, black = formula) |
| **Ratios** | 20+ ratios per company per year, all formulas linked to Financials |
| **Comparison** | FY26 snapshot, 5-year averages, and peer rankings |
| **Charts** | Peer trend charts for margins, returns, and working capital |
| **Calc** | Lookup table that feeds the Dashboard (INDEX/MATCH) |

Every number in Ratios, Comparison, and Dashboard is a live formula. Changing an input in Financials updates the whole model.

---

## Results

### FY26 Peer Snapshot

| Metric | HUL | ITC | Dabur | Britannia | Marico |
| --- | ---: | ---: | ---: | ---: | ---: |
| Revenue CAGR (FY21–26) | 6.5% | 9.9% | 6.6% | 7.8% | **11.1%** |
| Net Profit CAGR (FY21–26) | 13.5%* | 9.4% | 2.0% | 6.5% | 8.6% |
| Operating Margin | 23.3% | **34.6%** | 18.6% | 18.3% | 17.1% |
| Net Profit Margin | 23.4%* | **26.6%** | 14.2% | 13.2% | 13.3% |
| ROE | 30.7%* | 29.5% | 16.8% | **53.6%** | 44.3% |
| ROCE | 36.8%* | 38.7% | 21.0% | **56.3%** | 50.1% |
| Asset Turnover | 0.81x | 0.87x | 0.78x | **2.06x** | 1.49x |
| Cash Conversion Cycle | **−89 days** | 164 days | −26 days | −9 days | 12 days |
| Debt / Equity | **0.03x** | 0.03x | 0.11x | 0.27x | 0.13x |
| FCF Margin | 15.0% | **20.7%** | 16.5% | 12.6% | 13.0% |

\* *HUL's FY26 figures are lifted by one-off other income of ₹4,923 cr. Its 5-year average ROE is 22.2%.*

### 5-Year Average ROE and ROCE (FY22–FY26)

| | HUL | ITC | Dabur | Britannia | Marico |
| --- | ---: | ---: | ---: | ---: | ---: |
| ROE | 22.2% | 32.3% | 18.8% | **57.8%** | 40.2% |
| ROCE | 28.7% | 41.4% | 23.0% | **51.0%** | 46.4% |

### Interactive Excel Dashboard

Choose any company and year from the yellow dropdowns. The KPI cards (with change versus the previous year), 5-year trend charts, DuPont breakdown, and colour-scaled peer table all update automatically, using `INDEX`/`MATCH` lookups, data validation, and conditional formatting.

[![Excel Dashboard](outputs/dashboard.png)](outputs/dashboard.png)

---

## DuPont Analysis: What Drives ROE? (FY26)

| Company | Net Margin | × Asset Turnover | × Equity Multiplier | = ROE |
| --- | ---: | ---: | ---: | ---: |
| HUL | 23.4% | 0.81x | 1.63x | 30.7% |
| ITC | 26.6% | 0.87x | 1.27x | 29.5% |
| Dabur | 14.2% | 0.78x | 1.52x | 16.8% |
| Britannia | 13.2% | **2.06x** | 1.96x | **53.6%** |
| Marico | 13.3% | 1.49x | **2.23x** | 44.3% |

Companies earn high ROE in two different ways:

* **ITC and HUL** earn high margins on a large asset base. ITC holds large investments and inventory, while HUL carries GSK goodwill, so both turn assets over slowly.
* **Britannia and Marico** earn modest margins but use a small, fast-moving asset base, and they return most of their profit as dividends, which keeps equity lean. Britannia's net margin is only half of ITC's, yet its ROE is almost double.

---

## Company Insights

**Britannia: the efficiency leader.** Asset turnover rose from 1.82x to 2.06x, and ROCE climbed from 41.6% (FY22) to 56.3% (FY26). It also cut debt sharply, with debt-to-equity falling from 0.85x in FY23 to 0.27x in FY26.

**ITC: high margins, heavy working capital.** ITC's operating margin (34–37%) is the best in the group, because cigarettes are a high-margin business. Inventory days rose to 209 in FY26, because ITC buys and ages leaf tobacco. Its FY25 net profit of ₹35,052 cr includes a one-time gain from the **ITC Hotels demerger**. Excluding that gain, profit growth was steady.

**HUL: the most stable business.** HUL's operating margin stayed between 23% and 25% every year. It pays suppliers after about 169 days but collects cash from customers in about 19, so suppliers effectively fund its operations. Its FY26 profit rose 41%, but cash from operations covered only 73% of net profit that year, which suggests much of the jump was not cash earnings.

**Marico: growth with margin pressure.** Marico had the fastest revenue growth in the group, including +25.7% in FY26, while ROE rose every year (38% → 44%). Its operating margin, however, fell from 19.7% to 17.1%, which suggests growth came partly from price increases while input costs rose faster.

**Dabur: slowing returns.** ROE fell every year (21.7% → 16.8%) and ROCE fell from 26% to 21%. Investments more than doubled (₹4,160 cr → ₹8,947 cr) without a matching rise in profit, so asset turnover fell from 0.94x to 0.78x.

---

## Repository Structure

```
indian-fmcg-financial-analysis/
├── data/
│   ├── fmcg_financials_long.csv   # 750 rows: all statements in long format
│   └── fmcg_ratios_long.csv       # 505 rows: all ratios in long format
├── excel/
│   └── FMCG_Financial_Analysis.xlsx
├── outputs/
│   └── dashboard.png              # Excel dashboard screenshot
└── README.md
```

---

## Limitations

* **One-off items:** ITC FY25 and HUL FY26 include large exceptional gains. Ratios use reported profit without adjustments.
* **HUL FY25 revenue:** revenue dipped 0.9%. This may partly reflect how the demerged ice cream business is reported, so check the annual reports before comparing.
* **No current ratio:** the source balance sheet does not split current and non-current items.
* **Different business mixes:** ITC earns most of its profit from cigarettes and also runs agri and paper businesses, so it is not a pure FMCG comparison.
* **Aggregated source:** figures come from Screener.in rather than directly from annual reports.

---

## Skills Demonstrated

* Financial statement analysis
* Ratio and DuPont analysis
* Working capital analysis
* Financial modelling in Excel (formula-driven, colour-coded inputs)
* Interactive Excel dashboards (dropdowns, INDEX/MATCH, conditional formatting)
* Data validation and reconciliation
* Business insight writing

---

## Future Improvements

* [ ] Adjust profits for one-off items and compare adjusted ROE
* [ ] Add valuation ratios (P/E, EV/EBITDA) using market prices
* [ ] Extend the analysis to 10 years
* [ ] Add Nestlé India once it has several full March-year-end periods
* [ ] Rebuild the dashboard in Power BI or Tableau using the long-format CSV files in `data/`

---

## Disclaimer

Educational portfolio project. Not investment advice.

---

## Author

**Akriti Kachroo**
Portfolio: [akritik12.github.io](https://akritik12.github.io/)
