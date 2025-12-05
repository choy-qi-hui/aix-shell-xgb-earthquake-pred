# Project Introduction
This project is part of BlendED's AI+X Project-Based Learning (PBL), under the **Shell Project, Track 3: Mitigating Environmental Impacts Associated with Energy Production**. I took on the challenge of completing this project solo.

## Project Motivation and Purpose
Human activities such as wastewater injection and fluid production can induce seismicity, with studies showing strong correlations between cumulative injection volumes and increased earthquake occurrences. Motivated by this link, this project applies Machine Learning to:
1. Identify the **key factors** driving earthquake frequency and severity
2. Forecast earthquake rates and magnitudes over **short- and long-term windows (3 months, 1 year, and 5 years)**

These forecasts can provide actionable insights for city planners and government agencies to strengthen earthquake preparedness.

### Literature Review
The direction of this project was guided by two key research studies:
1. **Amini et al. (2024)** compared geological and operational predictors of induced seismicity using eight ML models, finding that depth to basement and total injection volume were strong predictors, with LightGBM and Random Forest performing best
2. **Qin et al. (2022)** demonstrated the effectiveness of Random Forest for forecasting seismicity rates across multiple time windows, identifying pore pressure and poroelastic stress rates as key features

Building on these findings, this project adopts a similar predictive framing but extends the approach by using XGBoost, which is a fast, high-accuracy gradient boosting model, to evaluate factor importance and generate spatially visualised forecasts for the Los Angeles Basin.

## Dataset Overview
Two datasets were provided for this project.

### 1. Well Injections and Production
| Aspect | Details |
| --- | --- |
| Description | Wells in the LA Basin with corresponding data on **water, gas and/or oil injection and production** occurrences (if any), from 1977–2018 |
| Useful Columns | - **Well details**: Well #, latitude, longitude <br> - **Injection details**: Water injected, gas injected, surface injection pressure, reported date <br> - **Production details**: Oil produced, water produced, gas produced, gravity of oil, casing pressure, tubing pressure, reported date |

### 2. Earthquake Catalog
| Aspect | Details |
| --- | --- |
| Description | Southern California **earthquake occurrences** from 1932–2025, with their corresponding details |
| Useful Columns | - **Earthquake locations**: latitude, longitude <br> - **Earthquake details**: magnitude, depth, location quality, date, time |

# Methodology
## Machine Learning Model
**XGBoost** was selected as the primary model due to its speed, accuracy, and ability to capture non-linear relationships while handling mixed and noisy features.  

## Project Approach
### Data Processing and Integration
All well, injection/production, and earthquake datasets were cleaned, merged, and filtered to remove irrelevant columns. Geospatial coordinates were converted into **grid IDs and polygon geometries** to enable spatial analysis.

### Feature Engineering
Key features were created to capture temporal and operational dynamics, including:
- **Rolling 3-month, 1-year, and 5-year averages and lagged values** for injection, production, earthquake rate, and magnitude
- **Total injected/produced** water, oil, and gas
- **Feature ratios** (oil–water, gas–water, gas–oil)

### Exploratory Geospatial Analysis
**Distribution maps** were generated to visualise well density and injection/production intensities across the LA Basin, highlighting regional patterns.

From the spatial distribution maps, there is a high concentration of wells in the southern Los Angeles Basin. These wells exhibit substantial water and oil injection and production activity, while gas injection and production remain minimal across the region. This clustering highlights operational hotspots that may contribute more strongly to seismic responses.

### Model Training and Prediction
XGBoost regression models were trained separately using the standard 80-10-10 train-val-test splits for each forecasting horizon. Predicted earthquake rates and magnitudes were then mapped using the geospatial grid to produce interpretable spatial visualisations.

# Results
## Model Performance
| Output | RMSE | R^2 |
| --- | --- | --- |
| Earthquake Rate (3M) | 0.792 | 0.367 |
| Earthquake Rate (1Y) | 0.464 | 0.647 |
| Earthquake Rate (5Y) | 0.315 | 0.842 |
| Average Earthquake Magnitude (3M) | 0.072 | 0.310 |
| Average Earthquake Magnitude (1Y) | 0.030 | 0.262 |
| Average Earthquake Magnitude (5Y) | 0.008 | 0.649 |

Model accuracy improves noticeably with longer forecasting windows.
- Earthquake rate: **RMSE decreases (0.792 → 0.464 → 0.315), while R² increases (0.367 → 0.647 → 0.842)** across the 3-month, 1-year, and 5-year predictions
- Average magnitude: **RMSE similarly decreases (0.072 → 0.030 → 0.008) and R² rises**, with the 5-year model achieving the strongest performance (R²=0.649)

These trends indicate that XGBoost produces more stable and reliable forecasts over longer time horizons.

## Feature Importance
**SHAP beeswarm plots** were used to interpret the XGBoost models because they provide a clear, model-agnostic way to understand how each feature influences predictions. Unlike traditional feature-importance scores, SHAP shows both the strength and direction of each feature’s impact for every data point, allowing for a more transparent explanation of why the model behaves the way it does.

The XGBoost models highlight a clear set of operational and geological drivers behind seismic behaviour.
- Earthquake rate: cumulative gas injected and produced, seasonal/time-based patterns, and total water injected → long-term fluid activity contributes to changes in subsurface pressure
- Earthquake magnitude: earthquake depth, water injection volumes, and recent magnitude history → both geological conditions and ongoing human activities shape the severity of seismic events

# Conclusions
This project provided valuable experience in geospatial analysis, feature engineering, and spatial ML visualisation, including creating geospatial grids and transforming raw well and seismic data into meaningful predictive features. More importantly, the results demonstrate how ML can be used to forecast earthquake rates and magnitudes with reliable accuracy.

The predictive insights and spatial maps generated in this project have important real-world applications. They can support government agencies in identifying high-risk zones, guiding limits on injection and production activities, optimising monitoring efforts, and informing policy interventions to minimise human-induced seismicity. By linking operational practices to seismic outcomes, this project highlights the potential for data-driven strategies to reduce and prevent earthquake occurrences associated with man-made activities.
