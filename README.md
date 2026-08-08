
<svg xmlns="http://www.w3.org/2000/svg" width="1200" height="180" viewBox="0 0 1200 180">
  <defs>
    <linearGradient id="titleGrad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#38BDF8"/>
      <stop offset="50%" stop-color="#6366F1"/>
      <stop offset="100%" stop-color="#A855F7"/>
    </linearGradient>

<linearGradient id="lineGrad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#38BDF8" stop-opacity="0"/>
      <stop offset="50%" stop-color="#38BDF8"/>
      <stop offset="100%" stop-color="#A855F7" stop-opacity="0"/>
    </linearGradient>

  <filter id="glow">
      <feGaussianBlur stdDeviation="5" result="blur"/>
      <feMerge>
        <feMergeNode in="blur"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>

  <style>
      .title {
        font-family: Inter, Arial, sans-serif;
        font-size: 42px;
        font-weight: 750;
        fill: url(#titleGrad);
      }

      .subtitle {
        font-family: Inter, Arial, sans-serif;
        font-size: 14px;
        font-weight: 600;
        letter-spacing: 3px;
        fill: #64748B;
      }

      .pulse {
        animation: pulse 2s ease-in-out infinite;
        transform-origin: center;
      }

      .scan {
        animation: scan 3s linear infinite;
      }

      .float {
        animation: float 2.5s ease-in-out infinite;
      }

      @keyframes pulse {
        0%, 100% { opacity: .45; transform: scale(.9); }
        50% { opacity: 1; transform: scale(1.15); }
      }

      @keyframes scan {
        from { transform: translateX(-180px); }
        to { transform: translateX(1180px); }
      }

      @keyframes float {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-5px); }
      }
    </style>
  </defs>

  
  <rect width="1200" height="180" rx="18" fill="#F8FAFC"/>

  <!-- Decorative grid -->
  <g opacity=".35" stroke="#CBD5E1" stroke-width="1">
    <path d="M40 35H1160"/>
    <path d="M40 145H1160"/>
    <path d="M160 25V155"/>
    <path d="M1040 25V155"/>
  </g>

  <!-- Animated data flow -->
  <g class="scan" filter="url(#glow)">
    <circle cx="0" cy="112" r="4" fill="#38BDF8"/>
    <circle cx="25" cy="105" r="3" fill="#6366F1"/>
    <circle cx="50" cy="118" r="4" fill="#A855F7"/>
  </g>

  <!-- Forecast/data icon -->
  <g class="float" transform="translate(62 48)">
    <rect width="62" height="62" rx="16" fill="#EEF2FF"/>
    <path
      d="M14 43 L24 34 L33 39 L47 20"
      fill="none"
      stroke="url(#titleGrad)"
      stroke-width="4"
      stroke-linecap="round"
      stroke-linejoin="round"/>
    <circle class="pulse" cx="47" cy="20" r="5" fill="#6366F1"/>
  </g>

  <!-- Main title -->
  <text x="150" y="82" class="title">
    Ride Demand Forecasting
  </text>

  <!-- Subtitle -->
  <text x="153" y="112" class="subtitle">
    DATA PREP ENGINE
  </text>

  <!-- Live indicator -->
  <g transform="translate(920 65)">
    <circle class="pulse" cx="8" cy="8" r="8" fill="#22C55E" opacity=".25"/>
    <circle cx="8" cy="8" r="4" fill="#22C55E"/>
    <text x="24" y="13"
          font-family="Inter, Arial, sans-serif"
          font-size="12"
          font-weight="700"
          fill="#475569">
      PIPELINE READY
    </text>
  </g>

  <!-- Bottom animated line -->
  <rect x="150" y="132" width="900" height="2" rx="1" fill="#E2E8F0"/>
  <rect class="scan"
        x="150" y="132"
        width="180"
        height="2"
        rx="1"
        fill="url(#lineGrad)"
        filter="url(#glow)"/>
</svg>


