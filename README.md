# 📊 Amazon Product Analytics: Discount Impact Analysis using A/B Testing, CLV Modelling & Statistical Inference

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Domain](https://img.shields.io/badge/Domain-E--Commerce%20Analytics-purple)
![Methods](https://img.shields.io/badge/Methods-A%2FB%20Testing%20%7C%20CLV%20%7C%20Statistical%20Inference-red)

---

## 🧭 Project Overview

Every e-commerce business faces the same fundamental question - **do discounts actually work?**

Discounting is one of the most commonly used levers in e-commerce. Sellers apply heavy price 
reductions hoping to boost visibility, drive purchases, accumulate reviews, and ultimately 
improve their product rankings. But beneath this assumption lies a critical gap - **does 
discounting genuinely improve customer engagement and satisfaction, or does it simply erode 
margins without delivering measurable value?**

This project tackles that question using a real Amazon product dataset. It is an 
**end-to-end data science project** that moves from raw data cleaning all the way through to 
formal statistical testing, customer lifetime value modelling, and adaptive algorithm 
simulation producing actionable business recommendations backed by statistical evidence.

---

## 📌 The Business Problem

### Context
In competitive e-commerce marketplaces like Amazon, sellers must constantly decide:
- How aggressively should I discount my products?
- Will discounting improve my product's ratings and visibility?
- Which customers and product categories are actually worth investing in?
- How do I maximise long-term revenue rather than just short-term sales?

### The Core Hypothesis Being Tested
> *"Products with high discounts (≥50%) will generate significantly more customer 
> engagement than products with low discounts (<50%), because lower prices reduce 
> purchase barriers and encourage more buyers to leave reviews."*

### Business Questions This Project Answers

| Business Question | Method |
|---|---|
| Do high discounts drive more customer reviews? | A/B Testing - Welch's T-Test |
| Do discounts improve customer satisfaction? | T-Test + Cohen's d |
| Does product price affect customer ratings? | Welch's T-Test |
| Which customers and categories generate the most long-term value? | CLV Modelling + Segmentation |
| How much more revenue does targeted acquisition deliver? | Pareto/Lorenz Analysis |
| Is adaptive allocation better than a fixed 50/50 split? | Epsilon-Greedy Bandit Simulation |
| Are there associations between discount, price, and rating tier? | Chi-Square Tests |

---
## ✅ Business Recommendations

 - **Use discounts only for new product launches** - the **6.29% uplift** is real but
   uncertain (CI: -17.9% to +31.8%). Broad discounting risks margin erosion without
   guaranteed returns.
- **Build retention programs for the Very High CLV segment** - **22.8% of customers
   generate 80% of revenue**. Targeted loyalty programs here deliver disproportionate
   long-term impact.
- **Shift to targeted acquisition** - focusing on top 25% CLV customers delivers
   **237% more LTV** (₹66.4M vs ₹19.7M) from the same acquisition effort.
- **Invest in product quality, not pricing** - **neither discount level nor price
   meaningfully affects ratings**. Quality and accurate descriptions drive satisfaction.
- **Prioritise retention over margin** - moving retention from **50% → 70%
   triples average CLV** (₹1,088 → ₹3,234), a bigger lever than any margin improvement.
- **Concentrate on smartphones and Smart TVs** - these **two categories dominate
   long-term revenue** entirely. Allocate marketing and inventory disproportionately here.
- **Collect more data before policy decisions** - current samples are underpowered.
   **3,264 per group** needed to reliably detect a **15% business-meaningful uplift**.
   
---

## 📂 Dataset

- **Source:** [Amazon Sales Dataset — Kaggle](https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset/data)
- **Size:** 1,465 products across 211 unique categories
- **Price range:** ₹39 - ₹1,39,900
- **Average discount:** 47.7%
- **Rating range:** 1.0 -  5.0 (median 4.1)
- **Review count range:** ~1,000 - 400,000+

### Key Columns

| Column | Description |
|---|---|
| `product_id` | Unique product identifier |
| `product_name` | Full product name |
| `category` | Product category path |
| `actual_price` | Original listed price (₹) |
| `discounted_price` | Final selling price after discount (₹) |
| `discount_percentage` | Percentage discount applied |
| `rating` | Average customer rating (1-5) |
| `rating_count` | Total number of customer reviews |

---
## 🔬 Methodology

### Data Cleaning 
- Removed currency symbols, standardised formats, imputed missing values
- Detected outliers using Z-score (threshold = 3) - retained as genuine premium products
- Engineered 4 derived features: `price_discount_amount`, `price_discount_percentage`,
  `discount_ratio`, `log_rating_count`
- **98%+ data retention** after cleaning · Final dataset: **1,465 rows × 16 columns**
### Engineered Features
| Feature | Description | Purpose |
|---|---|---|
| `price_discount_amount` | actual_price - discounted_price | Absolute discount value |
| `price_discount_percentage` | discount × 100 | Percentage form for grouping |
| `discount_ratio` | discounted_price / actual_price | Normalised price reduction |
| `log_rating_count` | log(1 + rating_count) | Reduces skew for statistical testing |

---

### Exploratory Data Analysis
EDA was structured around 8 targeted business questions - every chart chosen to 
reveal something actionable.

- Prices are heavily right-skewed - most products are budget to mid-range with 
  a small number of premium outliers
- Ratings cluster tightly between 4.0-4.3 - very little variance, making rating 
  a poor A/B test metric
- `rating_count` ranges from ~1,000 to 400,000+ - log transformation required 
  to meet statistical testing assumptions
- Electronics dominate in volume and engagement. Wearables receive the highest 
  average discounts. Kitchen tools lead in review count (270,000+)
- Weighted category ranking applied - 70% rating quality + 30% review volume

> **Key EDA finding:** Rating shows near-zero correlation with all pricing and 
> discount variables - no pricing variable predicts engagement or satisfaction, 
> directly motivating formal A/B testing.

---

### Customer Lifetime Value (CLV) Modelling
CLV modelling shifts the analysis from *"which products sell?"* to 
*"which customers yield long-term profitability?"*

**Model assumptions:**

| Parameter | Value |
|---|---|
| Gross margin | 30% |
| Base purchase frequency | 1.0 per year |
| Base retention rate | 60% |
| Annual discount rate | 10% |
| Rating scaling | rating / 5.0 |
```
CLV = (unit_margin × purchase_freq × (1 + retention_rate)) 
      / (1 + discount_rate - retention_rate)
```

**Results:**
- CLV ranges from **₹17 to ₹64,174** (median ₹482, mean ₹1,972)
- Top 15 products exclusively **Smartphones and Smart TVs**
  - Smartphones win through volume (300,000+ customers)
  - Smart TVs win through higher per-customer CLV (premium pricing)
- **Lorenz curve:** Top 25% customers → **81% of total LTV**
- Very High CLV segment: 22.8% of customers → 80% of revenue

**Business impact translation:**

| Strategy | LTV (10,000 customers) |
|---|---|
| Random acquisition | ₹19,720,000 |
| Targeted top 25% CLV | ₹66,400,000 |
| **Uplift** | **+237%** |

**Sensitivity analysis:**

| Scenario | Margin | Retention | Avg CLV |
|---|---|---|---|
| Worst Case | 20% | 50% | ₹1,088 |
| Expected Case | 30% | 60% | ₹1,972 |
| Best Case | 40% | 70% | ₹3,234 |

> Retention rate is a bigger CLV lever than gross margin, 
> improving retention from 50% to 70% triples average CLV.

---

### A/B Test Design & Metric Selection
Three metrics are screened before committing to the full test:

| Metric | Result | Decision |
|---|---|---|
| `rating` | Cohen's d = -0.22, clustered 4.0-4.3 | ❌ Rejected - negligible variance |
| Conversion rate | 100% both groups - fully saturated | ❌ Rejected - no signal |
| `rating_count` | High variance, proxies purchase volume | ✅ Selected |

**Test design:**

| | Group A (Control) | Group B (Treatment) |
|---|---|---|
| Definition | Low Discount (<50%) | High Discount (≥50%) |
| Sample size | n = 770 | n = 695 |

- **H₀ -** Engagement is equal between groups
- **H₁ -** High discount group has higher engagement

---

### A/B Test Results

`rating_count` is log-transformed before testing, extreme right-skew 
(1,000 - 400,000+) violates t-test normality assumptions. Log transformation 
produces a near-normal distribution, making the test valid.

- **Raw uplift: +6.29%** - statistically significant (p = 0.00006) ✓
- **Log metric uplift: -5.05%** - contradicts the raw uplift

**Why the contradiction?** The raw 6.29% is driven by a small number of outlier 
products in Group B with extremely high review counts. Log transformation reveals 
the typical high-discount product actually has **fewer reviews** than the typical 
low-discount product. Group A has a higher median log rating count.

- **Bootstrap CI: -17.9% to +31.8%** - includes zero, true direction uncertain
- **Power = 0.98** - test is highly reliable, small effect is genuine
- **Sample underpowered** for 15% MDE - 3,264 per group required

---

### Supporting Statistical Tests

| Test | Variables | Result |
|---|---|---|
| Welch's T-Test | Discount → Rating | p = 0.00003, d = -0.22 - negligible |
| Welch's T-Test | Price → Rating | p = 0.43 - not significant |
| Z-Test | Conversion Rate | 100% both groups - saturated |
| Chi-Square (×5) | Discount/Price/Category → Rating | All significant ✓ |

> Category is a stronger driver of rating tier than discount level. 
> what you sell matters more than how much you discount it.

---

### Multi-Armed Bandit Simulation
Unlike fixed 50/50 A/B testing, an epsilon-greedy bandit adapts in real time, 
directing more traffic to the better-performing group while still exploring.

- **Algorithm:** Epsilon-Greedy (ε = 0.10) · 90% exploit · 10% explore
- **1,000 simulation rounds** resampling directly from actual `rating_count` 
  values, no normality assumption, more realistic than parametric sampling
- **Finding:** Bandit advantage is limited when both groups perform similarly
  consistent with Cohen's d = -0.21. Results vary across runs due to early 
  random draws from a skewed distribution. Set a random seed for reproducibility.

---

## 💡 Key Business Findings

> **The headline finding: High discounts drive 6.29% more engagement but the 
> effect is uncertain, driven by outliers, and should be used selectively.**

| Finding | Metric | Business Impact |
|---|---|---|
| Discounts drive engagement | 6.29% uplift, p = 0.00006 | Use for new product launches only |
| Discounts don't improve ratings | d = -0.22, negligible | Stop using discounts as a satisfaction tool |
| Price doesn't affect ratings | p = 0.43 | Invest in quality, not pricing strategy |
| CLV is highly concentrated | Top 25% = 81% of LTV | Build retention programs for high-value segment |
| Targeted beats random acquisition | 237% more LTV | Shift budget to targeted campaigns |
| Retention beats margin | ₹1,088 → ₹3,234 CLV | Post-purchase experience is critical |
| Electronics dominate LTV | Smartphones + Smart TVs | Concentrate marketing investment here |

---
