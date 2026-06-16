# Luxury Home Market Analysis

## About This Project
This project analyzes luxury real estate trends across the top 20 U.S. states 
using Zillow listing data sourced from Kaggle. The goal was to uncover what 
defines the luxury real estate market, identify the best value deals under 
$1 million, and compare them against true luxury properties priced over $1 million.

## Main Question
What makes a home “luxury,” and where can buyers find the best luxury value?

## Tools & Technologies
| Tool | Purpose |
| Google Sheets | Data cleaning, pivot tables, and calculations |
| Google BigQuery | SQL analysis and querying |
| Tableau Public | Data visualization and dashboards |
| GitHub | Project documentation and portfolio |
- **Key Columns:** Address, State Code, Zip Code, Price, Area SQFT, 
  Bedrooms, Bathrooms, Luxury Features, Luxury/Non-Luxury Classification

  ## Data Workflow
1. **Google Sheets** — Imported raw Kaggle CSV, cleaned duplicates and 
   blank rows, standardized formatting, built pivot tables, and created 
   calculated columns including price per square foot and state code extraction
2. **Google BigQuery** — Uploaded cleaned data as an external table, 
   wrote SQL queries ranging from basic filtering to advanced window 
   functions using PARTITION BY, SPLIT, and UNNEST
3. **Tableau Public** — Exported query results as CSVs, built 7 
   visualizations, and organized them into 2 interactive dashboards


## What I Analyzed
- Price per square foot
- Most expensive ZIP codes
- Best-value homes
- Luxury features
- Deal score rankings

## Key Findings
- California leads the nation with the highest number of luxury homes for sale, reflecting its predominance in the high end real estate market.
- Montana offers the largest homes by square footage at the lowest price per square foot making it the top value state for buyers seeking space at an aevrgae of 8293 sqft for homes over $1 million.
- Aspen, Co 81611 ranks as the most expensive zip code in the dataset followed by Beverly Hills 90210, and Midtown Manhattan, 10019
- Pool is the most common luxury amenity found across high end properties, appearing in more homes than any other feature, followed by Chef's kitchen, and Quartz coutertops
- A custom deal score helped identify homes with strong value.

## Tableau Dashboard
https://public.tableau.com/shared/ZZGSCHZS4?:display_count=n&:origin=viz_share_link

View the cleaned dataset in Google sheets here:
https://docs.google.com/spreadsheets/d/1UXYFeltI4SftiTUSTPQ51Bukmr-C0yiU_L6qKJ6pA8o/edit?usp=sharing

SQL Queries Document:
https://docs.google.com/document/d/1wsetYpxkTkj-TQDCarxukszAWfEcdiar2PlRB53wauU/edit?usp=sharing

## SQL Highlights
A few examples of the queries written in Google BigQuery:

— Homes Above Zip Code Average:**
```sql
SELECT address, zip_code, price, avg_zip_price,
  ROUND(price - avg_zip_price, 2) AS above_avg_by
FROM (
  SELECT address, zip_code, price,
    AVG(price) OVER (PARTITION BY zip_code) AS avg_zip_price
  FROM  `project-9b348a78-ee0b-4c11-892.luxury_homes_analysis.luxury_homes`
)
WHERE price > avg_zip_price
ORDER BY above_avg_by DESC;
