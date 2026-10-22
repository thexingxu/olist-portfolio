# Olist E-Commerce Analytics: Unit Economics, Logistics SLA & Customer Behavioral Segmentation

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3%2B-orange.svg)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An end-to-end commercial and operational data science portfolio investigating **100k+ real-world Brazilian marketplace transactions** (Olist, 2017–2018). 

Rather than treating the dataset as generic exploratory practice, this project evaluates the marketplace through the lens of executive decision-making across three interconnected pillars: **Unit Economics & Retention**, **Logistics Lead-Time & Review Risk**, and **Behavioral Segmentation Under Extreme Purchase Skew**.

---

## Executive Summary & Core Insights
THE MARKETPLACE FLYWHEEL CONSTRAINTS
               
 +-----------------------------------------------------------+
 |                  Unit Economics Gate                      |
 |   • 97% single-transaction customers                      |
 |   • 12-month LTV: R$ 24.72 gross margin contribution      |
 |   • Maximum Allowable CAC: R$ 8.16 (at 3:1 LTV:CAC target)|
 +-----------------------------+-----------------------------+
                               |
                               v
 +-----------------------------------------------------------+
 |                 Logistics Defect Risk                     |
 |   • 65% interstate fulfillment dependency                 |
 |   • Late deliveries drop average review score to 1.68/5   |
 |   • 68% of delayed orders result in a 1-star review       |
 +-----------------------------+-----------------------------+
                               |
                               v
 +-----------------------------------------------------------+
 |              Actionable Customer Personas                 |
 |   • Standard RFM fails due to constant Frequency (F=1)    |
 |   • High-ticket spend is driven by installment depth (6-10x)
 |   • Free-freight thresholds required to convert low-basket|
 +-----------------------------------------------------------+

1. **Retention & Unit Economics:** Olist operates as an acquisition-dependent discovery engine rather than a habitual repeat-purchase platform (~97% one-time buyers). With a 12-month cumulative gross margin LTV of **R$ 24.72**, customer acquisition cost (CAC) must be held strictly below **R$ 8.16** to sustain an institutional 3:1 LTV:CAC ratio.
2. **Logistics & Defect Elasticity:** Seller dispatch is lean (~1.8 days median), but long-haul carrier transit (~7.2 days median) across Brazil's regional corridors drives >70% of fulfillment lead time. When delivery slips past the checkout SLA, customer satisfaction falls off an operational cliff: ratings drop from **4.29/5 to 1.68/5**, with **68% generating 1-star reviews**.
3. **Behavioral Segmentation:** Forcing standard RFM quartile frequency splits fails when frequency is uniform ($F = 1$). A modified hybrid framework combining Recency, Monetary spend, Installment depth, and Freight burden isolates four distinct behavioral personas—identifying that platform GMV is disproportionately anchored by credit-financed installment buyers.

---

## Repository Structure
├── notebooks/
│   ├── 01_retention_and_ltv.ipynb                 # Cohort retention, 12M LTV decay, CAC headroom
│   ├── 02_logistics_and_defect_analysis.ipynb     # Lead-time decomposition, defect inflection curves
│   └── 03_rfm_and_customer_segmentation.ipynb    # Hybrid RFM, K-Means clustering, persona profiling
├── requirements.txt                              # Pinned Python package dependencies
├── README.md                                     # Executive summary and portfolio documentation
└── .gitignore                                    # Production git exclusion rules

---

## Detailed Case Studies

### 1. Customer Lifetime Value (LTV) & Acquisition Headroom
* **Notebook:** `notebooks/01_retention_and_ltv.ipynb`
* **Methodology:** Time-based cohort tracking (monthly cohorts across 2017–2018; 2016 excluded as pre-scale beta period). Evaluated repeat transaction rates and cumulative gross margin expansion over 3, 6, and 12-month horizons.
* **Findings:** 
  * Repeat transaction velocity plateaus within 60–90 days post-initial order.
  * Over 97% of unique buyers never return to the platform.
  * Platform survival depends entirely on first-order contribution margin profitability rather than downstream retention recovery.

### 2. Logistics SLA Decomposition & Review Score Risk
* **Notebook:** `notebooks/02_logistics_and_defect_analysis.ipynb`
* **Methodology:** Lifecycle timestamp parsing (`approved_at` $\rightarrow$ `carrier_date` $\rightarrow$ `customer_delivered_date`). Modeled the non-linear relationship between delivery delta ($\text{Actual} - \text{Estimated}$) and review sentiment. Route classification (intrastate vs. interstate).
* **Findings:**
  * **Carrier Bottleneck:** Carrier transit represents ~72% of total lead time; seller fulfillment handling contributes less than 18%.
  * **The SLA Cliff:** On-time orders average 4.29 stars (5% 1-star share). Defect orders (>0 days late) drop to 1.68 stars (68% 1-star share).
  * **Regional Infrastructure Drag:** Interstate corridors (65% of volume) double median transit days (~9.5 vs. ~4.8 days) and yield defect rates exceeding 15–20% in destination states across the North and Northeast (e.g., AL, MA, BA).

### 3. Customer Behavioral Segmentation & K-Means Clustering
* **Notebook:** `notebooks/03_rfm_and_customer_segmentation.ipynb`
* **Methodology:** Consolidated data at the `customer_unique_id` grain. Engineered Recency, Monetary spend, Basket size, Freight ratio, and Installment depth. Applied log-transforms, `StandardScaler`, and K-Means clustering with Elbow and Silhouette ($N=10,000$ stratified sample) diagnostic checks.
* **Findings:**
  * Identified 4 primary operational personas:
    * **High-Ticket Installment Buyers:** Minority volume, dominant GMV contributors; heavily reliant on 6–10 payment installments.
    * **Freight-Burdened Low-Basket:** Friction-sensitive users whose shipping costs exceed 30–45% of total basket value.
    * **Recent Active Buyers:** Low recency one-time shoppers prime for automated post-purchase cross-selling.
    * **Dormant / Lapsed Single-Order:** High-recency shoppers that represent unviable re-targeting ROI under tight CAC constraints.

---

## Setup & Execution

### Prerequisites
* Python 3.10+
* Virtual environment tool (`venv` or `conda`)

### Local Setup
```bash
# 1. Clone the repository
git clone [https://github.com/](https://github.com/)<your-username>/<your-repo-name>.git
cd <your-repo-name>

# 2. Create and activate virtual environment
python -m venv .venv
# On Windows PowerShell:
.venv\Scripts\Activate.ps1
# On macOS/Linux:
source .venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt


