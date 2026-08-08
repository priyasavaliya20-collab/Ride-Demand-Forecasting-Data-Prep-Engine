
<img width="1600" height="700" alt="download" src="https://github.com/user-attachments/assets/5401e11e-99ca-423a-a4a5-a7a6fa6e7a07" />



## 🎯 Objective


<img width="1600" height="900" alt="ChatGPT Image Aug 8, 2026, 11_19_59 PM" src="https://github.com/user-attachments/assets/5202387a-4b10-4cc3-bb62-53f5642d0cad" />



## ♻️ WorkFlow :-

<img width="1200" height="620" alt="pic" src="https://github.com/user-attachments/assets/3b34c692-d359-4eb3-a4fd-2f51f9dcf6bc" />



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

---

## 🎬 Project Demo

[![Watch Demo](https://img.shields.io/badge/Watch%20Demo-Google%20Drive-blue?style=for-the-badge&logo=googledrive&logoColor=white)](https://drive.google.com/file/d/1215xHkwTyCsBinIyIcUpixhc1skNKj2z/view?usp=sharing)

📹 Click the badge above to watch the complete project demonstration.

---

## 🧬 Dataset Structure

| Source | Shape | Join Key |
|---|---|---|
| Riders | 300 × 9 | `rider_id` |
| Trips | 2000 × 9 | `rider_id`, `zone` |
| City Zones | 10 × 5 | `zone_name` |
| **Final Merged** | **2000 × 21** | — |

---

## 🔗 Part 1: Data Loading & Merging

<img width="1600" height="500" alt="pic" src="https://github.com/user-attachments/assets/118b78f4-f9ca-42b9-a89b-a0a692b76176" />


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

<img width="1600" height="500" alt="pic" src="https://github.com/user-attachments/assets/0d250ba5-f49c-4cfb-b362-c2dc53d9e330" />



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

<img width="1600" height="500" alt="pic" src="https://github.com/user-attachments/assets/2cda04b0-8a75-48b6-970a-42e6cec7a0ee" />




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


<img width="1600" height="500" alt="pic" src="https://github.com/user-attachments/assets/4d7ef5cf-5eae-44b8-8317-3b5c99996c31" />




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


<img width="1600" height="500" alt="pic" src="https://github.com/user-attachments/assets/bc717d24-d9cc-40b6-b8e0-bc2f2d505e82" />




```python
scaling_cols = ["age","total_rides","cancelled_rides","avg_rating","distance_km",
                 "duration_min","fare_amount","population_density","traffic_index","avg_speed_kmph"]

standard_scaled = StandardScaler().fit_transform(cleaned_data[scaling_cols])   # mean≈0, std≈1
minmax_scaled   = MinMaxScaler().fit_transform(cleaned_data[scaling_cols])     # range [0,1]
```

💡 **Insight:** 10 numeric columns spanning very different units (age, ride counts, population density) were scaled two ways — **StandardScaler** for algorithms needing centered features (logistic regression, SVM, PCA), **MinMaxScaler** for bounded-input needs. Since Winsorization already tamed extreme fares, MinMax's outlier sensitivity is low-risk here.

---

## 🏗️ Part 6: Feature Construct

<img width="1600" height="500" alt="pic" src="https://github.com/user-attachments/assets/c42bc621-6222-49e1-8590-9e10d78fb9c4" />

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

## ✅ Part 7: Final Dataset !

<img width="1600" height="500" alt="pic" src="https://github.com/user-attachments/assets/500abe6a-f318-4009-91a9-fb38d56f29b0" />


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

<img width="1600" height="500" alt="pic" src="https://github.com/user-attachments/assets/f4f77619-bd88-40ae-a245-62f721d1c093" />


```python
from ydata_profiling import ProfileReport
ProfileReport(final_data, title="Ride Demand Dataset EDA Report", explorative=True)\
    .to_file("ride_demand_eda_report.html")
```

💡 **Insight:** Busiest date in the dataset was **2023-09-08 (11 rides)**, quietest was **2023-01-01 (1 ride)** — over a 10x swing, plausibly tied to the New Year holiday. Final `surge_flag` split: **70.75% No-Surge vs 29.25% Surge** — a moderately imbalanced (~7:3) target worth class-weighting in downstream models.

---



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
