# 🛒 Brazilian E-Commerce Analytics Dashboard

An interactive Power BI dashboard analyzing the performance of Brazilian e-commerce across 27 states, built using the Olist public dataset (2016–2018).

🔗 **[View Live Dashboard](https://app.powerbi.com/view?r=eyJrIjoiZDljOTdkZTAtOTM1MC00MTk2LTg5NDItNWUyZjJmMTJiYTJiIiwidCI6IjEwMWRhNTg3LTE4NDMtNGY1Mi04YjhhLTE3YjA2OWM2NmQzMyIsImMiOjJ9)**

---

## 📊 Project Overview

This project transforms raw Brazilian e-commerce transaction data into an executive-level analytics dashboard. It enables stakeholders to explore state-level performance across five key dimensions, identify market leaders, and surface actionable insights through dynamic DAX-powered narratives.

---

## 🗂️ Dashboard Pages

### 1. Welcome
Landing page with navigation to all report sections.

### 2. Market Overview
- 4 KPI cards: Total Revenue, Total Orders, Average Order Value, Review Score
- Interactive image-based metric selector (bookmark-driven)
- Dynamic trend line chart that updates based on selected metric
- AI-style narrative: *"SP leads Brazilian e-commerce with R$16.01M in revenue..."*

### 3. State Performance
- Spider/Radar chart showing 5-dimension performance profile per state
- State selector with real-time radar chart update
- 10 KPI cards: value + rank for each dimension
- Dynamic narrative summarizing state strengths and weaknesses

### 4. State Ranking
- 5 parallel bar charts ranking all 27 states across each dimension
- Click-to-highlight: select any state to update rankings and narrative
- Large rank indicators (#1, #2...) for quick scanning
- Dynamic narrative: *"RJ ranks #2 in Revenue and #2 in Orders..."*

---

## ⚙️ Technical Highlights

| Feature | Implementation |
|--------|---------------|
| Dynamic narratives | DAX `SELECTEDVALUE` + `FORMAT` + conditional `IF` logic |
| Interactive metric selector | Power BI Bookmarks + Image Actions |
| State performance radar | Custom Spider Chart visual + normalized Score measures |
| Cross-page highlight | `RANKX` with `ALL()` + conditional color formatting |
| Standardized scoring | Min-Max normalization DAX (0–100 scale) |
| Cross-filter interactions | Edit Interactions panel configuration |

---

## 📐 DAX Measures (Selected)

```dax
-- Dynamic narrative for Market Overview page
Dynamic_Narrative =
VAR TopState = [Top_State]
VAR TopRevenue = FORMAT([total_revenue]/1000000, "#,##0.00") & "M"
VAR OnTimeRate = FORMAT([on_time_delivery_rate], "0%")
VAR AboveBelow = IF([on_time_delivery_rate] >= [Avg_OnTime_AllStates], "at or above", "below")
RETURN
"" & TopState & " leads Brazilian e-commerce with R$" & TopRevenue &
" in revenue. On-time delivery rate is " & OnTimeRate & ", " & AboveBelow & " the national average."

-- Min-Max normalization for radar chart
Score_Revenue =
DIVIDE(
    [total_revenue] - MINX(ALL(olist_customers_dataset[customer_state]), [total_revenue]),
    MAXX(ALL(olist_customers_dataset[customer_state]), [total_revenue]) -
    MINX(ALL(olist_customers_dataset[customer_state]), [total_revenue])
) * 100

-- State ranking with dense rank
State_Revenue_Rank =
RANKX(ALL(olist_customers_dataset[customer_state]), [total_revenue], , DESC, DENSE)
```

---

## 📦 Dataset

**Source:** [Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle)

| Table | Records | Description |
|-------|---------|-------------|
| olist_orders_dataset | 99,441 | Order status and timestamps |
| olist_order_items_dataset | 112,650 | Items per order |
| olist_order_payments_dataset | 103,886 | Payment details |
| olist_order_reviews_dataset | 99,224 | Customer reviews |
| olist_customers_dataset | 99,441 | Customer location |
| olist_sellers_dataset | 3,095 | Seller information |
| olist_products_dataset | 32,951 | Product catalog |
| olist_geolocation_dataset | 1,000,163 | ZIP code coordinates |

---

## 🧰 Tech Stack

- **Power BI Desktop** — Report development
- **DAX** — Advanced measures, dynamic narratives, normalization
- **Power Query (M)** — Data transformation and modeling
- **Spider Chart** — Custom visual for radar/pentagon chart
- **Power BI Service** — Publishing and public sharing

---

## 📈 Key Insights

- **SP (São Paulo)** dominates in Revenue (#1) and Order Volume (#1), accounting for ~38% of total national revenue
- **AP (Amapá)** leads in Review Score (#1) and On-Time Delivery (#4) despite low market volume
- **AOV** has declined from R$190 (2016) to R$162 (2018), suggesting market maturation and price competition
- **On-Time Delivery** national average is 92%, with significant variation across states

---

## 👤 Author

**Jing You**
Data Analytics & Engineering | Nashville, TN
[GitHub](https://github.com/JingYou-data) · [LinkedIn](https://www.linkedin.com/in/jing-you84/))

---

*Built with Power BI · Dataset: Olist Brazilian E-Commerce 2016–2018*
