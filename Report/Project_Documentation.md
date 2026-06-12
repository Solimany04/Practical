# Time Series Analysis: Hourly Energy Consumption

## Abstract / Executive Summary
This report presents a comprehensive time series analysis of hourly energy consumption across multiple electrical grid regions. The project entails data ingestion and preprocessing of over 1.09 million records, feature extraction to identify underlying seasonal and temporal patterns, and the application of both statistical moving averages and advanced forecasting algorithms. By evaluating Simple Moving Averages (SMA), Exponential Moving Averages (EMA), and leveraging the Facebook Prophet model, the analysis provides robust demand forecasts and comparative insights into regional energy utilization.

---

## 1. Introduction & Objectives
Predicting energy consumption is a critical task for grid stability, resource allocation, and sustainable energy management. The primary objectives of this study are to:
- Ingest and unify multi-regional hourly energy consumption datasets.
- Perform Exploratory Data Analysis (EDA) to uncover daily, weekly, and seasonal usage routines.
- Apply and evaluate the predictive accuracy of Simple Moving Averages (SMA) and Exponential Moving Averages (EMA).
- Utilize the Facebook Prophet framework for long-term time series forecasting.
- Conduct a comprehensive regional comparison to highlight consumption disparities across different grids.

---

## 2. Dataset Description & Preprocessing
The dataset comprises hourly megawatt (MW) energy consumption readings from multiple regional grids. To facilitate a holistic analysis, individual regional CSV files were concatenated into a unified structured dataset using automated globbing routines.

**Dataset Characteristics:**
- **Total Records:** 1,090,167 hourly observations.
- **Regions Covered (12):** PJM, PJME, PJMW, NI, AEP, DAYTON, DUQ, DOM, COMED, FE, DEOK, EKPC.
- **Preprocessing:** The `Datetime` column was parsed into standard pandas datetime objects and set as the DataFrame index. Subsequent feature engineering extracted granular temporal components: `Hour`, `DayOfWeek`, `Month`, `Year`, `DayOfYear`, and `Quarter`.

---

## 3. Exploratory Data Analysis (EDA) & Seasonality
Understanding human routines and climatic impacts is essential for modeling energy demand. Through feature extraction, the analysis isolated specific temporal frequencies to evaluate seasonal behavior.

**Key Analytical Steps:**
- **Hourly Trends:** Investigating intra-day peak and off-peak cycles.
- **Weekly & Monthly Patterns:** Assessing variations driven by weekday industrial operations versus weekend lulls, as well as seasonal heating and cooling demands across different months.

![Time Features & Seasonal Analysis](<./Images/time features & seasonal analysis.png>)

---

## 4. Moving Averages & Error Evaluation
To smooth out high-frequency noise and identify underlying trends, both Simple Moving Average (SMA) and Exponential Moving Average (EMA) models were implemented over a 30-day window.

**Methodology & Formulas:**
- **Simple Moving Average (SMA):** Computes the unweighted mean of the previous $n$ data points.
  $$ SMA_t = \frac{1}{n} \sum_{i=0}^{n-1} Y_{t-i} $$
  
- **Exponential Moving Average (EMA):** Applies exponentially decreasing weights, giving higher precedence to recent observations.
  $$ EMA_t = \alpha Y_t + (1 - \alpha) EMA_{t-1} $$
  *(where $\alpha$ represents the smoothing factor)*

**Performance Evaluation:**
The Mean Absolute Error (MAE) was utilized to quantify the prediction accuracy of the moving averages:
$$ MAE = \frac{1}{n} \sum_{i=1}^{n} |Y_i - \hat{Y}_i| $$

**Results (30-Day Window):**
- **30-Day SMA MAE:** $3266.65$
- **30-Day EMA MAE:** $2548.81$

*Insight:* The EMA significantly outperformed the SMA, indicating that recent energy consumption values are stronger predictors of immediate future demand due to the responsive nature of the exponential weighting.

![SMA, EMA, and Error Evaluation](<./Images/SMA, EMA, and Error Evaluation.png>)

---

## 5. Time Series Forecasting
For comprehensive long-term forecasting, the Facebook Prophet model was deployed. Prophet is an additive regression model designed to handle time series data with strong seasonal effects and historical trend changes.

**Methodology:**
The unified dataset was formatted to meet Prophet's strict input requirements (renaming the target variable to `y` and the time index to `ds`). The model was trained to capture daily, weekly, and yearly seasonality autonomously.

**Forecasting Performance:**
- **Prophet Model MAE:** $2883.33$

*Insight:* The Prophet model yielded a robust MAE of 2883.33 over the testing horizon. While its error metric is slightly higher than the short-term 30-Day EMA, Prophet excels in predicting longer horizons and maintaining seasonal structural integrity without relying strictly on the immediate prior lag.

![Forecasting with Facebook Prophet](<./Images/Forecasting with Facebook Prophet.png>)

![Forecasting with Facebook Prophet 2](<./Images/Forecasting with Facebook Prophet 2.png>)

---

## 6. Regional Comparisons
A cross-regional aggregation was performed to contrast the energy load profiles across the various grid territories. Analyzing these aggregates provides crucial insights for macro-level grid balancing.

**Highlight Regional Metrics:**
| Region | Average MW | Maximum MW | Minimum MW |
|--------|------------|------------|------------|
| **PJM** | 29,766.43 | 54,030.00 | 17,461.00 |
| **PJME** | 32,080.22 | 62,009.00 | 17,461.00 |
| **COMED** | 11,420.15 | 23,753.00 | 7,237.00 |
| **AEP** | 15,499.51 | 25,695.00 | 9,581.00 |
| **DAYTON** | 2,037.85 | 3,746.00 | 851.00 |

*Insight:* The PJME and PJM regions exhibit significantly higher baseline and peak loads compared to areas like DAYTON, directly correlating with population density, industrial footprint, and geographic coverage.

![Regional Comparisons](<./Images/Regional Comparisons.png>)

---

## 7. Conclusion & Future Work
This project successfully ingested and analyzed over a million records of hourly energy consumption data. The integration of engineered time features revealed distinct cyclic patterns critical for accurate modeling. 

**Conclusions:**
- The **Exponential Moving Average (MAE: 2548.81)** proved highly effective for short-term trend smoothing, outperforming the **Simple Moving Average (MAE: 3266.65)** by reacting more swiftly to recent load shifts.
- The **Facebook Prophet model** provided a reliable forecasting framework, achieving an **MAE of 2883.33**, demonstrating its capacity to map complex seasonal structures over extended periods.
- Regional analysis quantified the substantial variance in power loads, with PJME commanding the highest average output (32,080.22 MW).

**Future Work:**
To further enhance forecasting accuracy and grid reliability, future iterations of this project could:
1. **Incorporate Exogenous Variables:** Integrate historical weather data (temperature, humidity) and holiday schedules as regressors in the forecasting models.
2. **Advanced Deep Learning:** Implement Long Short-Term Memory (LSTM) networks or Temporal Fusion Transformers for complex, non-linear pattern recognition.
3. **Real-time Anomaly Detection:** Develop automated pipelines to detect sudden spikes or drops in energy consumption, allowing for rapid grid response.
