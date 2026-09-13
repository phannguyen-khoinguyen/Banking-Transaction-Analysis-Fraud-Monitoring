# Banking Transaction Analysis & Fraud Monitoring at Xóm Bank | Power BI

**Author:** Phan Nguyễn Khôi Nguyên

**Date:** August 2026

**Tools Used:** Power BI

## Table of Contents
1. [📌 Background & Overview](#background--overview)
2. [📂 Dataset Description & Data Structure](#dataset-description--data-structure)
3. [📊 Key Insights & Visualizations](#key-insights--visualizations)
4. [🔎 Final Conclusion & Recommendation](#final-conclusion--recommendation)

---

## 📌 Background & Overview

**📖 What is this project about?**

This project analyzes banking transaction data to support customer behavior monitoring, revenue tracking, and fraud/risk detection. The Power BI dashboard is built around three core themes:

- **Transaction Overview**: Tracking total transaction volume, value, and refund trends over time, with year-over-year comparison and forecasting.
- **Transaction Behaviors**: Understanding how customers pay, where they spend, and how different customer segments behave.
- **Risk & Fraud**: Monitoring refund rates, high-risk transactions, and flagged merchants to catch anomalies early.

**👤 Who is this project for?**

This dashboard is designed for key stakeholders involved in banking operations, including:

- **Risk & Fraud Team**: To monitor refund rates, high-risk transactions, and flagged merchants in near real-time.
- **Business/Product Team**: To understand customer payment behavior and merchant category trends.
- **Board of Directors (BOD)**: To gain a high-level view of transaction performance and year-over-year growth.

**❓ Business Questions:**

- How are total transaction volume and value trending month over month, and how does this compare to last year?
- Which payment methods and merchant categories drive the most spending?
- Which customer segments (by income, credit score) contribute the most value and transactions?
- Are there unusual spikes in refund rate or high-risk transactions that signal fraud?
- Which merchants and states carry the highest refund risk?

**🎯 Project Outcome:**

The project provided insights into **transaction trends**, **customer payment behavior**, and **fraud risk**, helping identify areas to improve monitoring and reduce risk exposure.

#### Key Results:
- Tracked total transaction value (**2.43M**) and volume (**56K transactions**) with clear YoY and forecast trend lines.
- Identified customer segments and merchant categories driving the highest spend.
- Surfaced **22 flagged merchants** and **5K high-risk transactions** for closer review.
- Built a transaction heatmap to detect unusual timing patterns tied to refund/fraud activity.

#### Outcome:
The dashboard enables data-driven monitoring of transaction health and risk, supporting faster identification of anomalies and more informed decisions on card, merchant, and customer segment strategy.

## 📂 Dataset Description & Data Structure

### 📌 Data Source
- **Source**: Xóm - Dataset Bannking
- **Size**: The **banking transactions** table contains approximately **150000** transaction records across **2000** customers.
- **Format**: Pbix

### 📊 Data Structure & Relationships

#### 1️⃣ Tables Used:
The dataset consists of **6 main tables** used to build the transaction dashboard:

1. 💳 **banking cards** – Card master data.

<details>
<summary><strong>Table 1: banking cards</strong></summary>

| Column Name             | Data Type      | Nullable | Description                                    |
|--------------------------|----------------|----------|-------------------------------------------------|
| `id`                     | int            | NO       | Unique card identifier (Primary Key)             |
| `client_id`              | int            | YES      | Foreign key to card owner (users)                |
| `card_brand`             | nvarchar(50)   | YES      | Card brand (Visa, Mastercard, ...)               |
| `card_type`              | nvarchar(50)   | YES      | Credit / Debit / Debit (Prepaid)                 |
| `card_number`            | nvarchar(20)   | YES      | Card number                                      |
| `expires`                | date           | YES      | Card expiration date                             |
| `cvv`                    | nvarchar(10)   | YES      | Card verification value                          |
| `has_chip`               | nvarchar(10)   | YES      | Whether the card has a chip (Yes/No)             |
| `num_cards_issued`       | int            | YES      | Number of cards issued to this account           |
| `credit_limit`           | decimal        | YES      | Credit limit of the card                         |
| `acct_open_date`         | date           | YES      | Date the account was opened                      |
| `year_pin_last_changed`  | int            | YES      | Year the card PIN was last changed               |


</details>

2. 💸 **banking transactions** – Core transaction fact table.

<details>
<summary><strong>Table 2: banking transactions</strong></summary>

| Column Name       | Data Type      | Nullable | Description                                        |
|--------------------|----------------|----------|------------------------------------------------------|
| `id`               | int            | NO       | Unique transaction identifier (Primary Key)          |
| `date`             | datetime       | YES      | Transaction date and time                            |
| `client_id`        | int            | YES      | Foreign key to customer (users)                      |
| `card_id`          | int            | YES      | Foreign key to card (cards)                          |
| `amount`           | decimal        | YES      | Transaction amount                                    |
| `use_chip`         | nvarchar(50)   | YES      | Payment method used (Chip / Swipe / Online)          |
| `merchant_id`      | int            | YES      | Merchant identifier                                   |
| `merchant_city`    | nvarchar(100)  | YES      | City where the transaction occurred                   |
| `merchant_state`   | nvarchar(50)   | YES      | State where the transaction occurred                  |
| `zip`              | nvarchar(10)   | YES      | Zip code of the merchant                              |
| `mcc`              | int            | YES      | Merchant category code, foreign key to `mcc_codes`    |
| `errors`           | nvarchar       | YES      | Error/refund flag or reason recorded on the transaction |

</details>

3. 👤 **banking users** – Customer master data.

<details>
<summary><strong>Table 3: banking users</strong></summary>

| Column Name          | Data Type      | Nullable | Description                                     |
|------------------------|----------------|----------|---------------------------------------------------|
| `id`                  | int            | NO       | Unique customer identifier (Primary Key)          |
| `current_age`         | int            | YES      | Customer's current age                            |
| `retirement_age`      | int            | YES      | Customer's expected retirement age                |
| `birth_year`          | int            | YES      | Year of birth                                     |
| `birth_month`         | int            | YES      | Month of birth                                    |
| `gender`              | nvarchar(20)   | YES      | Customer gender                                   |
| `address`             | nvarchar(255)  | YES      | Customer address                                  |
| `latitude`            | nvarchar(20)   | YES      | Latitude of customer address                      |
| `longitude`           | nvarchar(20)   | YES      | Longitude of customer address                     |
| `per_capita_income`   | decimal        | YES      | Per capita income of the customer                 |
| `yearly_income`       | decimal        | YES      | Yearly income of the customer                     |
| `total_debt`          | decimal        | YES      | Total debt of the customer                        |
| `credit_score`        | int            | YES      | Customer credit score                             |
| `num_credit_cards`    | int            | YES      | Number of credit cards held by the customer       |
| `Credit Segment`      | calculated     | YES      | Segment derived from `credit_score`               |
| `Income Segment`      | calculated     | YES      | Segment derived from `yearly_income`               |

</details>

4. 🏷️ **banking mcc_codes** – Merchant category reference.

<details>
<summary><strong>Table 4: banking mcc_codes</strong></summary>

| Column Name   | Data Type      | Nullable | Description                            |
|---------------|----------------|----------|------------------------------------------|
| `mcc_id`      | int            | NO       | Merchant category code (Primary Key)     |
| `description` | nvarchar(255)  | YES      | Merchant category description            |


</details>

5. 📅 **Date** – Date dimension table.

<details>
<summary><strong>Table 5: Date</strong></summary>

| Column Name     | Description                          |
|-------------------|---------------------------------------|
| `Date`           | Calendar date                          |
| `Month Name`, `Month Number` | Month details               |
| `Year`           | Calendar year                          |
| `IsPast`         | Flag for past dates (vs. forecast)     |
| `Year Month`     | Year Month   |

</details>

6. 🧮 **_Caculation** – Measure table holding DAX calculations.



#### 2️⃣ Data Relationships:

| **From Table**         | **To Table**         | **Join Key**              | **Relationship Type**                                   |
|--------------------------|------------------------|-----------------------------|------------------------------------------------------------|
| `banking cards`         | `banking users`       | `client_id` ↔ `id`         | Many-to-One (many cards per customer)                       |
| `banking transactions`  | `banking cards`       | `card_id` ↔ `id`           | Many-to-One (many transactions per card)                    |
| `banking transactions`  | `banking users`       | `client_id` ↔ `id`         | Many-to-One (many transactions per customer)                |
| `banking transactions`  | `banking mcc_codes`   | `mcc` ↔ `mcc_id`           | Many-to-One (many transactions per merchant category)       |
| `banking transactions`  | `Date`                 | `date` ↔ `Date`            | Many-to-One (many transactions map to one calendar date)   |

## 📊 Key Insights & Visualizations

### 🔍 Dashboard Preview

### 📋 I. Overview

### 📌 Key Findings:

#### **1. Overall Performance**
- Total transaction amount reached **2.43M**, up **+0.18%** vs. last year, across **56K transactions** (down **-0.11%** YoY).
- Refund rate stood at **5.18%**, down **-0.47%** YoY, while average transaction value rose slightly to **43.74** (+0.29%).

-> **Volume is roughly flat while spend value edges up**, and refund rate is trending in a healthy direction.

#### **2. Actual, Moving Average and Forecast Trend**
- Monthly transaction amount is volatile, swinging between **~180K–230K**, with the sharpest dip in **February** and a strong spike in **July (~230K)**.
- The 12-month moving average stays flat around **200K–210K**, smoothing out the monthly noise, with a forecast line projecting continued stability.

-> **Underlying demand is stable**, but individual months can swing sharply and deserve investigation (e.g. the July spike, February dip).

#### **3. Transaction Trend (Volume)**
- Transaction counts range from **~4,270 to ~4,860** per month, with a low in **February** and a peak in **July**, mirroring the amount trend.

-> **Volume and value move together**, suggesting seasonality rather than pricing shifts drives the swings.

#### **4. YoY Comparison**
- Most months show **positive YoY growth (4–7%)**, but a few months (**Feb -8%, Apr -2%, Jun -3%, Oct -2%**) underperform last year.

-> **Growth is inconsistent across the year**, with early-year and mid-year dips worth root-causing.

#### **5. Comparison by Income Segment**
- **High Income** customers contribute the most spend (**1.07M** this year vs **1.05M** last year), followed by **Low Income (0.70M)** and **Middle Income (0.66M)**.

-> **High-income customers are the primary revenue driver** and a segment worth prioritizing for retention.

### 📈 II. Transaction Behaviors

### 📌 Key Findings:

#### **1. Transactions by Payment Method**
- **Chip Transactions dominate at 71.44% (40K)**, followed by **Swipe (17.18%, 10K)** and **Online (11.38%, 6K)**.

-> **Chip usage is the primary channel**, meaning fraud controls and UX should prioritize this method.

#### **2. Top Merchant Categories**
- **Money Transfer** leads merchant spend (**0.21M**), followed by **Grocery Stores/Supermarkets (0.18M)** and **Wholesale Clubs (0.17M)**.
- Categories like **Telecommunication Services** and **Tolls and Bridge Fees** trail at around **0.10M** each.

-> **Spend is concentrated in a handful of essential categories**, useful for targeted merchant partnerships or rewards.

#### **3. Customer Segments (by Credit Score)**
- The **"Good"** and **"Very Good"** segments make up the bulk of customers (**29.3%** and **44.6%** respectively), while **"Very Poor"** is a small minority (**2.25%**).
- Average income is highest for the **"Very Poor"** segment (**49,804**) — likely a small, distinct outlier group — while other segments average **~44K–46K**.

-> **The customer base skews toward good/very good credit**, a relatively low-risk portfolio overall.

#### **4. Monthly Customer Trend**
- Active customer count (AC) hovers close to the prior-year (PY) and forecast (FC) lines, spiking slightly around **July** before returning to baseline.

-> **Customer base size is stable**, with no signs of major churn or acquisition swings.

#### **5. Merchant State Breakdown**
- **CA (California)** is the top state by transaction amount (**275,214**) and transaction count (**6,920**), followed by **AK** and **FL**.
- International states like **China, Costa Rica, Denmark, Dominican Republic** appear with very low volumes, likely cross-border edge cases.

-> **Transaction activity is heavily domestic and concentrated in a few states**, with a long tail of low-volume international activity worth monitoring for anomalies.

### III. 🚨 Risk & Fraud

### 📌 Key Findings:

#### **1. Overall Risk Metrics**
- **High-risk transactions** total **5K**, down **-16.86%** YoY, and **refunded count** is **2K**, down **-16.03%** YoY — both improving.
- However, **refund rate** ticked up slightly to **5.23% (+1.00%)**, and **22 merchants** remain flagged (down from more last year, **-12%**).

-> **Risk volume is shrinking, but refund rate creeping up** suggests remaining risk is more concentrated per transaction.

#### **2. Refund Rate Trend**
- Refund rate is volatile month to month, spiking to **~5.8% in April** and **~5.6% in July**, with lower points around **~4.8–5.0%** in March, May, and August.

-> **Refund rate spikes don't follow a clean seasonal pattern**, warranting month-by-month root-cause review rather than assuming seasonality.

#### **3. Refund Rate by Card Type**
- **Debit cards** have the highest refund rate (**5.38%**), close behind **Credit (5.15%)**, while **Debit (Prepaid)** is notably lower (**3.99%**).

-> **Prepaid debit cards carry the lowest refund risk**, possibly due to lower per-transaction limits or different customer behavior.

#### **4. Transaction Heatmap**
- Transaction activity is heavily concentrated in the **morning hours (07:00–08:00)**, especially on **weekdays**, with counts reaching **20–33** per hour/day cell.
- Very early hours (00:00–05:00) show minimal activity across all days.

-> **Fraud monitoring resources should weight toward early-morning weekday hours**, where volume — and therefore absolute risk exposure — is highest.

#### **5. Refund Rate by Payment Method**
- **Chip transactions** have the highest refund rate (**6.06%**), followed by **Swipe (5.06%)**, while **Online transactions** are far lower (**0.25%**).

-> Despite chip being the dominant and generally trusted channel, **it also carries the highest refund rate**, suggesting closer scrutiny is needed there rather than assuming online is riskier.

#### **6. Top 5 States by Refund Rate**
- **Spain** shows a **100% refund rate**, though on a very small base (1 transaction), followed by **Portugal and France (50%)**, **Germany (40%)**, and **United Kingdom (36.36%)**.

-> These are **low-volume international outliers rather than systemic risk**, but they're worth flagging individually given the small sample sizes can mask real fraud patterns.

## 🔎 Final Conclusion & Recommendation

| **Aspect**                     | **Insight**                                                                                                      | **Recommendation**                                                                                       |
|---------------------------------|------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Transaction Volume & Value**  | Volume is roughly flat YoY (-0.11%) while total amount edges up (+0.18%); monthly swings (e.g., July spike, February dip) don't map cleanly to seasonality. | Investigate the drivers behind the **July spike and February dip** specifically, rather than assuming seasonal patterns. Use the moving-average and forecast lines to set realistic monthly targets. |
| **Customer & Revenue Concentration** | **High-income customers** drive the largest share of spend; credit segments skew toward **Good/Very Good**, indicating a relatively low-risk customer base. | **Prioritize retention programs for high-income customers** and consider targeted offers for **Fair/Poor** segments to grow volume without adding disproportionate risk. |
| **Payment Method & Channel Risk** | **Chip transactions dominate volume (71%)** but also carry the **highest refund rate (6.06%)**, higher than swipe or online. | Strengthen **fraud checks specifically on chip transactions** rather than assuming they are inherently safer; review authorization rules for this channel. |
| **Fraud & Refund Risk**         | High-risk transaction count and refunded count are both **declining YoY**, but overall refund rate ticked **up slightly**, and a handful of low-volume states show **very high refund rates**. | Set up **automated alerts for refund rate spikes** by month, card type, and merchant, and review the **22 flagged merchants** and low-volume international states individually to confirm whether they represent fraud or one-off anomalies. |
| **Timing Patterns**             | Transaction activity is concentrated in **early morning weekday hours**, based on the heatmap. | Align **fraud monitoring staffing and alert thresholds** with peak activity windows to catch issues in near real-time rather than after the fact. |
