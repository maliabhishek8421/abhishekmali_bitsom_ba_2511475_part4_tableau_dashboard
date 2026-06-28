# Business Insights

All figures below are computed directly from `data/dashboard_sales_data.xlsx` (4,200 orders, Jan 2024 – Dec 2025). Each insight includes the calculated fields used: `Profit Margin`, `Return Rate`, `Average Order Value`, `Shipping Delay Bucket`, `Discount Band`.

---

## 1. Sales Trend

**Observation:** Sales are stable year-over-year with seasonal dips, not a consistent growth or decline trend.

**Data evidence:** Quarterly sales range from a low of ₹23.3M (2025 Q2) to a high of ₹29.6M (2025 Q3), with no clear upward or downward trajectory across the 8 quarters (2024 Q1: ₹29.0M → 2025 Q4: ₹29.4M). Monthly sales dip noticeably in April–June and August of both years.

**Business interpretation:** The business is in a steady state, not a growth phase. The April–June dip recurring in both years suggests a real seasonal pattern (possibly post-quarter-end budget cycles or a seasonal lull in B2B/B2C purchasing) rather than random noise.

**Recommended action:** Investigate the April–June dip with a targeted promotional push or campaign timing shift next year, and monitor whether Q3 2025's strong rebound (₹29.6M, the highest quarter in the dataset) was driven by a specific campaign or category that can be repeated.

---

## 2. Regional Performance

**Observation:** South region leads in absolute sales and profit, but profit margins are nearly identical across all four regions.

**Data evidence:** South: ₹64.7M sales / 15.44% margin. North: ₹54.6M / 15.24%. West: ₹48.9M / 15.14%. East: ₹48.9M / 15.55%. The margin spread across regions is just 0.41 percentage points — far tighter than the sales spread.

**Business interpretation:** Regional differences are almost entirely about **volume/scale**, not regional pricing or cost efficiency — East is actually the most profitable per rupee of sales despite having the lowest absolute sales, tied with West.

**Recommended action:** Treat East and West as growth opportunities (good margins, lower volume) rather than problem regions. Investigate what's limiting order volume there — store/warehouse coverage, marketing spend allocation, or customer density — since the underlying unit economics are sound.

---

## 3. Category / Sub-Category Profitability

**Observation:** Technology drives the large majority of profit; Furniture is a consistent margin laggard across every sub-category.

**Data evidence:** Technology: ₹154.0M sales, 18.22% margin. Office Supplies: ₹11.5M sales, 14.85% margin. Furniture: ₹51.6M sales, only 6.89% margin. Within Furniture, Tables (5.67% margin) and Bookcases (5.71% margin) are the two weakest sub-categories in the entire dataset — well below Furniture's already-low category average.

**Business interpretation:** Furniture is generating meaningful revenue (₹51.6M, the second-largest category by sales) but converting it into profit far less efficiently than Technology or Office Supplies. This isn't one weak sub-category dragging the average down — all four Furniture sub-categories (Tables, Bookcases, Furnishings, Chairs) sit between 5.7% and 8.2% margin, well below every Technology sub-category (all above 18%).

**Recommended action:** Review Furniture pricing and cost structure specifically for Tables and Bookcases — these may be carrying disproportionate freight/handling costs, deep discounting, or thin supplier margins. A targeted cost or pricing review here has more upside than a similar review in Technology.

---

## 4. Customer Segment Behavior

**Observation:** All three segments generate similar sales and margin, but Home Office customers return orders meaningfully more often.

**Data evidence:** Consumer: ₹71.9M sales, 15.34% margin, 3.91% return rate. Corporate: ₹70.6M sales, 15.18% margin, 4.00% return rate. Home Office: ₹74.5M sales, 15.51% margin, 5.67% return rate — roughly 45% higher than Consumer's rate.

**Business interpretation:** Home Office is not a problem segment in terms of revenue or margin — it's actually the largest by sales — but its elevated return rate is an outlier worth understanding, since each return carries fulfillment and reverse-logistics cost not captured in the margin figure above.

**Recommended action:** Pull a sample of Home Office returns to check whether they cluster in specific sub-categories (e.g., furniture bought for home offices) or reasons — this would clarify whether it's a product-fit issue or something specific to how this segment shops.

