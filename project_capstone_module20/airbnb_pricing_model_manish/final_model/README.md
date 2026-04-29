# Airbnb Price Optimizer

**Manish Kumar**  
**UC Berkeley MLAI Program Capstone Project**

## Table of Contents
- [Executive Summary](#executive-summary)
- [Rationale](#rationale)
- [Research Question](#research-question)
- [Data Sources](#data-sources)
- [Methodology](#methodology)
- [Results](#results)
- [Project Files](#project-files)
- [Conclusion](#conclusion)

---

## Executive Summary

This project explores **dynamic price prediction** for Airbnb properties using structured listing data from **New York City** and **Los Angeles**. The objective is to recommend optimal nightly prices based on property features, location, and amenities — balancing revenue potential with occupancy rates.

**Key Achievement**: Developed multiple machine learning models achieving **R² scores up to 0.71**, providing hosts with data-driven pricing recommendations.

---

## Rationale

In the competitive short-term rental market, pricing decisions directly impact:
* **Host revenue** and profitability
* **Occupancy rates** and booking frequency  
* **Guest satisfaction** and review scores

Unlike traditional hotels, Airbnb listings vary widely in:
* **Amenities** and property types
* **Booking flexibility** and cancellation policies
* **Location-specific** market dynamics

This project addresses these challenges by building a **data-driven dynamic pricing model** that assists Airbnb hosts in setting optimal nightly prices based on:
* Property attributes and features
* Location and neighborhood factors
* Market demand patterns

---

## Research Question

**How can we accurately predict the nightly price of an Airbnb property based on its characteristics, amenities, and location to optimize host revenue while maintaining competitive occupancy rates?**

---

## Data Sources

### Primary Dataset
* **Source**: [Kaggle Airbnb Price Dataset](https://www.kaggle.com/datasets/rupindersinghrana/airbnb-price-dataset/data)
* **Size**: Over 74,000 Airbnb listings from major U.S. cities
* **Focus**: NYC and LA markets (≈53,000 records after filtering)

### Data Understanding

**Initial Analysis Revealed**:
* **Log price** (target variable) is moderately skewed and benefits from log transformation
* **Pricing factors** include accommodates, bedrooms, room type, and zip code
* **Amenities analysis**: Decomposed 119 binary features, selected top 20 most relevant
* **Feature cleaning**: Removed common amenities (e.g., "Wireless Internet", "Air conditioning") appearing in >95% of listings

### Key Fields Used
* **Pricing**: `price`, `log_price` (raw and log-transformed nightly prices)
* **Property**: `property_type`, `room_type`, `accommodates`, `bathrooms`, `bedrooms`, `beds`
* **Location**: `city`, `zipcode`
* **Features**: Top 20 amenities (extracted via correlation analysis with log_price)

---

## Methodology

1. **Data Cleaning and Preprocessing**
   * Remove duplicates and null values
   * Feature engineering and selection
   * Data type conversions and standardization

2. **Exploratory Data Analysis (EDA)**
   * Statistical analysis and visualization
   * Correlation analysis between features and target
   * Data quality assessment

3. **Model Development**
   * **Multiple Regression Models**: Linear, Ridge, Lasso, Random Forest, XGBoost, SVR
   * **Cross-Validation**: 5-fold cross-validation for robust evaluation
   * **Hyperparameter Tuning**: Grid search optimization

4. **Model Evaluation**
   * Performance metrics: MAE, RMSE, R²
   * Model comparison and selection
   * Ensemble methods (Stacking)

---

## Results

### Model Comparison Summary

| Model | MAE | RMSE | R² | Performance Notes |
|-------|-----|------|----|-------------------|
| **Random Forest** | 0.2901 | 0.3994 | 0.6666 | Simple yet effective baseline |
| **Linear Regression** | 0.2803 | 0.3842 | 0.6914 | Simple, interpretable baseline |
| **Ridge** | 0.2798 | 0.3833 | 0.6929 | Slight improvement with regularization |
| **Lasso** | 0.3094 | 0.4179 | 0.6350 | Underperformed — likely over-regularized |
| **XGBoost** | 0.2757 | 0.3766 | 0.7035 | Strong non-linear ensemble |
| **SVR** | **0.2666** | **0.3709** | **0.7125** | ⭐ **Best overall performer** |
| **XGBoost + Ridge** | 0.2719 | 0.3721 | 0.7105 | ✅ **Strong ensemble** |

### Key Findings

**1️⃣ SVR (Support Vector Regression)**
* **Best performer** across all metrics (MAE, RMSE, R²)
* **Captures complex non-linear relationships** effectively
* **Limitation**: Poor scalability (`600+ minutes` for 50k+ rows)

**2️⃣ XGBoost + Ridge Stacking Regressor**
* **Excellent balance** of accuracy and scalability
* **Second-best performance** overall
* **Production-ready** for large-scale deployment

**3️⃣ XGBoost**
* **Close second** to SVR in performance
* **Preferred choice** for larger datasets
* **Good scalability** and feature importance analysis

**4️⃣ Linear Models (Linear, Ridge)**
* **Reasonable performance** but lag behind ensemble methods
* **High interpretability** for business stakeholders

**5️⃣ Lasso**
* **Consistently underperformed**
* **Likely over-regularized** for this dataset

### Recommendations

**For Small-to-Medium Datasets**:
* Use **SVR** for maximum accuracy
* Accept longer training times for best results

**For Production Deployment**:
* Use **XGBoost + Ridge** ensemble
* Provides excellent accuracy with good scalability
* Enables feature importance analysis

---

## Project Files

* **[Initial Report README.md](../README.md)** - Project overview and initial analysis
* **[Exploratory Data Analysis](../basic_model_eda_airbnb_pricing.ipynb)** - Initial data exploration and baseline model
* **[Final Model Development](final_models_airbnb_pricing.ipynb)** - Advanced model development and comparison
* **[Final Report README.md](README.md)** - Final Modeling and Loss functional comparison to observe best possible modeling technic
  
### Summary of Initial Analysis

**Dataset**: Kaggle Airbnb Price Dataset (~74,000 listings from NYC and LA)

**Key Work**: 
* Data cleaning and feature engineering (selected top 20 amenities from 119 total)
* City-focused analysis (NYC: 32K, LA: 22K listings)
* Baseline Random Forest model (R²: 0.67, MAE: 0.29)

**Outcome**: Established foundation for final model optimization.

---

## Conclusion

This project successfully demonstrates that **machine learning models can effectively predict Airbnb nightly prices** using structured property and location data. The developed models — especially **SVR** and **XGBoost + Ridge ensemble** — provide strong baselines for future enhancements.

### Future Enhancements

* **Time-series modeling** for date-aware pricing
* **External data integration** (seasonality, local events, reviews)
* **API deployment** for real-time price recommendations
* **A/B testing framework** for model validation
