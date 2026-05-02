# 🌞 A Systematic Pre-Modeling Optimization Framework for Solar Power Forecasting Using Gradient Boosting Ensembles

> **Presented at the 4th Annual Graduate Research Conference — Lamar University**
> 📅 Monday, April 20, 2026 | 📍 Setzer Student Center, Lamar University, Beaumont, TX


---

## 👤 Author

**Md Rayhan Chowdhury**
Graduate Researcher, Department of Electrical Engineering
Lamar University, Beaumont, Texas

**Supervisor:** Dr. Farah Ferdaus
Department of Electrical Engineering, Lamar University

---

## 🎓 Conference

**4th Annual Graduate Research Conference at Lamar University**
- 📍 Setzer Student Center, Lamar University
- 📅 April 20, 2026 | 9:00 AM – 5:30 PM
- 🎤 Keynote Address by **Dr. Gene Theodori**, Associate Provost for Academic and Research Administration
- 🏛️ Organized by the College of Graduate Studies, Lamar University

---

## 📌 Research Overview

Most existing solar power forecasting research focuses on **comparing algorithms** (LSTM, ANN, CNN-LSTM, Random Forest) while ignoring three critical pre-modeling issues that systematically suppress accuracy. This research proposes a **systematic pre-modeling optimization framework** that addresses these issues before any algorithm is selected.

### 🔴 The Three Ignored Problems in Prior Work

| Problem | Description |
|---|---|
| **Night-time Zeros** | 62.6% of hourly records are trivially zero (no sunlight), artificially inflating R² |
| **Raw Features Only** | No cyclic time encoding, physics interactions, or lag/memory features |
| **Skewed Target** | Right-skew causes loss functions to over-penalize rare peak-hour errors |

---

## ❓ Research Questions

- **RQ1:** How much accuracy gain comes from pre-modeling decisions vs. algorithm selection?
- **RQ2:** Can structured feature engineering sustain R² > 0.90 across 1–10 day forecast horizons?
- **RQ3:** Does HistGradientBoosting (HGBR) with log-transformed targets outperform published LSTM/RFR baselines?

---

## ⚙️ Methodology: Pre-Modeling Optimization Framework

### Step 1 — Daytime Filtering
Remove all night-time zero records to prevent trivial predictions from inflating accuracy metrics.

### Step 2 — Feature Engineering (6 → 26 Features)

Starting from 6 raw inputs:
`wind speed · sunshine · air pressure · solar radiation · temperature · humidity`

Three groups of engineered features were created:

#### 🕐 Cyclic Time Features (6 features)
| Feature | Purpose |
|---|---|
| `hour_sin`, `hour_cos` | Encode time-of-day cyclically |
| `month_sin`, `month_cos` | Encode month cyclically |
| `doy_sin`, `doy_cos` | Encode day-of-year cyclically |

> Cyclic encoding ensures the model understands that 11 PM and 1 AM are close together, and December flows into January.

#### ⚡ Physics Interaction Features (5 features)
| Feature | Physics Meaning |
|---|---|
| `rad × sunshine` | Combined radiation and sunshine duration |
| `rad²` | Non-linear radiation effect |
| `sunshine²` | Non-linear sunshine effect |
| `CSI × temp × humidity` | Cloud Severity Index modulating temperature-humidity effect |

#### 📈 Lag & Rolling Features (9 features)
| Feature | Purpose |
|---|---|
| `solar_radiation_lag1`, `lag2` | Radiation from 1–2 hours ago |
| `solar_power_lag1`, `lag2` | Power output from 1–2 hours ago |
| `rad_roll3_mean` | 3-hour rolling average radiation |
| `pow_roll3_mean` | 3-hour rolling average power |
| + additional rolling windows | Capture longer-term trends |

> Lag and rolling features give the model **memory** — it learns that yesterday's patterns influence today's forecast.

### Step 3 — Log Transformation
Apply log-transform to the target variable to compress right-skew and balance error distribution across peak and off-peak hours.

### Step 4 — Algorithm Selection & Tuning
Compare RFR, GBR, HGBR, and Ensemble models with hyperparameter tuning.

---

## 📊 Results

### Ablation Study — R² Improvement at Each Step

| Step | Model | R² |
|---|---|---|
| Baseline | RFR (raw features) | 31.0% |
| + Daytime Filter | RFR | 22.6% *(temporary dip — less data)* |
| + Feature Engineering | RFR | 74.1% |
| + Log Transform | RFR | 70.2% *(temporary dip — more honest metric)* |
| + HGBR Algorithm | **HGBR (final)** | **90.4%** |

**Key Finding:** Pre-modeling steps contributed **+0.39 R² points** vs. only **+0.20** from algorithm upgrade — a **2:1 ratio**.

### Multi-Horizon Forecasting Performance

| Forecast Horizon | RMSE | R² |
|---|---|---|
| 1 day ahead | 39.8 | 0.9545 |
| 3 days ahead | 34.7 | 0.9451 |
| 5 days ahead | 40.9 | 0.9511 |
| 10 days ahead | 90.1 | **0.9654** |

> GBR and HGBR maintain R² > 0.94 across all forecast horizons.

---

## 🏆 Key Conclusions

- ✅ Pre-modeling decisions (filtering + feature engineering + log-transform) contribute **2× more accuracy** than algorithm selection
- ✅ HGBR achieves **R² = 0.9037** on full test set, matching published LSTM results (~0.91–0.92) **without GPU infrastructure**
- ✅ All models sustain **R² > 0.90** at 1-, 3-, 5-day horizons; HGBR reaches **0.951 at 10 days**
- 🔮 **Future work:** SHAP explainability, transfer learning across geographies, quantile boosting for probabilistic forecasts

---

## 🛠️ Tools & Dataset

| Item | Details |
|---|---|
| **Dataset** | Hourly solar power plant, 8,760 records (2017) |
| **Language** | Python |
| **Libraries** | scikit-learn, Matplotlib, NumPy, Pandas |
| **Poster** | LaTeX (beamerposter) |
| **Models** | RFR, GBR, HGBR, Ensemble |

---

## 📚 References

1. Balal et al., *IEEE GreenTech 2025*
2. Kim et al., *Electrical Power Systems Research*, 2023
3. Lim et al., *Energies*, vol. 15, 2022
4. Mystakidis et al., *Computing*, 2023
5. Yagli et al., *Solar Energy*, vol. 208, 2020
6. Inman et al., *Progress in Energy and Combustion Science*, 2013

---

## 📬 Contact

**Md Rayhan Chowdhury**
📧 mchowdhury15@lamar.edu; rayhan.eee.ruet@gmail.com
🏛️ Department of Electrical Engineering, Lamar University

---

*This research was presented as a poster at the 4th Annual Graduate Research Conference, College of Graduate Studies, Lamar University, April 20, 2026.*
