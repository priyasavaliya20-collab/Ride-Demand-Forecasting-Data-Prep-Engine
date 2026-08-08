<svg xmlns="http://www.w3.org/2000/svg" width="1600" height="700" viewBox="0 0 1600 700">

<defs>
  <linearGradient id="bg" x1="0" y1="0" x2="1" y2="1">
    <stop offset="0%" stop-color="#03142E"></stop>
    <stop offset="50%" stop-color="#0A4A78"></stop>
    <stop offset="100%" stop-color="#020A19"></stop>
  </linearGradient>

  <linearGradient id="text" x1="0" y1="0" x2="1" y2="0">
    <stop offset="0%" stop-color="#FFFFFF"></stop>
    <stop offset="50%" stop-color="#55E9FF"></stop>
    <stop offset="100%" stop-color="#8EA8FF"></stop>
  </linearGradient>

  <linearGradient id="carBody" x1="0" y1="0" x2="1" y2="1">
    <stop offset="0%" stop-color="#69F2FF"></stop>
    <stop offset="45%" stop-color="#168CC5"></stop>
    <stop offset="100%" stop-color="#063E70"></stop>
  </linearGradient>

  <filter id="glow">
    <feGaussianBlur stdDeviation="6" result="blur"></feGaussianBlur>
    <feMerge>
      <feMergeNode in="blur"></feMergeNode>
      <feMergeNode in="SourceGraphic"></feMergeNode>
    </feMerge>
  </filter>

  <filter id="bigGlow">
    <feGaussianBlur stdDeviation="13" result="blur"></feGaussianBlur>
    <feMerge>
      <feMergeNode in="blur"></feMergeNode>
      <feMergeNode in="SourceGraphic"></feMergeNode>
    </feMerge>
  </filter>

  <linearGradient id="shine">
    <stop offset="0" stop-color="white" stop-opacity="0"></stop>
    <stop offset=".5" stop-color="white" stop-opacity=".9"></stop>
    <stop offset="1" stop-color="white" stop-opacity="0"></stop>
  </linearGradient>
</defs>


<!-- ================= BACKGROUND ================= -->

<rect width="1600" height="700" fill="url(#bg)"></rect>

<g opacity=".07" stroke="#62EFFF">
  <path d="M0 100H1600"></path>
  <path d="M0 200H1600"></path>
  <path d="M0 300H1600"></path>
  <path d="M0 400H1600"></path>
  <path d="M0 500H1600"></path>
  <path d="M0 600H1600"></path>

  <path d="M100 0V700"></path>
  <path d="M300 0V700"></path>
  <path d="M500 0V700"></path>
  <path d="M700 0V700"></path>
  <path d="M900 0V700"></path>
  <path d="M1100 0V700"></path>
  <path d="M1300 0V700"></path>
  <path d="M1500 0V700"></path>
</g>


<!-- ================= GPS ROUTE ================= -->

<path d="M80 170
         C230 80 370 230 520 145
         S820 80 980 150
         S1280 80 1510 180" fill="none" stroke="#4DEAFF" stroke-width="2" stroke-dasharray="8 12" opacity=".35"></path>

<circle cx="80" cy="170" r="7" fill="#5BEFFF" filter="url(#glow)">
  <animate attributeName="r" values="5;11;5" dur="1.5s" repeatCount="indefinite"></animate>
</circle>

<circle cx="1510" cy="180" r="7" fill="#5BEFFF" filter="url(#glow)">
  <animate attributeName="r" values="5;11;5" dur="1.7s" repeatCount="indefinite"></animate>
</circle>


<!-- ================= MAIN RIDE VISUAL ================= -->