## 🎯 Objective
This project builds a complete, end-to-end **data preparation pipeline** for a ride-hailing dataset — combining multiple raw sources, cleaning them, treating outliers, engineering features, and scaling everything into a single ML-ready dataset for **ride demand forecasting**.

## 📄 Problem Statement
Ride data is scattered across three different sources (rider profiles, trip logs, city zone info) in three different formats, with missing values, invalid records, and outlier fares/distances. The task is to merge, clean, transform, and enrich this data into one consistent, model-ready table.

The dataset contains:
- **Rider details** — demographics, signup info, ride history (`riders.csv`)
- **Trip records** — fare, distance, duration, payment mode (`trips.json`)
- **Zone info** — population density, traffic index, avg. speed (`city_zones.sql`)
- **Target-relevant fields** — `surge_flag` for surge-pricing prediction
  
## 📂 Project Files
| 📄 File | 📌 Description |
|---|---|
| ` 🧑‍💻 riders.csv` | Raw rider-level demographic & signup data |
| ` 🚕 trips.json` | Raw trip-level records |
| ` 🗺️ city_zones.sql` | SQLite script with zone-level reference data |
| ` 📓 main.ipynb` | Main notebook — loading, merging, cleaning, feature engineering |
| ` 🔗 merged_rides_dataset.csv` | Checkpoint after merging all 3 sources |
| ` 🤖 final_prepared_rides_dataset.csv` | Final ML-ready dataset |
| ` 📊 ride_demand_eda_report.html` | Auto-generated YData Profiling EDA report |


## 🛠️ Tools Used
`pandas` · `numpy` · `sqlite3` · `scikit-learn` (SimpleImputer, KNNImputer, StandardScaler, MinMaxScaler, LabelEncoder, OneHotEncoder) · `scipy` (zscore, winsorize) · `matplotlib` · `ydata-profiling`

## 🧬 Dataset Structure

| Source | Shape | Join Key |
|---|---|---|
| Riders | 300 × 9 | `rider_id` |
| Trips | 2000 × 9 | `rider_id`, `zone` |
| City Zones | 10 × 5 | `zone_name` |
| **Final Merged** | **2000 × 21** | — |

---

## 🔗 Part 1: Data Loading & Merging

Loaded three different formats and merged them into one table.

```python
riders = pd.read_csv("riders.csv")
trips = pd.read_json("trips.json")

conn = sqlite3.connect(":memory:")
conn.executescript(open("city_zones.sql").read())
city_zones = pd.read_sql("SELECT * FROM city_zones", conn)

merged_data = pd.merge(trips, riders, on="rider_id", how="left")
final_merged_data = pd.merge(merged_data, city_zones,
                              left_on="zone", right_on="zone_name", how="left")
final_merged_data.drop(columns=["zone_name"], inplace=True)
```

💡 **Insight:** Left-joining all three sources produced a clean **2000 × 21** table with **0 unmatched riders** and **0 unmatched zones** — a 100% referential match, so no rows were lost or duplicated.

---

## 🧹 Part 2: Data Cleaning

```python
# Numeric missing values → mean
numeric_cols = cleaned_data.select_dtypes(include=np.number).columns
cleaned_data[numeric_cols] = SimpleImputer(strategy="mean").fit_transform(cleaned_data[numeric_cols])

# Categorical missing values → most frequent
categorical_cols = cleaned_data.select_dtypes(include="object").columns
cleaned_data[categorical_cols] = SimpleImputer(strategy="most_frequent").fit_transform(cleaned_data[categorical_cols])

# Correlated numeric columns → KNN Imputer
knn_cols = ["duration_min", "distance_km", "fare_amount"]
cleaned_data[knn_cols] = KNNImputer(n_neighbors=5).fit_transform(cleaned_data[knn_cols])

# Fix dates + drop logically invalid rows
cleaned_data["ride_date"] = pd.to_datetime(cleaned_data["ride_date"], errors="coerce")
cleaned_data = cleaned_data[cleaned_data["fare_amount"] >= 0]
cleaned_data = cleaned_data[~((cleaned_data["distance_km"] == 0) & (cleaned_data["fare_amount"] > 0))]
```

