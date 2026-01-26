# The Cost of Living Crisis: A Data-Driven Analysis

## The Problem

The official Consumer Price Index (CPI) is designed to measure inflation for the "average" American consumer. But the average consumer doesn't exist—and certainly doesn't look like a college student.

Students face a unique cost structure: we pay tuition bills that rival mortgage payments, rent apartments in expensive college towns, buy groceries on tight budgets, and yes—even the Red Solo Cups at our weekend gatherings have doubled in price. Yet when economists report that "inflation is cooling," they're measuring a basket of goods that barely resembles our reality.

**Why does the CPI fail students?**
- The CPI weights housing at ~34%, but for students, tuition + rent can exceed 65% of our budget
- Education receives only ~6% weight in the official CPI, despite being our largest expense
- The "average consumer" is a 40-year-old homeowner—not a 20-year-old renter paying $1,800/month

This project asks a simple question: **What if we built our own inflation index—one that actually reflects how students spend money?**

## Methodology

**Tools:** Python, pandas, matplotlib, Federal Reserve Economic Data (FRED) API

**The Approach:**

1. **Manual Basket Construction:** I created a "Student Basket" of five expense categories with real 2016 and 2024 prices:

   | Item | 2016 Price | 2024 Price | Inflation |
   |------|-----------|-----------|-----------|
   | Tuition | $45,000 | $58,000 | 28.9% |
   | Rent (1 Bed) | $1,200/mo | $1,800/mo | 50.0% |
   | Groceries | $200/mo | $400/mo | 100.0% |
   | Textbooks | $1,230/yr | $1,240/yr | 0.8% |
   | Red Solo Cups (50ct) | $5.99 | $12.00 | 100.3% |

2. **FRED API Integration:** I fetched official BLS price indices as proxies for each category:
   - `CPIAUCSL` → Official CPI
   - `CUSR0000SEEB` → Tuition proxy
   - `CUSR0000SEHA` → Rent proxy
   - `CUSR0000SAF11` → Groceries proxy
   - `CUSR0000SEEA` → Textbooks proxy
   - `CUSR0000SAH3` → Household supplies proxy (Red Solo Cups)

3. **Normalization:** Raw CPI indices have different base years (1982-84, 2001, etc.), making direct comparison meaningless. I re-indexed all series to **January 2016 = 100** using the formula:
Value_Index = (Value_Current / Value_at_Start_Date) * 100

4. **Laspeyres-Style Weighted Index:** I constructed a custom **Student Price Index (SPI)** using weights that reflect actual student spending:

   | Category | Weight | Rationale |
   |----------|--------|-----------|
   | Tuition | 35% | Largest annual expense |
   | Rent | 30% | Largest monthly expense |
   | Groceries | 20% | Essential recurring cost |
   | Household Supplies | 10% | Cleaning, party supplies, misc. |
   | Textbooks | 5% | Declining due to digital options |

   The Laspeyres price index measures price changes using a fixed base-period quantity basket—exactly what the official CPI does, but with weights that actually match student budgets.

5. **Regional Analysis:** I compared my Student SPI against both the National CPI and the Boston-Cambridge-Newton regional CPI to test for geographic disparities.

## Key Findings

**My analysis reveals a ~5-10 percentage point divergence between Student Costs and National Inflation since January 2016.**

| Index | Cumulative Inflation (2016-2024) |
|-------|----------------------------------|
| National CPI | ~30% |
| Boston CPI | ~32% |
| **Student SPI** | **~35-38%** |

### What's Driving the Gap?

1. **Rent is the biggest culprit.** The Rent CPI increased ~40-50% since 2016, and rent consumes 30% of a typical student budget. The official CPI only weights housing at 34%—and that includes homeowners who benefit from fixed mortgages.

2. **Groceries have exploded.** My personal grocery spending doubled from $200 to $400/month. The official Food-at-Home CPI confirms a ~25-30% increase, but that understates the impact for students who can't buy in bulk or take advantage of warehouse clubs.

3. **Even Solo Cups doubled.** This isn't just about parties—it's a proxy for all the small household items that have quietly inflated. The Household Furnishings & Operations index shows significant increases that don't make headlines.

4. **Textbooks are the lone bright spot.** Thanks to digital alternatives, rentals, and open educational resources (OER), textbook costs have remained nearly flat. This is the one category where students have actually won.

### The Scale Fallacy

One critical lesson from this analysis: **never compare raw CPI indices with different base years.** When I plotted raw Tuition (~900) against raw Streaming (~100), it looked like tuition was 9x higher. That's a "data crime"—the indices simply have different starting points. Normalization is essential for honest comparison.

## Conclusion

The official CPI tells us inflation is moderating. But for students, the lived experience is different. When your two biggest expenses—tuition and rent—are rising faster than the average, and you're too young to benefit from home equity or retirement accounts, the "cooling inflation" narrative rings hollow.

This project demonstrates that **who you are determines what inflation means to you.** The average doesn't exist. Build your own index.

## Data Sources

- [Federal Reserve Economic Data (FRED)](https://fred.stlouisfed.org/)
- [College Board: Trends in College Pricing](https://research.collegeboard.org/trends/college-pricing)
- [USDA Economic Research Service: Food Price Outlook](https://www.ers.usda.gov/data-products/food-price-outlook/)
- [U.S. Bureau of Labor Statistics: CPI](https://www.bls.gov/cpi/)

## Author

Joao Andrade Merjam | Northeastern University | Economics 3916: Data Foundations

January 2025
