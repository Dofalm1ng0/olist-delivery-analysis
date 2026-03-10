# Olist E-Commerce: Delivery Performance Analysis

**Tools:** Python · Pandas · NumPy · Tableau  
**Dataset:** [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) — 95,000+ orders · 2016–2018  
**Dashboard:** [Live Tableau Dashboard](#) *(replace with your Tableau Public URL)*

---

## Business Problem

Olist is a Brazilian e-commerce marketplace connecting sellers to customers across the country. The goal of this project was to answer a single operational question:

> **How is delivery performance affecting customer satisfaction, and where should Olist intervene first?**

---

## Dataset

9 CSV files covering orders, customers, sellers, products, reviews, payments, and category translations. After filtering to delivered orders with complete delivery data, the final analysed dataset contains **95,809 orders**.

---

## Methodology

1. **Data loading and inspection** — Loaded all 9 CSVs, identified null patterns and incorrect data types
2. **Cleaning** — Converted 5 date columns from object to datetime; filtered to delivered orders only; dropped 24 rows with missing delivery dates
3. **Metric engineering** — Built 5 core metrics: delivery_delay_days, is_late, actual_delivery_days, seller_processing_days, logistics_days
4. **Merging** — Left-joined customers, items, products (with category translation), and reviews onto the delivered orders base; deduplicated to one row per order after items fan-out
5. **EDA** — Headline metrics, state analysis, category analysis, seller vs logistics split, delay bucket vs review score, 1-star review rate by bucket, correlation analysis, monthly trend
6. **Export** — 5 clean CSVs exported for Tableau visualisation
7. **Dashboard** — 4-view Tableau dashboard published on Tableau Public

---

## Key Findings

### 1. Late delivery severely damages customer satisfaction
| Delivery Status | Avg Review Score | % Receiving 1-Star |
|---|---|---|
| Early | 4.29 | 6.6% |
| On Time | 4.03 | 8.6% |
| 1–3 days late | 3.29 | 25.1% |
| 4–7 days late | 2.10 | 58.5% |
| 7+ days late | 1.70 | **69.8%** |

The sharpest drop occurs crossing from on-time to 1–3 days late (−0.74 points). The late-only correlation of −0.147 (vs full-dataset −0.267) confirms that damage concentrates at the moment of breach, not progressively thereafter.

**Implication:** Preventing lateness entirely is more valuable than reducing delay severity once an order is already late.

### 2. Three northeast states have late rates up to 3x the national average
| State | Late Rate | Avg Delay When Late | Avg Review |
|---|---|---|---|
| AL (Alagoas) | 20.8% | 9.7 days | 3.86 |
| MA (Maranhão) | 17.2% | 10.5 days | 3.83 |
| SE (Sergipe) | 15.0% | 16.4 days | 3.91 |
| PI (Piauí) | 13.8% | 13.5 days | 3.99 |
| CE (Ceará) | 13.7% | 15.0 days | 3.94 |

National average: **6.7%**. All five worst-performing states are in Brazil's northeast.

### 3. Logistics accounts for 73% of total delivery time
| Segment | Avg Time | % of Total |
|---|---|---|
| Seller processing (order → carrier pickup) | 2.7 days | 22% |
| Logistics / carrier (pickup → customer) | 8.8 days | **73%** |
| Total delivery | 12.1 days | — |

**office_furniture** is the only major category-level outlier on the seller side: 9.9 days avg processing vs platform norm of 2.7 days.

### 4. Feb–Mar 2018 operational anomaly
Late rate spiked from 5.6% in January to **18.7% in March 2018** before recovering to 4.4% in April — strong evidence of an event-driven disruption, not structural deterioration.

---

## Recommendations

| Priority | Action | Target |
|---|---|---|
| 1 — Immediate | Carrier performance audit for AL, MA, SE routes | Operations / Logistics |
| 2 — 30 days | Seller SLA enforcement for office_furniture (9.9 day processing) | Seller Management |
| 3 — 60 days | At-risk order early warning system at carrier handoff | Product / Engineering |
| 4 — 90 days | Root cause investigation of Q1 2018 anomaly | Operations |

---

## Dashboard

Four interactive views published on Tableau Public:

- **State Map** — Brazil filled map coloured by late delivery rate
- **Review by Delay Bucket** — Bar chart showing satisfaction collapse across 5 delivery timing buckets
- **Top Categories by Late Rate** — Horizontal bar with avg delay severity overlay
- **Monthly Trend** — Dual-axis line + bar showing late rate vs order volume 2016–2018

[**View Live Dashboard →**](#) *(replace # with your Tableau Public URL)*

![Dashboard Preview](dashboard_preview.png)

---

## How to Run

```bash
# Clone the repository
git clone https://github.com/Dofalm1ng0/olist-delivery-analysis

# Install dependencies
pip install pandas numpy jupyter

# Download dataset from Kaggle
# https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
# Place all CSV files in /Dataset/

# Run the notebook
jupyter notebook Olist_Delivery_Analysis.ipynb
```

Update the file paths in Cell 1 to match your local dataset location.

---

## Files

| File | Description |
|---|---|
| `Olist_Delivery_Analysis.ipynb` | Full analysis notebook — EDA, metrics, findings |
| `olist_master.csv` | Order-level export (95,809 rows) for Tableau |
| `state_performance.csv` | State-level aggregation |
| `category_delay.csv` | Category-level aggregation |
| `monthly_trend.csv` | Monthly late rate and order volume |
| `processing_by_cat.csv` | Seller vs logistics split by category |

---

*Analysis by Rajat Kumar · MBA, IIM Udaipur · [LinkedIn](https://www.linkedin.com/in/rajatkumar97611/)*
