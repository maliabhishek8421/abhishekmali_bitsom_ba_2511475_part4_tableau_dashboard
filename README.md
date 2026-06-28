# Part 4: Tableau Executive Dashboard & Data Storytelling

## Business Problem Summary

A retail leadership team needs a single executive dashboard to monitor sales performance, profitability, customer segments, category performance, shipping performance, discount impact, and return patterns — so they can quickly spot business opportunities and risks without digging through raw data.

## Dataset Description

- **File:** `data/dashboard_sales_data.xlsx`
- **Sheets:** `dashboard_sales_data` (the data, 4,200 rows × 20 columns) and `data_dictionary` (field definitions, supplied with the file)
- **Granularity:** One row per order (`order_id` is unique — confirmed no duplicates, so order-level aggregations are straightforward)
- **Date range:** 2024-01-01 to 2025-12-31 (2 full years)
- **Geography:** 19 Indian states across 4 regions (North, South, East, West), 20 cities

### Field Types (as connected in Tableau)

| Field | Tableau Type | Role | Notes |
|---|---|---|---|
| `order_id` | String, Dimension | Identifier | Unique per row; used as COUNTD basis for orders |
| `order_date` | Date, Dimension | Date field | Used for trend views |
| `ship_date` | Date, Dimension | Date field | Used to sanity-check delivery_days |
| `customer_id` | String, Dimension | Identifier | 4,096 unique customers across 4,200 orders |
| `customer_segment` | String, Dimension | Categorical | Consumer / Corporate / Home Office |
| `region` | String, Dimension | Geographic | North / South / East / West |
| `state` | String, Dimension | Geographic | 19 Indian states — set as Geographic Role "State/Province", Country = India, for the map view |
| `city` | String, Dimension | Geographic | 20 cities |
| `category` | String, Dimension | Categorical | Furniture / Office Supplies / Technology |
| `sub_category` | String, Dimension | Categorical | 13 sub-categories |
| `product_name` | String, Dimension | Categorical | Product-level detail (not used directly in dashboard, available for drill-down) |
| `ship_mode` | String, Dimension | Categorical | Standard Class / Second Class / First Class / Same Day |
| `sales` | Number (decimal), Measure | Numerical measure | ₹ |
| `quantity` | Number (whole), Measure | Numerical measure | Units sold |
| `discount` | Number (decimal), Measure | Numerical measure | Stored as a decimal (e.g. 0.15 = 15%) — formatted as percentage in Tableau |
| `profit` | Number (decimal), Measure | Numerical measure | ₹, can be negative |
| `return_flag` | Number (whole) / Boolean, Measure or Dimension | Binary/flag field | 1 = returned, 0 = not returned. Used both as a measure (for SUM/AVG = return rate) and converted to a Boolean for filtering |
| `delivery_days` | Number (whole), Measure | Numerical measure | ship_date − order_date, already pre-computed in the source data |
| `customer_rating` | Number (decimal), Measure | Numerical measure | 1–5 scale; 32 nulls (0.76% of rows) |
| `campaign_channel` | String, Dimension | Categorical | Organic / Paid / Email / Social / Referral; 24 nulls (0.57% of rows) |

### Assumptions
- `order_id` is unique per row (verified — 4,200 distinct IDs for 4,200 rows), so all order-count measures use `COUNTD([Order ID])` safely, and `SUM` vs `COUNTD` of order_id give the same result; `COUNTD` was used throughout for clarity and to be safe against any future duplicate rows.
- Null `customer_rating` (32 rows) and `campaign_channel` (24 rows) were **left as nulls** rather than imputed — Tableau excludes nulls from AVG/SUM by default, which is the correct behavior here (we don't want to assume a rating or channel that wasn't recorded).
- `discount` is stored as a decimal fraction (0–0.35 range observed), not a whole-number percentage — formatted as % in all calculated fields and chart labels.
- `region`/`state`/`city` describe the **customer's shipping location**, used as the basis for all geographic analysis (map, regional bar charts).

## Tableau Workbook Description

`tableau/executive_dashboard.twbx` is a packaged workbook containing:
- 1 data source connection to `dashboard_sales_data.xlsx`
- 7+ worksheets (one per required view, listed below)
- 1 executive dashboard combining the views with KPI cards, filters, and a filter action

## Calculated Fields Created

