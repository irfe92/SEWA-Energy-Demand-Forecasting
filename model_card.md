# Model Card: Energy Demand Forecasting Models

## Model 1: AutoML (MLJAR)

### Model Description
**Type:** Automated Machine Learning ensemble  
**Framework:**  AutoML  
**Training Time:** ~2 min
**Description:** Automated pipeline performing feature preprocessing, algorithm selection, and hyperparameter tuning across multiple model families (LightGBM, XGBoost, Catboost). Selected best-performing model from ensemble.

### Performance Characteristics
- **Strengths:** Minimal manual intervention, benchmark establishment, discovers non-obvious algorithms
- **Weaknesses:** Black-box optimization, computationally expensive, less control over feature engineering
- **Best For:** Rapid baseline establishment and algorithm discovery

### Requirements
- Minimal domain knowledge input

---

## Model 2: CatBoost Default

### Model Description
**Type:** Gradient Boosting on Decision Trees  
**Framework:** CatBoost v1.2+  
**Parameters:** Default settings (iterations=1000, learning_rate=0.03, depth=6)  
**Key Feature:** Native handling of categorical variables, built-in ordered boosting

### Performance Characteristics
- **Training Time:** ~1 minute
- **Interpretability:** Medium (feature importance available, "golden feature" support)
- **Baseline MAPE:** 3%
- **Strengths:** Robust to overfitting, automatic categorical handling, good defaults
- **Weaknesses:** Suboptimal hyperparameters for specific problem

### Intended Use
- Quick baseline after feature engineering
- Benchmark for tuned models
- Scenarios requiring fast iteration

---
## Model 3: CatBoost Tuned (GridSearchCV)

### Model Description
**Type:** Optimized Gradient Boosting  
**Framework:** CatBoost with scikit-learn wrapper  
**Tuning Method:** Exhaustive Grid Search with 3-fold time-series cross-validation  
**Optimal Parameters:**: -iterations: : 900 -learning_rate : 0.02 -depth : 6 -l2_leaf_reg : 1

### Performance Characteristics
- **Training Time:** ~20 min 
- **MAPE Improvement:** ~1% over default 
- **Strengths:** Optimized for specific problem, validated robustness via CV
- **Weaknesses:** Computationally intensive tuning phase

### Recommendations
✅ **Use when:** Model performance is critical, compute resources available  
⚠️ **Consider:** Tuning time vs. performance gain trade-off  
🔍 **Monitor:** Potential overfitting to validation split
