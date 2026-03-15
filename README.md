This project applies log-linear multiple regression in R to the 2019 Inside Airbnb NYC dataset (48,895 listings) to uncover what drives nightly price variation across Manhattan, Brooklyn, Queens, the Bronx, and Staten Island. Five stepwise models are built, starting from room type alone (R²=0.42) up to the full model including neighbourhood, minimum nights, availability, and review metrics (R²=0.51). Key findings: entire homes earn ~55% more than private rooms; Manhattan commands a ~75% premium over the Bronx. Model diagnostics include residual plots, QQ-plots, and VIF checks.

## 📒 Notebook Sections

---

### 1️⃣ 🏙️ Business Problem
> Helping Airbnb hosts move from gut-feel pricing to data-driven strategy

- ❓ **The Problem** — Most hosts price by scanning nearby listings, not by data
- 📉 **The Risk** — Too high = empty calendar; Too low = lost revenue
- 🎯 **The Goal** — Quantify how room type, location, and host behaviour
  jointly drive nightly prices across all 5 NYC boroughs

**Research Questions:**
1. What explains price variation — location, room type, or guest activity?
2. How much does accommodation type affect the final price?
3. Do review counts and availability matter as much as location?

---

### 2️⃣ 📦 Data Description
> Inside Airbnb NYC 2019 dataset — listing-level snapshot

| Detail | Value |
|---|---|
| 📊 Observations | 48,895 listings |
| 📋 Variables | 16 columns |
| 🗺️ Coverage | All 5 NYC boroughs (Manhattan, Brooklyn, Queens, Bronx, Staten Island) |
| 🌐 Source | [Kaggle — AB_NYC_2019.csv](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data) |
| 🎯 Target Variable | `price` (modelled as `log(price)`) |

**Cleaning Rules Applied:**
- ❌ Removed non-positive prices
- ❌ Constrained `minimum_nights ≤ 365`
- 🔄 Replaced missing `reviews_per_month` → `0`
- 📐 Modelled `log(price)` to handle right-skew

---

### 3️⃣ 🔍 Exploratory Data Analysis (EDA)

| Figure | What It Shows | Key Insight |
|---|---|---|
| 📊 Fig 2.1 | Price histogram | Right-skewed — most listings < $200/night |
| 🗺️ Fig 2.2 | Price by borough boxplot | Manhattan & Brooklyn are most expensive |
| 🏠 Fig 2.3 | Price by room type | Entire homes command highest medians |
| 📍 Fig 2.4 | Lat/Long scatter map | Listings cluster in Manhattan & N. Brooklyn |
| ⭐ Fig 2.5 | Price vs reviews scatter | More reviews → slightly lower price (turnover effect) |
| 📅 Fig 2.6A | Availability histogram | Most listings open < 100 days/year (part-time hosts) |
| 🔥 Fig 2.7 | Correlation heatmap | Low numeric correlations → categorical variables carry signal |

---

### 4️⃣ 📈 5 Stepwise Regression Models
> Building from simple to full specification — watching R² grow

| Model | Formula | R² | Key Addition |
|---|---|---|---|
| 1️⃣ Model 1 | `log(price) ~ room_type` | 0.4199 | Room type alone explains 42%! |
| 2️⃣ Model 2 | + `neighbourhood_group` | 0.4907 | Borough adds 7% more |
| 3️⃣ Model 3 | + `minimum_nights` | 0.4933 | Each extra night required → −0.2% price |
| 4️⃣ Model 4 | + `availability_365` | 0.5078 | More availability → +0.06% per day |
| 5️⃣ **Model 5** | + `number_of_reviews` + `reviews_per_month` | **0.5103** | ⭐ Best full model |

**🏆 Key Coefficients from Model 5:**

| Predictor | Effect on Price | Interpretation |
|---|---|---|
| 🏠 Entire Home vs Shared Room | **−69%** 🔴 | Guests pay massive premium for privacy |
| 🏠 Private Room vs Entire Home | **−54%** 🔴 | Privacy matters more than location |
| 🗽 Manhattan vs Bronx | **+75%** 🟢 | Location premium is real |
| 🌉 Brooklyn vs Bronx | **+28%** 🟢 | Secondary location premium |
| 🗺️ Queens vs Bronx | **+13%** 🟡 | Moderate premium |

---

### 5️⃣ 🎨 Effect Visualizations
> Model 5 predictions plotted across key dimensions

| Plot | Description |
|---|---|
| 📊 Fig 4.1.1 | Predicted price by room type (Manhattan fixed) |
| 🗺️ Fig 4.1.2 | Predicted price by borough (Entire Home fixed) |
| 📉 Fig 4.1.3 | Predicted price vs number of reviews (downward line) |
| 📈 Fig 4.1.4 | Predicted price vs availability (upward line) |
| 🎯 Fig 4.2 | Coefficient plot with error bars — all predictors |

---

### 6️⃣ 🩺 Model Diagnostics

| Check | Tool | Result |
|---|---|---|
| 📉 Linearity | Residuals plot | ✅ Even spread around zero |
| 📐 Normality | Q-Q Plot | ✅ Approximate normality (minor tail deviation) |
| 🔎 Multicollinearity | VIF scores | ✅ No serious multicollinearity detected |
| 📊 Homoscedasticity | Residuals histogram | ✅ Approximately normal distribution |
