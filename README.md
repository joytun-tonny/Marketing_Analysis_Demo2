# Digital Conversion Funnel & EDA with Python

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/joytun-tonny/Marketing_Analysis_Demo2/blob/main/Lab2_Conversion_Funnel_EDA.ipynb)
![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-data%20wrangling-150458?logo=pandas)
![seaborn](https://img.shields.io/badge/seaborn-visualization-4C72B0)

**Course:** DS 4115 – Marketing Analytics (United International University)
**Lab 2:** Exploratory Data Analysis (EDA) & Digital Conversion Funnel Analysis using Python

This project generates a synthetic GA4-style dataset of 5,000 web/app sessions. It builds a three-step conversion funnel (`session_start → add_to_cart → purchase`), cleans the data with pandas, and uses a seaborn correlation heatmap to find what drives or blocks conversion.

---

## 📂 Repository Contents

| File | Description |
|------|-------------|
| `Lab2_Conversion_Funnel_EDA.ipynb` | Complete notebook covering data generation, funnel analysis, data cleaning and the correlation heatmap |
| `README.md` | Project overview (this file) |

> The notebook creates `ga4_sample_session_logs.csv` when it runs. No external dataset needs to be downloaded.

---

## 🎯 Learning Objectives

- Generate and load digital telemetry session logs with Python
- Construct a multi-step conversion funnel and measure drop-off rates
- Clean data and impute missing values using pandas
- Perform EDA to find where users drop off
- Generate and interpret a correlation heatmap to identify conversion drivers

---

## 🗂️ Dataset

5,000 synthetic sessions generated with `np.random.seed(42)` so the results can be reproduced.

| Column | Type | Description |
|--------|------|-------------|
| `session_id` | object | Unique session ID (`sess_0001` … `sess_5000`) |
| `user_id` | object | Random user ID (`user_1000` – `user_1999`) |
| `device_category` | object | desktop (50%), mobile (40%), tablet (10%) |
| `session_start_flag` | int | Always 1 (every session starts) |
| `add_to_cart_flag` | int | 1 if an item was added to cart (~30%) |
| `purchase_flag` | int | 1 if purchased (~60% of cart sessions) |
| `page_views` | int | Poisson(λ = 3) + 1 |
| `time_on_site_sec` | float | page_views × random 20–60 sec, with **50 values deliberately left missing** |

---

## 🔬 Workflow

1. **Environment setup:** import `pandas`, `numpy`, `matplotlib` and `seaborn`
2. **Part 1: Data generation:** create the session logs, save them to CSV, and check the structure with `df.info()`
3. **Part 2: Conversion funnel:** count unique sessions at each stage and calculate conversion rates
4. **Part 3: Data cleaning:** detect missing values, fill `time_on_site_sec` with the median, and standardise the `device_category` text
5. **Part 4: EDA:** calculate the correlation matrix and plot an annotated heatmap

---

## 📊 Results

### Conversion Funnel

| Funnel Step | Sessions | Conversion vs. Start | Step-to-Step |
|-------------|---------:|---------------------:|-------------:|
| 1. Session Start | 5,000 | 100.0% | — |
| 2. Add to Cart | 1,470 | 29.4% | 29.4% (70.6% drop-off) |
| 3. Purchase | 905 | 18.1% | 61.6% (38.4% drop-off) |

### Data Cleaning

- 50 missing values (1%) were found in `time_on_site_sec` and filled with the median
- **0 missing values** remain, and all 5,000 sessions were kept

### Correlation Heatmap Insights

| Variable Pair | r | Interpretation |
|---------------|---:|----------------|
| `add_to_cart_flag` ↔ `purchase_flag` | **0.73** | Strong: adding to cart is the main predictor of a purchase |
| `page_views` ↔ `time_on_site_sec` | **0.80** | Strong: more pages viewed means more time on site |
| `page_views` / `time_on_site_sec` ↔ `purchase_flag` | ≈ 0.00 | None: browsing more does not by itself lead to buying |
| `session_start_flag` | NaN | Constant column (zero variance) |

---

## 💡 Key Findings

- **Main conversion blocker:** the *Session → Add to Cart* step, where 70.6% of sessions leave. Product pages, calls-to-action and recommendations need the most work.
- **Cart is the strongest buying signal:** cart-abandonment emails and retargeting could recover the 38.4% of cart sessions that don't convert.
- **Engagement is not the same as intent:** page views and time on site should not be used as success KPIs on their own.

> ⚠️ The dataset is synthetic and its flags were randomly generated, so the near-zero engagement correlations come from how the data was simulated. With real GA4 data, these relationships should be re-checked.

---

## ▶️ How to Run

**Google Colab (recommended):** click the **Open in Colab** badge at the top and choose *Runtime → Run all*.

**Locally:**

```bash
git clone https://github.com/joytun-tonny/Marketing_Analysis_Demo2.git
cd Marketing_Analysis_Demo2
pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook Lab2_Conversion_Funnel_EDA.ipynb
```

---

## 🛠️ Tech Stack

`Python` · `pandas` · `NumPy` · `matplotlib` · `seaborn` · `Google Colab`