---

## 5. Discount Impact

**Observation:** Profit margin declines steadily as discount increases, and discounts above 30% push the business into a net loss.

**Data evidence:** At 0% discount, overall margin is 20.56%. At 1-10% discount: 17.01%. At 11-20%: 13.63%. At 21-30%: 8.05%. At 31%+: -4.95% (a net loss across all orders in that band, on ₹2.07M of sales). The correlation between discount and profit margin across all orders is -0.60 (a clear negative relationship, not just at the band level).

**Business interpretation:** Discounting has a consistent, almost linear cost to margin — and there is a real breakpoint above 30% where the business loses money outright on those orders, not just earns less.

**Recommended action:** Cap discretionary discounting at 30% as a hard guardrail, or require manager approval above that threshold. Investigate the 64 orders in the 31%+ band specifically — if they cluster in one category or region, that's a process/approval gap to close.

---

## 6. Shipping / Delivery Performance

**Observation:** Standard Class is by far the most-used shipping mode and also has the slowest average delivery time, but slower shipping does not clearly correlate with higher returns.

**Data evidence:** Standard Class: 2,435 orders (58% of all orders), 4.71 average delivery days. Same Day: 241 orders, 0.40 average days. Return rates by delivery-day bucket are flat: 0-1 days = 4.33%, 2-3 days = 5.09%, 4-5 days = 4.14%, 6+ days = 4.80% — no clear upward trend as delivery takes longer.

**Business interpretation:** Slower shipping is not, on this evidence, a meaningful driver of returns — the return rate doesn't increase monotonically with delivery delay. The business's shipping mix is dominated by the slowest option, which is more a cost/logistics question than a customer-satisfaction risk based on this data.

**Recommended action:** Before investing in faster shipping as a returns-reduction lever, validate with direct customer feedback — the data here doesn't support that link. Separately, since Standard Class carries the bulk of volume, any shipping cost optimization effort should focus there for the largest impact.

---

## 7. Return Pattern

**Observation:** Returns are concentrated in Furniture, and within Furniture, in Bookcases, Furnishings, and Chairs specifically — the same category that already has the weakest margin.

**Data evidence:** Furniture return rate: 7.67% (88 of 1,147 orders) vs. Office Supplies 3.65% and Technology 3.03%. Within sub-categories, the four highest return rates are all Furniture: Bookcases (8.42%), Furnishings (8.16%), Chairs (7.99%), Tables (6.16%) — each more than double the lowest sub-category (Art, 3.05%).

**Business interpretation:** Furniture has a compounding problem: low margin and high returns, and each return on an already-thin-margin sale erodes profit further (and may not even be captured fully in the margin figure, since reverse logistics cost isn't itemized in this dataset). This is the single clearest area of concern on the whole dashboard.

**Recommended action:** Prioritize a root-cause review of Furniture returns — likely candidates are damage in transit (large/heavy items), product-listing mismatches (size, color, assembly expectations), or quality issues. This is worth investigating before the discount or shipping questions above, since it touches both profitability and customer experience.

---

## 8. Business Risk / Opportunity

**Observation:** The single biggest visible risk is the combination of Furniture's low margin and high return rate; the single biggest visible opportunity is replicating Technology's margin performance in other categories, and growing East/West region volume.

**Data evidence — Risk:** Furniture is 23.8% of total sales (₹51.6M of ₹217.0M) but only 10.7% of total profit (₹3.56M of ₹33.3M), while carrying double the return rate of the other two categories. Opportunity: Technology converts sales to profit at 18.2% margin vs. the company average of 15.4%; East region matches South's margin (15.55% vs 15.44%) on roughly 75% of South's order volume, suggesting room to grow East without a margin penalty.

**Business interpretation:** The dashboard shows a business that is fundamentally healthy (no category or region is unprofitable overall) but has one clear underperformer (Furniture) dragging down what would otherwise be a stronger blended margin, and untapped volume potential in regions that already prove they can sell profitably.

**Recommended action:** Leadership should treat Furniture margin/returns as the top remediation priority for next quarter, and treat East/West regional expansion as the top growth opportunity to pursue in parallel — both are supported directly by this dashboard's evidence, not assumptions.
