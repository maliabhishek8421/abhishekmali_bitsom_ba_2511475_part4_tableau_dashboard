# Chart Selection Justification

For each major view in the dashboard: the business question it answers, why the chart type fits, how fields are encoded, the design principle applied, and the mistake avoided.

---

## 1. Sales Trend View — Line Chart

- **Question answered:** How are sales (and profit) changing over time?
- **Why this chart type:** A line chart is the standard, least-ambiguous way to show a continuous time series — the eye reads slope and direction naturally, which is exactly what "is this growing or shrinking" requires.
- **Field encoding:** `Order Date` (continuous, monthly) on Columns; `SUM(Sales)` on Rows; a dual-axis or second line for `SUM(Profit)` synced to the same axis scale where appropriate; color reserved for the Sales vs. Profit distinction only (not used for anything else on this chart).
- **Design principle applied:** Consistent, minimal color palette — one color per measure, used consistently across the whole dashboard (e.g., the same blue for Sales everywhere it appears).
- **Mistake avoided:** Did not use a bar chart for the trend (bars are better for discrete comparisons, not for showing a continuous trend), and did not truncate the Y-axis to exaggerate small fluctuations — the axis starts at zero so month-to-month changes are shown at true scale.

## 2. Regional Performance View — Map + Bar Chart

- **Question answered:** Which region/state performs best on sales and profit?
- **Why this chart type:** A filled map gives an immediate geographic read (useful for a leadership audience scanning for "where" problems are), paired with a bar chart for precise ranking — maps are good for spatial pattern, bars are better for exact comparison, so using both covers both needs without forcing one chart to do two jobs.
- **Field encoding:** `State` on the map (Geographic Role: State/Province), color = `SUM(Profit)` or `Profit Margin` (sequential color scale, not diverging, since all regions are profitable); the companion bar chart has `Region` on Rows, `SUM(Sales)` on Columns, sorted descending.
- **Design principle applied:** Appropriate sorting — the bar chart is sorted by sales descending so the ranking is immediately legible, rather than left in alphabetical order which would force the reader to scan for the longest bar.
- **Mistake avoided:** Did not use a diverging red-green color scale on the map, which would visually imply some regions are "bad" (red) when in fact every region is profitable — a sequential scale (light to dark) correctly shows "more vs. less," not "bad vs. good."

## 3. Category Profitability View — Bar Chart + Highlight Table

- **Question answered:** Which categories and sub-categories drive profit, and which lag?
- **Why this chart type:** A bar chart ranks categories clearly by margin; a highlight table (category x sub-category, colored by margin) lets leadership scan all 13 sub-categories at once without needing 13 separate bars, which would clutter the view.
- **Field encoding:** `Category` on Rows, `Profit Margin` (calculated field) on Columns for the bar chart, sorted descending; the highlight table uses `Category` and `Sub-Category` as row/column headers with `Profit Margin` driving cell color (sequential, light-to-dark).
- **Design principle applied:** Clear hierarchy — Category is shown first at a high level (3 bars), then Sub-Category detail is available in the same view without requiring a separate dashboard tab, so the reader can go from summary to detail in one place.
- **Mistake avoided:** Did not use a pie chart to show category share of profit — pie charts make it hard to compare slices of similar size precisely, and here the real story is about margin, which a pie chart (built for "share of whole") cannot show at all.

## 4. Customer Segment View — Bar Chart

- **Question answered:** How do Consumer, Corporate, and Home Office segments compare on sales, margin, and return rate?
- **Why this chart type:** With only 3 categories to compare across 2-3 metrics, a simple bar chart (or small set of bars) is the clearest option — no need for anything more complex with this few data points.
- **Field encoding:** `Customer Segment` on Columns; small multiples or side-by-side bars for `SUM(Sales)`, `Profit Margin`, and `Return Rate`; color encodes segment consistently with the rest of the dashboard.
- **Design principle applied:** Minimal clutter — only the 3 metrics that matter for this comparison are shown, not every available field, so the Home Office return-rate outlier is immediately visible rather than buried among unrelated numbers.
- **Mistake avoided:** Did not put sales, margin, and return rate on the same axis/scale (they have very different units and ranges) — each metric gets its own clearly labeled axis or its own small chart, avoiding a misleading combined scale.

