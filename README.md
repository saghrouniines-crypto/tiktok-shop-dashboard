# TikTok Shop — Sales and Market Dashboard

An end-to-end data analytics project scraping TikTok Shop product listings, transforming unstructured JSON into a cleaned relational dataset, and building an interactive Power BI dashboard to analyze sales drivers, market concentration, and pricing dynamics.

![TikTok Shop Dashboard Preview](images/dashboard.png)

---

## 📌 Executive Summary & Key Findings

* **Extreme Revenue Concentration:** Skin care and Makeup generates **96% of total estimated revenue** ($612.8M out of $638.0M) while accounting for **49% of total products** (81 of 164).
* **Superstar Merchant Dominance:** A tiny fraction of sellers drive the market—`medicube US Store` single-handedly accounts for **~51% of total revenue**, and the **Top 5 shops combined generate ~85%**.
* **Category Over Price Point:** Category selection is the primary determinant of sales volume. On a logarithmic scale, Skin Care & Makeup products consistently achieve between 1,000 and 1,000,000 units sold. In contrast, most products in Clothes, Others, and Phone Accessories sell under a few hundred units regardless of price point.
* **Limited Discounting Strategy:** Only **50 out of 164 products** feature promotional discounts. The average discount sits at **~14% for Skin Care & Makeup** and **~13% for Clothes**, whereas Phone Accessories products offer virtually zero discounts.

---

## 🛠️ Data Pipeline & Architecture

### 1. Data Collection
* Scraped product listings from TikTok Shop using an Apify Actor on **November 1, 2026**.
* Extracted raw payload output as structured and nested JSON.

### 2. ETL & Data Transformation (Python & Power Query)
* **Python (`pandas`):** Flattened nested JSON dictionaries and lists into a single tabular CSV format (`scripts/json_to_csv.py`).
* **Power Query Data Cleaning:**
  * Replaced blank/null categorical values with `"No Label"`.
  * Replaced `score = 0` values with `null` to distinguish between unrated products and bad ratings.
  * Dropped the `hasStock` column due to zero variance across records.
  * Filtered out **2 extreme price outliers** (~$9,880/product) to prevent severe skewing of visual distributions.

### 3. Data Modeling & DAX (Power BI)
Created calculated fields and measures, including:
- `Revenue = soldCount * Sale_Price`
- `Discount % = 1 - (Sale_Price / Original_price)`
- Created boolean flag indicators (Yes/No) from string tags for **Free Shipping**, **Flash Sale**, and **Limited Deal**.
- Generated a non-zero transformed column for `soldCount` to enable logarithmic scale representation on scatter plots.
- 
### 4. UI/UX Design
- Designed a custom dark-mode aesthetic (`theme/TikTok_Shop_Theme.json`) based on TikTok's brand palette (Black `#000000`, Cyan `#00F2FE`, and Magenta `#FE2C55`).


---

## 📊 Dashboard Features & Content

* **KPI Header Cards:** Total Estimated Revenue, Total Units Sold, Product Count, Average Score.
* **Category Breakdown:** Units sold and total revenue distribution across product types.
* **Logarithmic Scatter Plot:** `Price` vs. `Units Sold` grouped by category to evaluate elasticity.
* **Top 5 Merchants:** Ranked bar chart showcasing store-level revenue concentration.
* **Promotional Analysis:** Average discount percentage grouped by category.
* **Interactive Slicers:** Dynamic filtering by Free Shipping, Flash Sale, and Limited Deal status.

---

## 📂 Repository Structure

```text
tiktok-shop-dashboard/
├── README.md
├── dashboard/
│   └── tiktok.pbix
├── images/
│   └── dashboard.png
├── data/
│   └── Tiktok_shop.csv
├── scripts/
│   └── json_to_csv.py
└── theme/
    └── TikTok_Shop_Theme.json

## 🚀 How to Run & View

1. Download the `dashboard/tiktok.pbix` file.
2. Open the file in **Power BI Desktop** (Free).
3. *If prompted for broken data sources:* Navigate to **Home → Transform data → Data source settings** and update the file path to point to your local `data/Tiktok_shop.csv`.

---

## ⚠️ Data Limitations & Scope

* **Revenue Estimate:** Revenue is calculated as $(\text{Units Sold} \times \text{Current Sale Price})$. It represents a snapshot estimate rather than true historical cumulative earnings.
* **Sample Size:** The dataset contains 164 cleaned products from a single point-in-time scrape on **November 1, 2026**, serving as a sample rather than the complete global market.
* **Logarithmic Exclusions:** Two products with `0` units sold are excluded from the scatter plot visualization due to log-scale math constraints ($\log(0)$ is undefined).
* **Correlation vs. Causation:** Findings reflect empirical associations within this specific dataset and do not imply absolute causal mechanics.

---

## 👨‍💻 Tech Stack

* **Language:** Python (`pandas`)
* **Scraping:** Apify
* **ETL & Data Prep:** Power Query
* **BI & Visualizations:** Power BI, DAX, Custom JSON Themes

---

## 👤 Author

**Ines Saghrouni**  
*Master's Student in Social Sciences & Digital Data* — University of Padua  
🔗 **GitHub:** [@saghrouniines-crypto](https://github.com/saghrouniines-crypto)
