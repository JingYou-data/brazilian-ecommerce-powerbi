# 🛒 Brazilian E-Commerce Analytics Dashboard

<p align="center">
  <img src="https://img.shields.io/badge/Power%20BI-Advanced%20DAX-F2C811?style=for-the-badge&logo=powerbi&logoColor=black">
  <img src="https://img.shields.io/badge/Dataset-99K%2B%20Orders-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Coverage-27%20Brazilian%20States-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/Period-2016–2018-orange?style=for-the-badge">
</p>

> **Executive-level Power BI dashboard analyzing Brazilian e-commerce performance across 27 states — featuring dynamic DAX narratives, radar scoring, and RANKX-based state rankings.**

🔗 **[View Live Dashboard](https://app.powerbi.com/view?r=eyJrIjoiZDljOTdkZTAtOTM1MC00MTk2LTg5NDItNWUyZjJmMTJiYTJiIiwidCI6IjEwMWRhNTg3LTE4NDMtNGY1Mi04YjhhLTE3YjA2OWM2NmQzMyIsImMiOjJ9)**

---

## 📌 Project Overview

| | |
|---|---|
| **Dataset** | Olist Brazilian E-Commerce (Kaggle) |
| **Total Records** | 1,550,800+ across 8 tables |
| **Orders Analyzed** | 99,441 |
| **Geographic Scope** | 27 Brazilian states |
| **Key Dimensions** | Revenue, Orders, AOV, Review Score, On-Time Delivery |
| **Tools** | Power BI Desktop, DAX, Power Query, Spider Chart Visual |

---

## 🗂️ Dashboard Pages

### 1. Welcome
Landing page with visual navigation to all report sections.

### 2. Market Overview
- 4 KPI cards: Total Revenue, Total Orders, Average Order Value, Review Score
- Interactive image-based metric selector (bookmark-driven)
- Dynamic trend line that updates based on selected metric
- AI-style narrative auto-generated from DAX: *"SP leads Brazilian e-commerce with R$16.01M in revenue..."*

### 3. State Performance
- Spider/Radar chart showing 5-dimension performance profile per state
- State selector with real-time radar update
- 10 KPI cards showing value + rank for each dimension
- Dynamic narrative summarizing each state's strengths and weaknesses

### 4. State Ranking
- 5 parallel bar charts ranking all 27 states across each dimension
- Click-to-highlight: select any state to update all rankings and narrative simultaneously
- Large rank indicators (#1, #2...) for fast executive scanning

---

## ⚙️ Technical Highlights

| Feature | Implementation |
|---|---|
| Dynamic narratives | DAX `SELECTEDVALUE` + `FORMAT` + conditional `IF` logic |
| Interactive metric selector | Power BI Bookmarks + Image Actions |
| State performance radar | Custom Spider Chart visual + normalized Score measures |
| State ranking | `RANKX` with `ALL()` + conditional color formatting |
| Standardized scoring | Min-Max normalization DAX (0–100 scale) |
| Cross-filter interactions | Edit Interactions panel configuration |

---

## 📐 DAX Measures

```dax
-- Dynamic narrative (Market Overview page)
Dynamic_Narrative =
VAR TopState = [Top_State]
VAR TopRevenue = FORMAT([total_revenue]/1000000, "#,##0.00") & "M"
VAR OnTimeRate = FORMAT([on_time_delivery_rate], "0%")
VAR AboveBelow = IF([on_time_delivery_rate] >= [Avg_OnTime_AllStates], "at or above", "below")
RETURN
TopState & " leads Brazilian e-commerce with R$" & TopRevenue &
" in revenue. On-time delivery rate is " & OnTimeRate & ", " & AboveBelow & " the national average."

-- Min-Max normalization for radar chart
Score_Revenue =
DIVIDE(
    [total_revenue] - MINX(ALL(olist_customers_dataset[customer_state]), [total_revenue]),
    MAXX(ALL(olist_customers_dataset[customer_state]), [total_revenue]) -
    MINX(ALL(olist_customers_dataset[customer_state]), [total_revenue])
) * 100

-- Dense rank across all states
State_Revenue_Rank =
RANKX(ALL(olist_customers_dataset[customer_state]), [total_revenue], , DESC, DENSE)
```

---

## 📦 Dataset

**Source:** [Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle)

| Table | Records | Description |
|---|---|---|
| olist_orders_dataset | 99,441 | Order status and timestamps |
| olist_order_items_dataset | 112,650 | Items per order |
| olist_order_payments_dataset | 103,886 | Payment details |
| olist_order_reviews_dataset | 99,224 | Customer reviews |
| olist_customers_dataset | 99,441 | Customer location |
| olist_sellers_dataset | 3,095 | Seller information |
| olist_products_dataset | 32,951 | Product catalog |
| olist_geolocation_dataset | 1,000,163 | ZIP code coordinates |

---

## 📈 Key Insights

- **SP (São Paulo)** dominates Revenue (#1) and Order Volume (#1), accounting for ~38% of total national revenue
- **AP (Amapá)** leads in Review Score (#1) and ranks #4 in On-Time Delivery — strong quality performance despite low market volume
- **Average Order Value** declined from R$190 (2016) to R$162 (2018), signaling market maturation and price competition
- **On-Time Delivery** national average is 92%, with significant variation across states — a key differentiator for smaller markets

---

## 📁 Repository Structure

```
Brazilian-Ecommerce/
├── README.md                    # Project documentation
├── Brazilian_Ecommerce.pbix     # Power BI report file
└── data/
    ├── olist_orders_dataset.csv
    ├── olist_order_items_dataset.csv
    ├── olist_order_payments_dataset.csv
    ├── olist_order_reviews_dataset.csv
    ├── olist_customers_dataset.csv
    ├── olist_sellers_dataset.csv
    ├── olist_products_dataset.csv
    └── olist_geolocation_dataset.csv
```

---

## 🚀 Future Work

- [ ] Migrate to **dbt + Snowflake** for scalable data modeling
- [ ] Add **seller performance** analysis page
- [ ] Build **product category** drill-through analysis
- [ ] Automate data refresh via **Power BI Service scheduled refresh**

---

## 👤 Author

**Jing You** — Data Analytics & Engineering
[![LinkedIn](https://img.shields.io/badge/LinkedIn-jing--you84-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jing-you84/)
[![GitHub](https://img.shields.io/badge/GitHub-JingYou--data-181717?style=flat&logo=github&logoColor=white)](https://github.com/JingYou-data)
[![Portfolio](https://img.shields.io/badge/Portfolio-jingyou--data.github.io-blue?style=flat)](https://jingyou-data.github.io)

---

*Built with Power BI Desktop · Dataset: Olist Brazilian E-Commerce 2016–2018*
