<p align="center">
  <img src="Brand/mavuno_logo_horizontal.png" alt="Mavuno Extension Programme" width="360">
</p>

# Kenya Smallholder Crop Performance

**What makes one smallholder farm earn seven times more per acre than another?** This project cleans a messy dataset of 500 Kenyan farms in Excel Power Query and answers that question with a Tableau dashboard for a fictional farmer extension programme, Mavuno.

- **Interactive dashboard:** [Tableau Public](https://public.tableau.com/app/profile/joel.mutiso/viz/Kenya_Crops/MavunoDashboard)
- **Case study:** [Portfolio page](https://joeldataportfolio.streamlit.app/kenya_crops)

**Tools:** Excel Power Query (M) · Tableau (calculated fields, LOD expressions, table calculations)

---

## The data

500 smallholder farmers for the 2023 season: county, crop, variety, acreage, yield per acre, market price, revenue, cost, profit, planting and harvest dates, soil, irrigation, fertiliser, pest control and weather impact.

The data was messy: inconsistent labels, counties recorded as errors, missing values, US-format dates and harvests dated before planting.

## At a glance

| Revenue | Profit | Yield | Farmers | Irrigation premium |
|---|---|---|---|---|
| **KES 1.19B** | **KES 1.10B** | **12.0M kg** | **500** | **+27%** per acre |

Profit is reported by 404 of the 500 farms. Median yield is 2,556 kg per acre.

---

## Findings

### 1. What separates the best farms from the rest?

**Doing well on both yield and price.** Farms above average on both (Stars) earn about **KES 558K per acre**. Farms below average on both (Struggling) earn about **KES 80K**: roughly 7× less.

| Quadrant | Yield | Price | Profit per acre |
|---|---|---|---|
| Stars | High | High | KES 558K |
| Volume Growers | High | Low | KES 219K |
| Premium Sellers | Low | High | KES 215K |
| Struggling | Low | Low | KES 80K |

Being strong on just one earns about the same either way, so the jump only comes from having both. Yield and price are unrelated (R² = 0.004): high-yield farms don't sell for less.

### 2. How much do yield and price each matter?

**About equally.** On the 331 farms with a known county and crop:

| Lever | Pattern | R² | p-value |
|---|---|---|---|
| Price | KES 10/kg more → ~KES 24K more profit per acre | 0.43 | < 0.0001 |
| Yield | 1,000 kg/acre more → ~KES 95K more profit per acre | 0.38 | < 0.0001 |

Price varies widely between farmers growing the same crop: crop type explains only 2% of price differences. How and where farmers sell likely matters more than what they grow.

### 3. Which farming practice makes a clear difference?

**Irrigation.** Flood, sprinkler and drip all earn KES 275–283K per acre. Rain-fed farms earn KES 219K. The method matters little (about 3% apart), so the insight is to irrigate, not which system to use.

### 4. Do fertiliser and pest control pay off?

**Not clearly.** CAN farms earn the most per acre, but farms using no fertiliser beat those using manure or urea. Farms with no pest control earn about the same as those using organic or chemical control.

### 5. Where are the strongest and weakest areas?

**Nyeri leads** on profit per acre (KES 326K) and **Kericho trails** (KES 186K). The crop × county heatmap shows where each crop does well, but most pairs hold only 1–5 farms, so single cells are leads to investigate, not conclusions.

### 6. Does farm size matter?

**No.** Bigger farms are not more productive per acre (r = −0.03).

---

## Challenges and how I solved them

| Challenge | What I did |
|---|---|
| **A very dirty dataset** | Kept the raw import untouched, built a Data Quality Audit query to measure the problems, then standardised text, fixed types and converted dates to day-month order |
| **Missing values: zero or unknown?** | Kept blanks as null instead of 0 or an average, checked whether revenue and profit could be rebuilt from yield, price and cost and grouped unmatched records as "Unknown" |
| **Impossible dates** | Added a Date Flag column for usable planting-to-harvest windows (51% of records) and calculated Days to Harvest only for those |
| **Charts that disagreed with themselves** | Quadrant colours used FIXED averages, which ignore filters, while the reference lines followed them. Context filters made both use the same farms |
| **Ratios that lie** | Divided profit only by acres from farms that report profit: `SUM(IF NOT ISNULL(...))`, and used ratios of totals instead of averages of ratios |
| **Spotting unreliable data** | Margins near 98% looked too good. Correlations showed revenue and profit move together perfectly (r = 1.00) and cost doesn't grow with farm size (r = 0.04), so cost was flagged as a caveat |

---

## Recommendations

- **Help farmers sell better, not just grow more.** Price explains the largest share of the profit gap (43%) and varies widely within the same crop. Link farmers to cooperatives, storage and direct buyers.
- **Raise yields, starting with irrigation.** Yield explains 38% of the gap and irrigation is the only practice with a clear link to profit (+27% per acre).
- **Work on both levers together.** Farms strong on both yield and price earn about 7× more per acre than farms weak on both.
- **Target pest control instead of spraying routinely**, since spraying shows no profit benefit on average.
- **Trial CAN on matched plots before promoting it.** Its lead may come from which crops use it.
- **Fix data capture before next season:** real production costs, who each farmer sold to and when, validated dates and standard county and crop lists.

## Limitations

- Cost of production looks unreliable, so profit should be read as close to revenue.
- Only 51% of records have valid planting and harvest dates.
- There is one season of data (2023), so year-on-year comparisons aren't possible.
- Farmers chose their own practices, so differences are patterns, not proof of cause.
- Fertiliser cost assumes KES 5 per kg of yield, because no rate was given.

---

## Files

| File | What it is |
|---|---|
| `Kenya_Crops_Dataset.csv` | The original raw data |
| `Kenya_Crops_Analysis.xlsx` | Power Query workbook: Raw_crops, DQ_Audit, Crops_Clean and Date_2023 |
| `PQ_backup.txt` | The M code for every query, readable without Excel |
| `Kenya_Crops.twb` | Tableau workbook. Open it with the Excel file in the same folder |
| `Brand/` | Mavuno logo and emblem (SVG, PNG and JPG) |

**Brand palette:** Deep Leaf `#1F5E3B` · Harvest Gold `#E3A72F` · Fresh Shoot `#6BA368` · Rich Soil `#7A4E2D` · Clay Red `#B5452B` · Mist `#F4F1E8`

*Mavuno Extension Programme is fictional, created for this project.*
