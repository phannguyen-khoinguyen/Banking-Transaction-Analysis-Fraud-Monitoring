# Banking Transaction Analysis & Fraud Monitoring at Xóm Bank | Power BI

**Author:** Phan Nguyễn Khôi Nguyên

**Date:** August 2026

**Tools Used:** Power BI

## Table of Contents
[📌 Background & Overview](#-background--overview) <br>
[📂 Dataset Description & Data Structure](#-dataset-description--data-structure) <br>
[📊 Key Insights & Visualizations](#-key-insights--visualizations) <br>
[🔎 Final Conclusion & Recommendation](#-final-conclusion--recommendation)

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
- Tracked total transaction value and volume across **3 years (2022–2024)**, revealing stable performance in 2022–2023 followed by a **notable decline in 2024 (-16.66% in amount, -16.86% in transactions)**.
- Identified customer segments and merchant categories driving the highest spend, consistent across all 3 years.
- Found **flagged merchants declining every year (27 → 25 → 22)**, a positive fraud-control trend, while refund rate stays stable around **~5.2%** regardless of year.
- Built a transaction heatmap and payment-method risk breakdown that hold consistent across all 3 years, strengthening confidence in the patterns found (e.g., chip transactions consistently carry the highest refund rate).

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

![Image](https://github.com/user-attachments/assets/724ed908-32a0-459c-b1b0-3ba584f34a7f)

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

![Image](https://github.com/user-attachments/assets/d879767d-4e52-42f2-a5dc-16fe6c50f202)


### 📌 Key Findings (comparing 2022 → 2023 → 2024):

#### **1. Overall Performance**
- **2022** (baseline year): Total amount **2.42M**, **56K transactions**, refund rate **5.21%**, avg transaction **43.61**.
- **2023**: Total amount edged up to **2.43M (+0.18% YoY)**, transactions held steady at **56K (-0.11%)**, refund rate improved to **5.18% (-0.47%)**, avg transaction rose to **43.74 (+0.29%)**.
- **2024**: Total amount dropped to **2.02M (-16.66% YoY)** and transactions fell to **46K (-16.86%)**, while refund rate ticked up to **5.23% (+1.00%)** and avg transaction rose slightly to **43.84 (+0.24%)**.
- *Caveat: the 2024 YoY Comparison chart shows **November and December only as forecast (FC)**, not actual (AC), so part of the 2024 decline likely reflects an incomplete year (~10 months of actual data) rather than a full year-over-year drop.*

-> **2022–2023 was stable-to-slightly-growing**, but **2024 shows a real decline in volume even accounting for the missing Nov–Dec data**, since Jan–Oct 2024 transactions (~46K) already trail the same period in prior years.

#### **2. Actual, Moving Average and Forecast Trend**
- **2022**: Amount climbed from **~193K (Jan)** to a peak of **~213K (May)**, then gradually eased back to **~196K–202K** by year-end; the moving average stayed flat around **200K**.
- **2023**: More volatile — a sharp dip to **~180K in February**, a strong spike to **~230K in July**, moving average flat around **200K–210K**.
- **2024**: Similar volatility (dip to **~189K in February**, high of **~213K in January**), with actual data ending in **October (~209K)** and **Nov–Dec shown only as a flat ~202K forecast**.

-> **Underlying monthly demand is consistently volatile across all 3 years**, with February typically the weakest month — a recurring seasonal dip worth investigating (billing cycle, post-holiday spending pullback, etc.).

#### **3. Transaction Trend (Volume)**
- Monthly transaction counts follow the same shape every year: a **February trough** (~4,234–4,272) and generally higher counts from **April–August**.
- **2024** counts (~4,236–4,733) run consistently below the same months in **2022** (~4,577–4,797) and **2023** (~4,272–4,855), confirming the volume decline isn't just a Nov–Dec data gap.

-> **The February dip is a structural, recurring pattern**, not a one-off — and 2024's per-month volume is genuinely softer than prior years even before accounting for missing year-end data.

#### **4. YoY Comparison**
- **2023** had more up months than down (7 positive, 5 negative), with the sharpest drop in **February (-8%)** and biggest gain in **July (+8%)**.
- **2024** is more mixed: strong months in **January (+3%), June (+8%)**, but a sharp **-10% in July** and **-4% in March** — the opposite pattern from 2023's July spike.
- 2022 serves as the baseline year with no prior-year comparison available.

-> **Growth is inconsistent and doesn't repeat the same monthly pattern year to year**, so seasonality alone doesn't explain the swings — each dip/spike likely needs its own root-cause review.

#### **5. Comparison by Income Segment**
- **2022** (baseline): High Income **1.05M**, Low Income **0.71M**, Middle Income **0.67M**.
- **2023**: High Income grew to **1.07M** (vs **1.05M PY**); Low Income and Middle Income both dipped slightly (**0.70M** and **0.66M**).
- **2024**: **All three segments declined** — High Income **0.87M** (vs **1.07M PY**), Low Income **0.59M** (vs **0.70M PY**), Middle Income **0.56M** (vs **0.66M PY**).

-> **High-income customers remain the largest revenue segment every year**, but the **2024 drop hit all income segments proportionally**, suggesting the decline is broad-based (fewer/smaller transactions overall) rather than concentrated in one customer group.

### 📈 II. Transaction Behaviors

![Iamge](https://github.com/user-attachments/assets/9b185e7c-5379-4672-89a1-110d818fc8ff)


### 📌 Key Findings (comparing 2022 → 2023 → 2024):

#### **1. Transactions by Payment Method**
- **Chip transactions dominate every year**, but its share slips slightly: **71.44% (2022) → 71.34% (2023) → 71.12% (2024)**.
- **Swipe** share is edging up (**17.18% → 17.29% → 17.73%**) while **Online** stays roughly flat (**~11.1–11.4%**) across all 3 years.

-> **Chip remains the primary channel**, but its slow erosion in favor of Swipe is a trend worth watching for fraud-control and UX prioritization over time.

#### **2. Top Merchant Categories**
- The ranking is **consistent across all 3 years**: **Money Transfer** leads (**0.18M–0.21M**), followed by **Grocery Stores/Supermarkets** and **Wholesale Clubs**, with **Telecommunication Services** and **Tolls/Bridge Fees** trailing around **0.09–0.10M**.
- Absolute spend in every category shrinks in **2024**, in line with the overall volume decline, but the **relative ranking of categories is unchanged**.

-> **Spend concentration in essential categories is a stable, structural pattern**, not a one-year anomaly — useful for long-term merchant partnership or rewards strategy.

#### **3. Customer Segments (by Credit Score)**
- Segment composition is **identical every year**: **"Very Good" (44.6%)** and **"Good" (29.3%)** dominate the customer base, while **"Very Poor" (2.25%)** is a small minority.
- The **"Very Poor" segment consistently has the highest average amount per transaction** of any segment — but this figure itself is trending down sharply: **86.95 (2022) → 67.39 (2023) → 65.35 (2024)**.

-> **The customer base skews toward good/very good credit every year** (a low-risk portfolio), but the shrinking per-transaction spend of the small "Very Poor" segment is a leading indicator worth watching alongside the broader 2024 volume decline.

#### **4. Customer Count Trend**
- Total customers are **slowly declining**: **303 (2022) → 302 (2023, -0.33%) → 301 (2024, -0.33%)**.

-> **Customer base is essentially stable with only marginal attrition**, meaning the 2024 revenue drop is driven by **lower spend per customer**, not customer loss.

#### **5. Merchant State Breakdown**
- **CA (California)** is the top state every year, but its transaction volume falls in line with the overall trend: **275,214 / 6,920 orders (2022) → 279,062 / 6,742 (2023) → 224,356 / 5,579 (2024)**.
- Total transaction amount by state mirrors the headline numbers: **2.42M (2022) → 2.43M (2023) → 2.02M (2024)**, with the same long tail of low-volume international states (China, Costa Rica, Dominican Republic, etc.) appearing each year.

-> **The 2024 decline is broad-based across states**, not localized to one region — reinforcing that this is an overall volume/spend issue rather than a geography-specific one.

### III. 🚨 Risk & Fraud

![Image](https://github.com/user-attachments/assets/ab8f8d4e-8da3-42c6-b218-d567093124ea)


### 📌 Key Findings (comparing 2022 → 2023 → 2024):

#### **1. Overall Risk Metrics**
- **High-risk transactions**: **6K (2022) → 6K (2023, +0.66%) → 5K (2024, -16.86%)**.
- **Refunded count**: **3K (2022) → 3K (2023, -0.59%) → 2K (2024, -16.03%)**.
- **Refund rate** is remarkably stable: **5.21% → 5.18% → 5.23%**, never straying more than ~0.05 points across 3 years.
- **Flagged merchants steadily decline every year**: **27 (2022) → 25 (2023, -7.41%) → 22 (2024, -12.00%)**.

-> **Refund rate is a very stable baseline metric (~5.2%) regardless of year**, while the **flagged-merchant count is genuinely improving year over year** — a positive sign of tighter vendor risk controls. The drop in raw high-risk/refunded counts in 2024 should be read alongside the partial-year data caveat noted in the Overview.

#### **2. Refund Rate Trend**
- **2022**: ranged from a low of **4.44% (Sep)** to a high of **5.61% (May)**, with no single consistent seasonal pattern.
- **2023**: low of **4.42% (Feb)**, high of **5.61% (Apr)**.
- **2024**: low of **4.84% (Mar)**, high of **5.78% (Apr)**, with data available through **October** only.
- **April stands out as a recurring high-refund month across all 3 years** (5.51%, 5.61%, 5.78%).

-> **April is a consistent refund-rate hotspot every year** — worth investigating for a recurring cause (e.g., billing cycle, seasonal merchandise returns, post-holiday disputes) rather than treating it as random noise.

#### **3. Refund Rate by Card Type**
- **Debit (Prepaid) is consistently the lowest-risk card type every year**: **3.73% (2022) → 3.06% (2023) → 3.99% (2024)**.
- **Credit and Debit refund rates stay close together and elevated (~5.1–5.4%)** in all 3 years, with the higher of the two alternating between Credit (2022, 2023) and Debit (2024).

-> **Prepaid debit is a structurally lower-risk card type**, a pattern that holds across all 3 years — a reliable signal for risk-based pricing or fraud-model weighting.

#### **4. Transaction Heatmap**
- Across the dashboards, transaction activity consistently concentrates in **weekday morning hours (07:00–08:00)**, with counts reaching into the **20s–40s** per cell — this pattern repeats in every year filter.

-> **Peak-hour concentration is a stable, predictable pattern**, so fraud-monitoring staffing and alert thresholds can be confidently aligned to early-morning weekday windows.

#### **5. Refund Rate by Payment Method**
- **Chip consistently has the highest refund rate of the three payment methods every year**: **5.88–6.06%**, vs. Swipe at **5.06–5.71%**, vs. Online at a much lower **0.21–0.25%**.

-> This is a **persistent, 3-year pattern, not a one-off** — chip transactions warrant closer fraud scrutiny despite being the trusted, dominant channel, while online's very low refund rate suggests its controls are working well.

#### **6. Top 5 States by Refund Rate**
- The specific states change completely every year (**HI/AK/KS/Lebanon/Norway in 2022**, **Switzerland/Aruba/Jamaica/Peru/Germany/Nigeria in 2023**, **UK/Germany/Portugal/France/Spain in 2024**), but they share one trait: **very small transaction counts (1–35 transactions)** driving extreme refund-rate percentages (up to 100%).

-> These are **statistical outliers from thin sample sizes rather than a systemic geography risk** — worth a manual review case by case, but not a pattern to build a blanket policy around.

## 🔎 Final Conclusion & Recommendation

| **Aspect**                     | **Insight**                                                                                                      | **Recommendation**                                                                                       |
|---------------------------------|------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **2024 Volume Decline**         | Total amount and transactions were stable-to-growing in 2022–2023, then dropped sharply in **2024 (-16.66% / -16.86%)**, hitting all income segments and states proportionally. Note: 2024's Nov–Dec are forecast, not actual, so the true full-year gap may be smaller — but Jan–Oct 2024 data alone already trails prior years. | Confirm whether the 2024 dip is a genuine business decline or a data-completeness issue by refreshing with full-year actuals; if genuine, investigate broad-based causes (macro, product, competition) rather than a single segment or region. |
| **Recurring Seasonal Patterns** | **February is a consistent low point** in transaction amount/volume across all 3 years, and **April is a consistent refund-rate hotspot**. | Plan **staffing, marketing, and fraud-review resources** around these recurring calendar patterns instead of treating each year's dip/spike as a one-off. |
| **Customer & Revenue Concentration** | **High-income customers** drive the largest share of spend every year; credit segments skew toward **Good/Very Good**, indicating a relatively low-risk, stable customer base (customer count declining only ~0.33%/year). | **Prioritize retention programs for high-income customers**, since 2024's spend drop hit them hardest in absolute terms; consider targeted offers for **Fair/Low-income** segments to diversify revenue. |
| **Payment Method & Channel Risk** | **Chip transactions dominate volume (~71%)** and consistently carry the **highest refund rate (5.9–6.1%)** of any payment method, every year; **Debit (Prepaid)** cards are consistently the lowest-risk card type. | Strengthen **fraud checks specifically on chip transactions** rather than assuming they are inherently safer; consider favorable risk weighting for prepaid debit given its consistently lower refund rate. |
| **Fraud & Vendor Risk**         | **Flagged merchants have declined every year (27 → 25 → 22)** — a genuine improving trend — while refund rate holds steady at ~5.2% and top-refund-rate states are low-volume statistical outliers that change every year. | Continue current vendor risk controls given the improving flagged-merchant trend; set up **automated alerts for refund rate spikes by month/card type**, and review small-sample high-refund states individually rather than building policy around them. |
| **Timing Patterns**             | Transaction activity is concentrated in **early morning weekday hours** consistently across all 3 years. | Align **fraud monitoring staffing and alert thresholds** with these confirmed peak activity windows. |