💡 **Insight:** Mean imputation for numerics, mode for categoricals, and KNN (k=5) for the three correlated trip metrics (duration, distance, fare). Invalid rows — negative fares and zero-distance-but-billed rides — were dropped. Result: **0 missing values**, row count held steady.

---

## 📊 Part 3: Outlier Handling

```python
# Z-score: fare & distance
cleaned_data["fare_zscore"] = zscore(cleaned_data["fare_amount"])
cleaned_data["distance_zscore"] = zscore(cleaned_data["distance_km"])
fare_outliers = cleaned_data[cleaned_data["fare_zscore"].abs() > 3]      # 16 outliers

# IQR: duration
Q1, Q3 = cleaned_data["duration_min"].quantile([0.25, 0.75])
IQR = Q3 - Q1
duration_outliers = cleaned_data[(cleaned_data["duration_min"] < Q1 - 1.5*IQR) |
                                  (cleaned_data["duration_min"] > Q3 + 1.5*IQR)]   # 18 outliers

# Winsorize fare (1% each tail)
cleaned_data["fare_amount"] = winsorize(cleaned_data["fare_amount"], limits=[0.01, 0.01])
```

💡 **Insight:** Z-score flagged **16 fare outliers** (up to ₹465) and **3 distance outliers**; IQR flagged **18 duration outliers** (~0.90% of trips). Winsorization capped max fare from **₹472.29 → ₹379.63** (~19.6% cut) while shifting the mean by less than 0.3% — extreme values tamed **without deleting a single row**.

---

## 🔧 Part 4: Data Transformation

```python
# Datetime → features
cleaned_data["hour"] = cleaned_data["ride_date"].dt.hour
cleaned_data["day_of_week"] = cleaned_data["ride_date"].dt.dayofweek
cleaned_data["month"] = cleaned_data["ride_date"].dt.month

# Encoding
cleaned_data["gender_encoded"] = LabelEncoder().fit_transform(cleaned_data["gender"])
# One-Hot: payment_mode, zone | Ordinal: traffic_level (Low<Medium<High)

# Binning + skew correction
cleaned_data["ride_frequency"] = pd.qcut(cleaned_data["total_rides"], q=3, labels=["Low","Medium","High"])
cleaned_data["log_fare"] = np.log1p(cleaned_data["fare_amount"])
cleaned_data["log_distance"] = np.log1p(cleaned_data["distance_km"])
cleaned_data["sqrt_duration"] = np.sqrt(cleaned_data["duration_min"])
```

💡 **Insight:** `gender` label-encoded, `payment_mode`/`zone` one-hot encoded (no false ordinality), `traffic_level` ordinally encoded (true Low<Medium<High order). Right-skewed `fare_amount`/`distance_km` log-transformed; moderately-skewed `duration_min` square-root transformed. **10 new columns** added in total.

---

## ⚙️ Part 5: Feature Scaling

```python
scaling_cols = ["age","total_rides","cancelled_rides","avg_rating","distance_km",
                 "duration_min","fare_amount","population_density","traffic_index","avg_speed_kmph"]

standard_scaled = StandardScaler().fit_transform(cleaned_data[scaling_cols])   # mean≈0, std≈1
minmax_scaled   = MinMaxScaler().fit_transform(cleaned_data[scaling_cols])     # range [0,1]
```

💡 **Insight:** 10 numeric columns spanning very different units (age, ride counts, population density) were scaled two ways — **StandardScaler** for algorithms needing centered features (logistic regression, SVM, PCA), **MinMaxScaler** for bounded-input needs. Since Winsorization already tamed extreme fares, MinMax's outlier sensitivity is low-risk here.

---

## 🏗️ Part 6: Feature Construction

