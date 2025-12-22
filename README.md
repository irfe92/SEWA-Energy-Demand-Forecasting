# SEWA-Energy-Demand-Forecasting
Capstone Project for Imperial College Business School's Professional Certificate Program

# SEWA Weather and Power Consumption Dataset (2021)

## Overview
This dataset provides weather and power consumption data for the Sharjah Electricity and Water Authority (SEWA) for the year 2021. Curated by Mohammed Saeed in 2023, it is adapted from the larger "SEWA Electricity Demand Forecasting -- 2020 and 2021" dataset hosted on Mendeley Data (Version 2). The data goes from 1 January 2021 to 31 December 2021 and is intended for electricity demand forecasting, predictive modeling, and energy policy analysis.

**Note:** This dataset contains only the data for the year 2021, which has been cleaned and adapted while retaining its analytical integrity.

## Dataset Details
- **Title:** SEWA Weather and Power Consumption Dataset for 2021
- **Curator:** Mohammed Saeed
- **Publication Year:** 2023
- **Source Dataset:** "SEWA Electricity Demand Forecasting -- 2020 and 2021"
- **Repository:** Mendeley Data
- **Version:** 2
- **DOI:** [10.17632/4rjc87zrd3.2](https://doi.org/10.17632/4rjc87zrd3.2)
- **Time Period:** 1 Jan 2021 – 31 Dec 2021 (Daily records)
- **Format:** Tabular data (e.g., CSV)

## Data Columns

| Column Name          | Description                          | Unit           |
|----------------------|--------------------------------------|----------------|
| Date                 | Date of the record                   | YYYY-MM-DD     |
| Day                  | Day of the week                      | Text (e.g., Monday) |
| MAX Tem              | Maximum temperature of the day       | Celsius (°C)   |
| Min Tem              | Minimum temperature of the day       | Celsius (°C)   |
| Max Hum              | Maximum humidity of the day          | Percentage (%) |
| Min Hum              | Minimum humidity of the day          | Percentage (%) |
| Temp                 | Actual / Average temperature         | Celsius (°C)   |
| Hum                  | Actual / Average humidity            | Percentage (%) |
| SEWA MIN LOAD(MW)    | SEWA minimum load                    | Megawatts (MW) |
| SEWA Peak Load(MW)   | SEWA peak load                       | Megawatts (MW) |
| SEWA Energy/hr.      | SEWA energy consumption per hour     | Megawatt-hours (MWh) |


## Project Goal : Energy Consumption Forecasting Using Tree-Based Models and Parameter Tuning
The primary objective of this project is to develop accurate predictive models for energy consumption outcomes by systematically tuning parameters, implementing cross-validation techniques, and applying theoretical knowledge gained from course.

## Methodology

### 1. Features
- **Target Variable:** Predict SEWA energy consumption (MWh/hour)
- **Features:** Weather parameters (temperature, humidity) and calendar variables


### 2. Data Preparation
Proposed preprocessing steps:
1. Handle missing values (if any) by generating a regular sequance of dates and mergind with the original data
2. Feature engineering:
   - Remove the 'C' and '%' characters and convert columns to numeric
   - Create lag features for time-series analysis of Temperature (lag 1, lag 2, lag 3)
   - Generate MA7 & MA14 of the dependent variable
   - Remove 1 outlier (last day of consumption series, maybe not the entire 24 hours were available for the last day)
   - Generate additional calendar variables: Month, Day of Yer, Day of Month, Week of Year, Day of Week, Is Week End
  
### 3. Train and Test Split     
Train-test split 80%/20%

### 3. Framework for Developing Predictive Models
I will start by using MLJAR AutoML to establish a performance benchmark, identify the most promising algorithms, gain feature insights, and obtain a summary report to guide my next steps.
It's an effortless starting point for refining one's data science intuition.

- **Phase 1: MLJAR AutoML for Tree-Based Models**
  - Gradient Boosting Machines (XGBoost, LightGBM, Catboost)
 
  **AutoML Results:**

 | Best model             | Name                 | Model Type | Metric Type | Metric Value | Train Time |
 |------------------------|----------------------|------------|-------------|--------------|------------|
 | 1_Default_LightGBM     | LightGBM             | mape       | 0.0430291   | 18.33        |
 | 2_Default_Xgboost      | Xgboost              | mape       | 0.0401356   | 15.78        |
 | 3_Default_CatBoost     | CatBoost             | mape       | 0.0369111   | 7.86         |
 | the best               | Ensemble             | Ensemble   | mape        | 0.0354686    | 0.38       |

The above table summarizes the performance of different models based on their metric values.

- **Phase 2: Catboost implementation**
 - Default Parameter Modeling (baseline)
   
   ![Catboost Default](https://raw.githubusercontent.com/irfe92/SEWA-Energy-Demand-Forecasting/main/catboost_default.png)
   
 - Prediction on Test based on Default Parameter Modeling
   
   Mean of actual values :  1487.5857142857142
   Mean of predicted values:  1485.9115968489148
   ![Predict on Test](https://raw.githubusercontent.com/irfe92/SEWA-Energy-Demand-Forecasting/main/Prediction_on_Test.png)

   Testing performance
   RMSE_test: 54.47
   MAPE_test: 0.03 (3%)
   R2: 0.99
   
 - Assess Variable Importance
  ![Variable Importance](https://raw.githubusercontent.com/irfe92/SEWA-Energy-Demand-Forecasting/main/Variable_Importance.png)
   
 - Adjust Explanatory Variables

   I will drop: I will drop 'Max Hum' & 'Day of Month'
   
 - Optuna Parameter Tuning
   
   **Final Parameters Optuna:**
      **iterations:** : 747
      **learning_rate** : 0.09
      **depth** : 10
      **l2_leaf_reg** : 2

   
 - RandomizedSearchCV & GridSearchCV
   I've tried both: GridSearchCV as I have a small parameter spaces and ideally would like to have the best parameters possible, and          RandomizedSearchCV to see how "good enough" results perform.
    **Final Parameters GridSearchCV:**
      **iterations:** : 900
      **learning_rate** : 0.02
      **depth** : 6
      **l2_leaf_reg** : 1
   
   
 - Fit Final Model
    I've kept the params from GridSearchCV only. 

 - Make Predictions
 - Evaluate Metrics 