## 5. Shipping Performance View — Bar Chart / Table

- **Question answered:** Which shipping mode is used most, and does shipping speed relate to delivery delay or returns?
- **Why this chart type:** A bar chart for order volume by `Ship Mode`, paired with a table or second bar chart for average `Delivery Days` and `Return Rate` by `Shipping Delay Bucket` — this is a categorical comparison, which bars handle better than a line or scatter would.
- **Field encoding:** `Ship Mode` on Rows, `COUNTD(Order ID)` on Columns for volume; `Shipping Delay Bucket` (calculated field) on Rows with `Return Rate` (calculated field) on Columns for the delay-vs-returns check.
- **Design principle applied:** Focus on business interpretation over decoration — the delay-bucket table intentionally shows flat return rates across buckets, which is itself the insight (no relationship), so no chart embellishment was added to imply a trend that isn't there.
- **Mistake avoided:** Did not force a trend line onto the delay-vs-return-rate chart — fitting a trend line to four roughly flat points would visually suggest a relationship the data doesn't support.

## 6. Discount vs. Profit View — Scatter Plot

- **Question answered:** How does discount level relate to profit (or profit margin)?
- **Why this chart type:** A scatter plot is the correct chart for showing the relationship between two continuous numeric variables at the order level — this is precisely the textbook use case, and no other chart type shows individual-order variation alongside the overall trend as well.
- **Field encoding:** `Discount` on Columns (continuous), `Profit` (or `Profit Margin`) on Rows (continuous); color by `Category` to check whether the discount-profit relationship differs by category (it doesn't dramatically — confirming the pattern is general); a trend line is added here deliberately, since the data does support one (correlation -0.60).
- **Design principle applied:** Proper labeling — axis titles explicitly state units (₹ for profit, % for discount) so the steep negative relationship can't be misread.
- **Mistake avoided:** Did not bucket discount into bins for this chart (that's reserved for the summary bar chart in the insights view) — the scatter plot shows the full, ungrouped relationship so outliers (like the 31%+ loss-making orders) remain visible as individual points rather than being averaged away.

## 7. Return Analysis View — Bar Chart / Highlight Table

- **Question answered:** Which category, segment, or sub-category has the highest return risk?
- **Why this chart type:** A bar chart sorted by `Return Rate` descending makes the ranking immediately obvious; a highlight table cross-tabbing category x segment return rate shows whether the risk concentrates in a single combination or spreads broadly.
- **Field encoding:** `Sub-Category` on Rows, `Return Rate` (calculated field) on Columns, sorted descending, color also tied to `Return Rate` for a consistent read between position and color.
- **Design principle applied:** Consistent color usage — the same sequential color ramp used for margin elsewhere is reused here (just applied to a "lower is better" metric), so the dashboard doesn't force the reader to learn a new color convention per chart.
- **Mistake avoided:** Did not show return rate as a raw COUNT of returns — raw counts would make high-volume sub-categories look artificially risky; using a rate (returns divided by orders) correctly normalizes for volume so Bookcases (8.4% rate, 24 returns) is comparable to Copiers (3.0%-range, more total orders).

## General Principles Applied Dashboard-Wide

- **Color consistency:** Sales = one fixed color, Profit = a second fixed color, used identically across every view they appear in.
- **No 3D, no decorative chart junk:** every chart type was chosen because it answers a specific question, not for visual variety.
- **All monetary values formatted consistently** (Rupee symbol, thousands separators) and all percentages shown to one decimal place, dashboard-wide.
- **Sorting applied wherever ranking matters** (regions, categories, sub-categories, ship modes) so the most important bar is never buried in alphabetical order.
