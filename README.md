# Loyalty-Driven Dealer Performance Analysis
### GO Auto Services 

**Team ALPACA**  

---

## Project Overview

GO Auto is a multi-dealership automotive network operating seven dealerships across Edmonton. This project investigates how loyalty program participation impacts dealership performance and customer behavior, using machine learning techniques to uncover patterns that aren't visible through basic reporting.

We worked with a dataset of ~242,000 service transaction records spanning 2020–2024 across all seven locations. The analysis answers a core question: **which dealerships benefit most from loyalty card usage, and what distinct customer segments emerge from loyalty-driven behavior?**

---

## Problem Statement

**Problem #5 — Loyalty-Driven Dealer Performance (Clustering + Comparative Analysis)**

How does loyalty program participation impact dealership performance, and what customer segments drive the most value? The project covers:

- Comparing dealerships by loyalty adoption, retention, and revenue
- Segmenting customers into behavioral groups using clustering
- Benchmarking dealer performance against loyalty engagement
- Exploring whether loyalty members travel farther to specific dealerships

---

## Repository Structure

```
├── GoAutoServices.csv                              # Source dataset
├── cmpt3830_phase_1_go_auto.ipynb                  # Phase 1: EDA, cleaning, and Demo 1 presentation
├── cmpt3830_phase_1_go_auto_from_feedback.ipynb     # Phase 1 (revised): Post-demo adjustments from client/instructor feedback
├── CMPT_3830_DEMO_2_ALPACA.ipynb                   # Phase 2: ML clustering, dealer analysis, and Demo 2 presentation
└── README.md
```

---

## Phase 1 — Exploratory Data Analysis

**Notebook:** `cmpt3830_phase_1_go_auto.ipynb`

Covers the full EDA pipeline for understanding GO Auto's service data and loyalty program adoption:

- **Data quality assessment** — Duplicate detection, missing value handling (loyalty_card imputation, cost imputation using payment type and dealer medians)
- **Data type conversions** — service_date to datetime, sale_date parsing, cost range extraction
- **Feature engineering** — Vehicle age calculation, cost range parsing (e.g., '200-300' -> avg 250), min-max normalization for clustering readiness
- **Outlier detection and removal** — IQR-based removal per instructor requirements (outliers must be removed, not just flagged)
- **Exploratory visualizations** — Dealer volume, loyalty adoption rates, customer behavior comparisons (loyalty vs. non-loyalty), dealer performance matrix

### Post-Demo 1 Amendments

Following Demo 1, the client and instructor provided feedback that led to additional analysis within the same notebook:

1. **Intermittent loyalty card scanning** — The client asked us to investigate inconsistent scan patterns (e.g., scanned, scanned, not scanned, scanned). We built per-VIN scanning timelines and classified customers into scan categories: Always Scanned, Never Scanned, and Intermittent. Intermittent scanners were further assigned a loyalty treatment (Non-Loyal, Partially Engaged, or Loyal) based on their scan ratio and sequential patterns.
2. **Same-day visit aggregation** — The client asked whether multiple records on the same day should count as one visit or many. We created a visit-level aggregated dataset grouping by VIN + service_date.

### Revised Phase 1

**Notebook:** `cmpt3830_phase_1_go_auto_from_feedback.ipynb`

A cleaner, reorganized version of Phase 1 that integrates all client/instructor feedback directly into the main pipeline rather than as post-hoc amendments. Same core analysis with tighter structure.

---

## Phase 2 — Machine Learning & Clustering

**Notebook:** `CMPT_3830_DEMO_2_ALPACA.ipynb`

Builds on the cleaned Phase 1 output to perform customer segmentation and dealer performance analysis:

### Data Preparation
- Dropped non-modeling columns (dates, flags, raw strings)
- **Ordinal encoding** for loyalty_treatment (Non-Loyal < Partially Engaged < Loyal) and scan_category (Never Scanned < Intermittent Scanner < Always Scanned)
- **Frequency encoding** for make (65 unique) and model (892 unique)

### Customer Segmentation (Clustering)
- Aggregated to customer level with features: total_visits, avg_cost, avg_distance, loyalty_ratio, loyalty_treatment_ord, scan_category_ord
- **Optimal k selection** using elbow method + silhouette analysis → k=5 selected
- **KMeans clustering** achieving silhouette score ~0.527 (above 0.50 threshold)
- **Algorithm comparison**: KMeans vs. Gaussian Mixture Model vs. DBSCAN — KMeans selected as best performer
- **Make-specific sub-clustering**: Repeated analysis within top vehicle makes to find brand-level segments

### Five Customer Segments Identified
| Segment | Share | Description |
|---|---|---|
| Non-Loyal Regulars | ~56% | Never scanned loyalty card, baseline service behavior |
| Engaged Power Users | ~21.5% | Moderate loyalty engagement, consistent visits |
| Loyal Advocates | ~12.4% | High loyalty participation, strong retention |
| Distance Travelers | ~7.1% | Travel significantly farther (avg distance ≥ 145 km) |
| High-Spend Luxury | ~2.6% | Premium service spending (avg cost ≥ $2,150) |

### Dealer Performance Analysis
- Dealer-level aggregation and clustering
- Loyalty rate vs. revenue scatter analysis
- Interactive dealer location map (Folium)
- Per-dealer cluster distribution breakdowns

### Client Recommendations (with supporting visualizations)
1. Strengthen loyalty engagement across underperforming dealers
2. Target high-value customer segments for retention campaigns
3. Optimize dealer performance benchmarking using loyalty metrics
4. Geographic targeting based on distance willingness patterns

---

## Key Findings

- **82% of service transactions come from non-loyalty customers** — massive untapped potential for the loyalty program
- **Loyalty members visit ~2.4x more frequently** than non-loyalty customers
- **Toyota Trail (Go Toyota)** has the highest loyalty adoption (~37.6%), combining high volume with strong engagement
- **Luxury paradox**: Premium dealerships (Porsche, Jaguar) have the lowest loyalty adoption despite high per-customer value
- **scan_category** (loyalty scanning consistency) proved to be a critical clustering feature — adding it pushed silhouette scores above the 0.50 threshold

---

## Tech Stack

- **Python** — pandas, numpy, matplotlib, seaborn, scikit-learn
- **scikit-learn** — KMeans, GaussianMixture, DBSCAN, AgglomerativeClustering, silhouette analysis, MinMaxScaler
- **Folium** — Interactive dealer location mapping
- **Google Colab** — Primary development environment
- **Power BI** — Dashboard for final demo presentation

---

## Datasets Produced

| File | Level | Description |
|---|---|---|
| `go_auto_cleaned_amended.csv` | Transaction | Cleaned dataset with loyalty features merged |
| `go_auto_visits.csv` | Visit | Same-day aggregated (1 row per VIN per service date) |
| `go_auto_customer_summary.csv` | Customer | One row per VIN with aggregated behavioral features |

---

## How to Run

1. Clone this repository
2. Upload `GoAutoServices.csv` to your working environment (Google Colab or local)
3. Run `cmpt3830_phase_1_go_auto_from_feedback.ipynb` first — this produces the cleaned datasets needed for Phase 2
4. Run `CMPT_3830_DEMO_2_ALPACA.ipynb` — this loads the Phase 1 output and runs the full ML pipeline

**Requirements:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `folium`

---

## Course

CMPT 3830 — Machine Learning Work Integrated Learning  
NorQuest College, Winter 2026