<g transform="translate(80 270)" filter="url(#glow)">

  <!-- Road -->
  <path d="M0 185 Q190 160 390 185" fill="none" stroke="#6AEFFF" stroke-width="5" opacity=".3"></path>

  <path d="M15 185 H365" stroke="#FFFFFF" stroke-width="3" stroke-dasharray="25 18" opacity=".35"></path>


  <!-- CAR -->
  <g>

    <!-- Car body -->
    <path d="
      M55 135
      L88 82
      Q98 65 120 65
      H285
      Q310 65 325 85
      L360 135
      H390
      Q410 135 410 155
      V172
      H25
      V153
      Q25 135 55 135 Z" fill="url(#carBody)" stroke="#78F3FF" stroke-width="4"></path>


    <!-- Roof -->
    <path d="
      M98 80
      Q108 60 130 60
      H280
      Q302 60 317 82" fill="none" stroke="#B8FAFF" stroke-width="3"></path>


    <!-- Windows -->
    <path d="
      M113 75
      L135 75
      H195
      V119
      H84
      L106 82
      Q109 76 113 75Z" fill="#041A34" stroke="#62EFFF" stroke-width="2"></path>

    <path d="
      M205 75
      H275
      Q290 75 300 90
      L319 119
      H205Z" fill="#041A34" stroke="#62EFFF" stroke-width="2"></path>


    <!-- Door lines -->
    <path d="M205 75 V160" stroke="#65EFFF" stroke-width="2" opacity=".5"></path>


    <!-- Door handle -->
    <rect x="265" y="128" width="24" height="4" rx="2" fill="#B8FAFF"></rect>


    <!-- Front light -->
    <ellipse cx="398" cy="143" rx="9" ry="6" fill="#FFFFFF">

      <animate attributeName="opacity" values=".2;1;.2" dur="1s" repeatCount="indefinite"></animate>

    </ellipse>


    <!-- Back light -->
    <ellipse cx="34" cy="143" rx="8" ry="6" fill="#FF6070"></ellipse>


    <!-- Wheels -->
    <g>
      <circle cx="105" cy="166" r="28" fill="#020A18" stroke="#6DEFFF" stroke-width="5"></circle>

      <circle cx="105" cy="166" r="9" fill="#62EFFF"></circle>

      <circle cx="330" cy="166" r="28" fill="#020A18" stroke="#6DEFFF" stroke-width="5"></circle>

      <circle cx="330" cy="166" r="9" fill="#62EFFF"></circle>
    </g>

    <!-- Car movement -->
    <animateTransform attributeName="transform" type="translate" values="0 0;35 0;0 0" dur="4s" repeatCount="indefinite"></animateTransform>

  </g>


  <!-- GPS PIN -->

  <g transform="translate(385 15)">

    <path d="
      M25 0
      C8 0 0 12 0 25
      C0 45 25 68 25 68
      S50 45 50 25
      C50 12 42 0 25 0Z" fill="#FFB84D" stroke="#FFF0B8" stroke-width="3" filter="url(#glow)"></path>

    <circle cx="25" cy="25" r="9" fill="#08213B"></circle>

  </g>

</g>


<!-- ================= HUGE RIDE TEXT ================= -->

<text x="290" y="520" text-anchor="middle" font-family="Arial, Helvetica, sans-serif" font-size="78" font-weight="900" letter-spacing="10" fill="url(#text)" filter="url(#glow)">
  RIDE
</text>

<text x="290" y="552" text-anchor="middle" font-family="Arial, Helvetica, sans-serif" font-size="15" font-weight="600" letter-spacing="5" fill="#9DEEFF">
  MOBILITY DATA
</text>


<!-- ================= MAIN TITLE ================= -->

<g text-anchor="middle">

  <text x="950" y="300" font-family="Arial, Helvetica, sans-serif" font-size="68" font-weight="900" letter-spacing="1" fill="url(#text)" filter="url(#glow)">
    Ride Demand
  </text>

  <text x="950" y="380" font-family="Arial, Helvetica, sans-serif" font-size="68" font-weight="900" letter-spacing="1" fill="url(#text)" filter="url(#glow)">
    Forecasting
  </text>

  <text x="950" y="435" font-family="Arial, Helvetica, sans-serif" font-size="31" font-weight="700" letter-spacing="6" fill="#FFB84D">
    DATA PREP ENGINE
  </text>

  <!-- Animated underline -->
  <rect x="700" y="458" width="500" height="3" rx="2" fill="#5CEBFF">

    <animate attributeName="width" values="80;500;80" dur="3s" repeatCount="indefinite"></animate>

    <animate attributeName="x" values="950;700;950" dur="3s" repeatCount="indefinite"></animate>

  </rect>

</g>


<!-- ================= FORECAST GRAPH ================= -->

<g>

  <path d="
    M470 610
    C550 570 600 595 670 575
    S790 535 860 560
    S990 520 1060 535
    S1190 480 1260 500
    S1390 450 1510 465" fill="none" stroke="#45E9FF" stroke-width="12" opacity=".18" filter="url(#bigGlow)"></path>


  <path d="
    M470 610
    C550 570 600 595 670 575
    S790 535 860 560
    S990 520 1060 535
    S1190 480 1260 500
    S1390 450 1510 465" fill="none" stroke="#65EEFF" stroke-width="4" stroke-linecap="round" stroke-dasharray="1300" stroke-dashoffset="1300">

    <animate attributeName="stroke-dashoffset" from="1300" to="0" dur="4s" repeatCount="indefinite"></animate>

  </path>


  <!-- Forecast points -->

  <g fill="#FFFFFF" stroke="#65EEFF" stroke-width="3">

    <circle cx="670" cy="575" r="6">
      <animate attributeName="r" values="5;10;5" dur="1.5s" repeatCount="indefinite"></animate>
    </circle>

    <circle cx="860" cy="560" r="6">
      <animate attributeName="r" values="5;10;5" dur="1.7s" repeatCount="indefinite"></animate>
    </circle>

    <circle cx="1060" cy="535" r="6">
      <animate attributeName="r" values="5;10;5" dur="1.9s" repeatCount="indefinite"></animate>
    </circle>

    <circle cx="1260" cy="500" r="6">
      <animate attributeName="r" values="5;10;5" dur="2.1s" repeatCount="indefinite"></animate>
    </circle>

    <circle cx="1510" cy="465" r="7">
      <animate attributeName="r" values="6;11;6" dur="1.3s" repeatCount="indefinite"></animate>
    </circle>

  </g>

</g>


<!-- ================= DATA PREP LABELS ================= -->

<g font-family="Arial" font-size="13" letter-spacing="2" fill="#8CEAF5" opacity=".65">

  <text x="480" y="525">CLEAN</text>
  <text x="650" y="500">TRANSFORM</text>
  <text x="850" y="480">FEATURE</text>
  <text x="1040" y="455">ENGINEER</text>
  <text x="1260" y="420">FORECAST</text>

</g>


<!-- ================= FLOATING DATA ================= -->

<g font-family="monospace" font-size="15" fill="#66EFFF" opacity=".25">

  <text x="470" y="120">101001 • 010101 • 110010</text>
  <text x="1170" y="120">GPS_DATA → DEMAND</text>
  <text x="470" y="665">DATA → CLEAN → TRANSFORM → FEATURE ENGINEERING</text>
  <text x="1170" y="665">MODEL → FORECAST</text>

</g>


<!-- ================= MOVING SHINE ================= -->

<rect x="-300" y="50" width="110" height="650" fill="url(#shine)" opacity=".08">

  <animateTransform attributeName="transform" type="translate" values="-500 0;2100 0" dur="5s" repeatCount="indefinite"></animateTransform>

</rect>

</svg>
<img width="1600" height="700" alt="download" src="https://github.com/user-attachments/assets/5401e11e-99ca-423a-a4a5-a7a6fa6e7a07" />



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

![Pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/numpy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![SQLite](https://img.shields.io/badge/sqlite3-07405E?style=for-the-badge&logo=sqlite&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![SciPy](https://img.shields.io/badge/scipy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/matplotlib-11557C?style=for-the-badge&logo=plotly&logoColor=white)
![ydata-profiling](https://img.shields.io/badge/ydata--profiling-4B8BBE?style=for-the-badge&logo=databricks&logoColor=white)

**Key modules used:** `SimpleImputer` · `KNNImputer` · `StandardScaler` · `MinMaxScaler` · `LabelEncoder` · `OneHotEncoder` (scikit-learn) &nbsp;|&nbsp; `zscore`, `winsorize` (scipy)

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