| Field Name | Formula | Logic |
|---|---|---|
| `Profit Margin` | `SUM([Profit]) / SUM([Sales])` | Profit divided by sales, aggregated |
| `Cost` | `[Sales] - [Profit]` | Sales minus profit, at row level |
| `Average Order Value` | `SUM([Sales]) / COUNTD([Order ID])` | Sales divided by number of distinct orders |
| `Return Rate` | `SUM([Return Flag]) / COUNTD([Order ID])` | Returned orders divided by total orders |
| `Shipping Delay Bucket` | `IF [Delivery Days] <= 1 THEN "0-1 days" ELSEIF [Delivery Days] <= 3 THEN "2-3 days" ELSEIF [Delivery Days] <= 5 THEN "4-5 days" ELSE "6+ days" END` | Categorizes delivery days into 4 meaningful buckets |
| `Discount Band` | `IF [Discount] = 0 THEN "0%" ELSEIF [Discount] <= 0.1 THEN "1-10%" ELSEIF [Discount] <= 0.2 THEN "11-20%" ELSEIF [Discount] <= 0.3 THEN "21-30%" ELSE "31%+" END` | Groups discount into bands to show the discount-vs-profit relationship cleanly |
| `Is Returned` | `[Return Flag] = 1` | Boolean version of the flag, used for filtering and color encoding |

Full explanation and business rationale for each: `outputs/business_insights.md` and `outputs/chart_selection_justification.md`.

## Dashboard Components

- **Title:** "Retail Performance Overview — FY2024–2025"
- **KPI Cards (4):** Total Sales, Total Profit, Overall Profit Margin, Return Rate — each as a single-number text/BAN (Big Ass Number) tile
- **Views (7):** Sales Trend, Regional Performance (map + bar), Category Profitability, Customer Segment Comparison, Shipping Performance, Discount vs Profit (scatter), Return Analysis
- **Filters:** Region, Category, Customer Segment, Date Range (Year/Quarter), Ship Mode — all applied to the whole dashboard
- **Interaction:** A filter action from the Regional map → all other views (clicking a region/state filters category, segment, shipping, and return views to that region)

## Filters and Interactions Used

- Global quick filters: Region, Category, Customer Segment, Ship Mode, Order Date (range)
- Dashboard action: clicking a mark on the map/regional bar chart filters the rest of the dashboard to that region/state (with a "show all values" reset, not "exclude")

## Key Business Insights

Full detail with data evidence in `outputs/business_insights.md`. Headlines:
- Furniture is the weakest-margin category (6.9%) and also has the highest return rate (7.7%) — same category, two compounding problems.
- Discounts above 30% flip overall profit margin negative (−5.0%) — discounting beyond this point is destroying margin, not just compressing it.
- Home Office customers have the highest return rate (5.7%) of the three segments, despite similar sales and ratings to Consumer and Corporate.
- South region leads in absolute sales (₹64.7M) but North/East/South/West margins are all within ~0.4 points of each other — this is a volume story, not a margin story.

## Dashboard Story Summary

Full narrative in `outputs/dashboard_story.md`. In short: overall the business is healthy (15.4% margin, steady ~₹9M/month sales), Technology is the clear profit engine, but Furniture's weak margin and elevated returns — concentrated in Tables and Bookcases — is the most actionable risk, compounded by heavy discounting above 30% turning unprofitable.

## Assumptions and Limitations

- The dataset has no `quantity returned` detail — return_flag is at the whole-order level, so we can't separate "fully returned" vs "partially returned" orders.
- `campaign_channel` reflects acquisition channel, not necessarily the channel that drove this specific order — treated as a customer-level attribute for this analysis.
- 2 years of data is enough to see a yearly pattern but not enough to confidently detect multi-year seasonality.
- Dashboard shows association, not causation — e.g., higher returns in Home Office doesn't by itself prove segment behavior is the cause; product mix purchased by that segment may differ.

## Screenshots Included

| File | Shows |
|---|---|
| `screenshots/full_dashboard.png` | Complete executive dashboard |
| `screenshots/sales_trend_view.png` | Sales trend over time |
| `screenshots/regional_performance_view.png` | Regional/state-level sales and profit |
| `screenshots/category_profitability_view.png` | Category and sub-category profitability |
| `screenshots/filter_interaction_view.png` | Evidence of filter/dashboard action interaction |

## Repository Structure

```
part4_tableau_dashboard/
├── data/dashboard_sales_data.xlsx
├── tableau/executive_dashboard.twbx
├── outputs/
│   ├── dashboard_story.md
│   ├── business_insights.md
│   └── chart_selection_justification.md
├── screenshots/
└── README.md
```
