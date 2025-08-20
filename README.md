# README – Passenger Traffic Forecasting Project

## Project Overview
This project analyzes international passenger traffic data to uncover busiest and least trafficked routes, highlight long-term and seasonal trends, and forecast future demand.  

The case study focuses on the **Sydney–Auckland route**, AeroConnect’s busiest international connection. Both **Random Forest (machine learning)** and **Prophet (time-series forecasting)** models were tested.  

---

## Process

### 1. Data Understanding and Cleaning
- Combined raw passenger traffic records into a structured dataset.  
- Removed routes with 0 passengers (e.g., *Melbourne–Denver*, *Brisbane–Chicago*).  
- Standardized date formats and created a continuous monthly time series.  

The cleaned dataset is available here: [link](https://docs.google.com/spreadsheets/d/1sczGL4ncKj5aynOoPYD_YwnbMPlNdoYJ1cmltj1WDts/edit?usp=sharing).

---

### 2. Exploratory Data Analysis (EDA)
Several visualizations were created to better understand route-level behavior:  

- **Route Traffic Distribution**  
  - Bar charts showed the **top 10 busiest routes** (e.g., *Sydney–Auckland*, *Sydney–Singapore*).  
  - Also revealed the **bottom 10 least trafficked routes** (e.g., *Hobart–Tokyo*, *Townsville–San Francisco*).  

- **Yearly Trends**  
  - Line plot showed steady growth from **1985–1988**, a **peak in 1988 (~7.8M passengers)**, and a **sharp decline in 1989 (~3.8M)**.  

- **Seasonality**  
  - Month-level plots revealed **peaks in January, August, December** and consistent **troughs in February**.  

- **Geographic Spread**  
  - Aggregated plots showed **New Zealand (~8M passengers)**, **Singapore**, and the **U.S.** as top destination markets.  

These graphs validated strong **seasonal cycles** and provided context for selecting forecasting models.  

---

### 3. Modeling

#### Random Forest (RF)
We engineered new features to allow RF to capture both **short-term memory** and **seasonality**:  
- **Lag Features:** Passenger counts from 1, 3, and 12 months prior.  
- **Rolling Averages:** 3- and 6-month moving averages.  
- **Time Features:** Year, month, sine/cosine transformations for cyclic monthly seasonality.  

**Results (after hyperparameter tuning):**
- MAE: ~6,263  
- RMSE: ~7,117  
- MAPE: ~11.9%  
- R²: ~0.54  

RF captured non-linear effects and interactions well.  
While more accurate in raw error terms, RF was **less transparent** in explaining seasonal cycles.  

---

#### Prophet
Prophet was chosen for its strength in handling **trend + seasonality** in time series.  

- **Setup:** Monthly frequency, additive seasonal components, yearly seasonality enabled.  
- **Training:** Fitted on Sydney–Auckland passenger data up to 12 months before the end.  
- **Forecasting:** Generated a **12-month ahead forecast** with confidence intervals.  

**Results:**
- MAE: ~9,870  
- RMSE: ~11,636  
- MAPE: ~17.8%  

Although less accurate numerically, Prophet provided **clear seasonal decomposition** and **interpretable forecasts**, making it better aligned with **airline planning**.  

---

### 4. Forecasting Results
Prophet’s 12-month forecast for **Sydney–Auckland** extended the strong cyclical patterns seen historically:  
- Clear **peaks in holiday months** and **dips in February**.  
- **Uncertainty intervals** reflected potential variability in demand.  
- Forecast plots showed:  
  - Strong alignment of predicted vs. actual values on historical data.  
  - Forward-looking projections consistent with past seasonal cycles.  
  - Clear visualization of both the **central forecast** and the **upper/lower bounds**.  