```python
cleaned_data["avg_ride_distance"]      = total_distance / cleaned_data["total_rides"]
cleaned_data["avg_ride_fare"]          = total_fare / cleaned_data["total_rides"]
cleaned_data["is_peak_hour"]           = cleaned_data["hour"].apply(lambda x: 1 if (7<=x<=9) or (18<=x<=21) else 0)
cleaned_data["days_since_signup"]      = (pd.Timestamp.today().normalize() - cleaned_data["signup_date"]).dt.days
cleaned_data["ride_cancellation_rate"] = cleaned_data["cancelled_rides"] / cleaned_data["total_rides"]
cleaned_data["fare_per_km"]            = cleaned_data["fare_amount"] / cleaned_data["distance_km"]
cleaned_data["surge_flag"]             = (cleaned_data["fare_per_km"] > 20).astype(int)
```

💡 **Insight:** 6 business-meaningful features engineered — rider behavior (`avg_ride_distance`, `avg_ride_fare`), timing (`is_peak_hour`), tenure (`days_since_signup`), risk (`ride_cancellation_rate`), and pricing (`surge_flag`, threshold ₹20/km).

---

## ✅ Part 7: Final Dataset

```python
final_data = cleaned_data.copy()
final_data.to_csv("final_prepared_rides_dataset.csv", index=False)
```

| Metric | Before | After |
|---|---|---|
| Rows | 2000 | 2000 |
| Missing Values | 0 | 0 |
| Fare Outliers | 16 | 0 |
| New Engineered Features | – | 6 |

💡 **Insight:** Row count held at **2000 → 2000 (100% retention)**, all missing values resolved, fare outliers reduced **16 → 0**, and **6** new engineered features added — confirming outlier treatment used capping (not deletion) throughout.

---

## 🎁 Part 8: Bonus — EDA Report & Visualizations

```python
from ydata_profiling import ProfileReport
ProfileReport(final_data, title="Ride Demand Dataset EDA Report", explorative=True)\
    .to_file("ride_demand_eda_report.html")
```

💡 **Insight:** Busiest date in the dataset was **2023-09-08 (11 rides)**, quietest was **2023-01-01 (1 ride)** — over a 10x swing, plausibly tied to the New Year holiday. Final `surge_flag` split: **70.75% No-Surge vs 29.25% Surge** — a moderately imbalanced (~7:3) target worth class-weighting in downstream models.

---

## 📂 Project Workflow
1. **Data Loading** → Import CSV, JSON, and SQL sources
2. **Merging** → Join Riders + Trips + City Zones on `rider_id` / `zone`
3. **Cleaning** → Mean, Mode, and KNN imputation + invalid-record removal
4. **Outlier Handling** → Z-score, IQR, and Winsorization
5. **Transformation** → Date features, encoding, binning, log/sqrt transforms
6. **Scaling** → StandardScaler vs MinMaxScaler comparison
7. **Feature Construction** → 6 new business-driven features
8. **Export** → `final_prepared_rides_dataset.csv`
9. **Bonus** → Auto EDA report + demand/surge visualizations

## 📈 Results & Insights
- ✅ Merged 3 sources (2000 × 21) with **100% match rate**, 0 unmatched keys
- ✅ **0 missing values** after mean / mode / KNN imputation
- ✅ Fare outliers reduced **16 → 0** via Winsorization, no rows deleted
- ✅ **10** new transformed columns + **6** new engineered features
- ✅ Dataset scaled two ways (StandardScaler & MinMaxScaler) for algorithm flexibility
- ✅ Final dataset exported as `final_prepared_rides_dataset.csv`, fully ML-ready

## 📌 Expected Outcomes
- A fully merged, cleaned, and imputed rider-trip-zone dataset with zero missing values
- Outlier-controlled `fare_amount`, `distance_km`, and `duration_min` via Z-score/IQR detection + Winsorization
- A rich, ML-ready feature set (encoded, scaled, and engineered) for ride-demand and surge-pricing forecasting
- An auto-generated EDA report for fast sanity-checking

## ⚙️ Installation & Setup
```bash
git clone https://github.com/yourusername/ride-demand-data-prep.git
cd ride-demand-data-prep
pip install -r requirements.txt
```

## 🙏 Thank You
Thanks for checking out this project! Feedback, suggestions, and contributions are always welcome.

⭐ If you found this project helpful, don't forget to star the repository and share it.
