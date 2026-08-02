# 🛒 Customer Shopping Behavior Data Analytics Project

## 📌 Overview

This project delivers an end-to-end data analysis workflow examining customer shopping behavior. By analyzing purchasing patterns, demographic segments, promotional responsiveness, and product performance, raw retail data is transformed into actionable business strategies to boost revenue, customer retention, and marketing efforts.

---

## 📊 Dataset

* 📁 **Source:** Customer Shopping Behavior Dataset (`customer_shopping_behavior.csv`)


* 📏 **Size:** 3,900 records across 18 initial feature columns


* 🔑 **Key Attributes:** `Customer ID`, `Age`, `Gender`, `Item Purchased`, `Category`, `Purchase Amount (USD)`, `Location`, `Review Rating`, `Subscription Status`, `Shipping Type`, `Discount Applied`, `Previous Purchases`, `Payment Method`, and `Frequency of Purchases`


---

## 🛠️ Tools & Technologies

* **Python (Pandas, NumPy):** Data ingestion, exploratory data analysis (EDA), cleaning, and feature engineering


* **PostgreSQL / Supabase:** Database hosting, advanced SQL queries, window functions, and customer segmentation


* **Power BI:** Interactive visualization dashboards and KPI tracking

---

## 🚀 Steps & Methodology

### 1. Data Cleaning & Feature Engineering (Python)

* 🧹 **Missing Values:** Imputed missing `Review Rating` values using the median rating of their respective product `Category`.


* 🔤 **Normalization:** Standardized column names to lowercase snake_case for smooth database ingestion.


* ✂️ **Redundancy:** Removed `promo_code_used` after confirming full correlation with `discount_applied`.


* 🏷️ **New Features:**
* Binned `age` into 4 distinct groups (`Young Adult`, `Adult`, `Middle-aged`, `Senior`).


* Mapped categorical purchase frequencies into numeric day counts (`purchase_frequency_days`).





```python
# Quick snippet: Imputation & Feature Engineering
df['review_rating'] = df.groupby('category')['review_rating'].transform(lambda x: x.fillna(x.median()))
df['age_group'] = pd.qcut(df['age'], q=4, labels=['Young Adult', 'Adult', 'Middle-aged', 'Senior'])

```

---

### 2. Database Ingestion & Analytical Queries (SQL)

Executed analytical queries in PostgreSQL / Supabase to extract key business insights:

```sql
-- Top 3 items per category using Window Functions
WITH item_counts AS (
    SELECT category, item_purchased, COUNT(customer_id) AS total_orders,
           ROW_NUMBER() OVER (PARTITION BY category ORDER BY COUNT(customer_id) DESC) AS item_rank
    FROM customer GROUP BY category, item_purchased
)
SELECT item_rank, category, item_purchased, total_orders 
FROM item_counts WHERE item_rank <= 3;
```[cite: 2]

---

## 📈 Power BI Dashboard
An interactive, multi-view dashboard highlighting core performance metrics:
* 🎯 **Executive Summary:** Overall KPI cards for total revenue, average order value, customer counts, and ratings.
* 👥 **Customer Demographics:** Breakdown of revenue by gender and age segments[cite: 2].
* 🏷️ **Promotions & Subscriptions:** Discount vs. full-price spend analysis and subscriber behaviors[cite: 2].
* 📦 **Product & Shipping:** Top-rated items, category rankings, and shipping preferences (Standard vs. Express)[cite: 2].

---

##💡 Key Business Insights
* 🎯 **Targeting:** Middle-aged customers contribute heavily to revenue, pointing to strong opportunities for loyalty programs[cite: 2].
* 🏷️ **Promotions:** Customers using discounts frequently spend above the average order value, showing promos successfully drive larger cart sizes[cite: 2].
* 📦 **Product Focus:** Clothing and Accessories drive the highest total order volumes[cite: 1, 2].

---

## 💻 How to Run

### Prerequisites
* Python 3.x (`pandas`, `numpy`, `jupyter`)[cite: 1]
* PostgreSQL database or Supabase instance
* Power BI Desktop

### Quick Start
1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/customer-shopping-analytics.git
   cd customer-shopping-analytics

```

2. **Run data cleaning:**
```bash
jupyter notebook customer_shopping_behavior.ipynb

```


3. **Database Setup:** Load cleaned output into your PostgreSQL/Supabase database and execute scripts in `sql/queries.sql`.
4. **Dashboards:** Open `reports/Customer_Analytics_Dashboard.pbix` in Power BI Desktop.
