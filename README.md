
# Analysis of Airbnb Listing Prices and Predictive Modeling

## Introduction

This document details an analysis conducted on Airbnb listing data. The primary objectives were twofold: 
1. To identify and understand the key factors that influence the pricing of accommodations.
2. To develop and evaluate predictive models capable of estimating the logarithm of the listing price (`log_price`).

The methodology encompasses data cleansing, exploratory data analysis (EDA), feature engineering, construction of multiple regression models, and a comparative assessment of their performance.

## Dataset Description

The analysis utilizes the `Airbnb_Data.csv` dataset, which comprises diverse attributes pertaining to Airbnb listings. Key feature categories include:

- **Property Characteristics**: property type, room type, accommodates, bathrooms, bedrooms, and beds.
- **Amenities**: a list of provided features converted into an amenity count.
- **Host Information**: limited use due to missing values; some removed in preprocessing.
- **Geospatial Data**: city, neighbourhood, latitude, longitude, zipcode.
- **Booking Parameters**: cancellation policies, cleaning fee, instant bookable.
- **Review Metrics**: number of reviews and review scores.
- **Target Variable**: `log_price` (log-transformed price of listing).

## Required Libraries

```bash
pip install numpy pandas seaborn matplotlib scikit-learn scipy
```

- **Data Manipulation**: `pandas`, `numpy`
- **Visualization**: `matplotlib`, `seaborn`
- **Machine Learning**: `scikit-learn`
- **Statistics**: `scipy`

## Execution Instructions

1. Clone or download this repository.
2. Place the dataset `Airbnb_Data.csv` in the working directory.
3. Open and execute the notebook `ProjectTest1.ipynb` in Jupyter.
4. Follow the cell execution from top to bottom.

## Analytical Procedure

### 1. Data Ingestion
- Load CSV using pandas.
- Explore data with `.shape`, `.columns`, `.head()`, `.describe()`, `.info()`, and null value summaries.

### 2. Data Preprocessing
- **Feature Removal**: Removed less useful or redundant fields like `id`, `description`, and date-based columns.
- **Feature Engineering**: Converted `amenities` to `amenities_count`.
- **Type Conversion**: Mapped booleans and categorical `t/f` to integers.
- **Missing Values**: 
  - Used KNN imputation (BallTree + Haversine) for `neighbourhood` and `zipcode`.
  - Used median or zero imputation for others.

### 3. Exploratory Data Analysis
- **Univariate**: Histograms and boxplots for numerical features; count plots for categorical.
- **Bivariate**:
  - Heatmaps for correlations.
  - Scatter plots and boxplots to explore price relations.



## 📈 Part 2: Statistical Analysis and Hypothesis Testing

This section presents a series of five statistical tasks based on a random sample of 100 Airbnb listings, using formal hypothesis testing and regression analysis to evaluate claims and build predictive models.

---

### 1. Mean and Proportion Tests

**Variables**:
- Mean test on `accommodates`
- Proportion test on binary `cleaning_fee` (mapped to 0/1)

**Mean Test (One-Sample T-Test on `accommodates`)**
- **Claim**: The average number of people a listing accommodates is 3.
- **Hypotheses**:
  - \( H_0: \mu = 3 \)
  - \( H_1: \mu 
e 3 \)
- **Test**: One-sample t-test
- **Result**: p-value = 0.163 → *Fail to reject H₀*
- **95% CI**: (2.88, 3.70)

**Proportion Test (`cleaning_fee`)**
- **Claim**: 50% of listings have a cleaning fee.
- **Hypotheses**:
  - \( H_0: p = 0.5 \)
  - \( H_1: p 
e 0.5 \)
- **Test**: One-proportion z-test
- **Result**: p-value < 0.05 → *Reject H₀*
- **95% CI**: (0.71, 0.87)

**Interpretation**: While the mean number of accommodated guests does not significantly differ from 3, the proportion of listings with cleaning fees is significantly higher than 0.5.

---

### 2. Two-Sample Comparison

**Variables**: `log_price` for "Entire home/apt" vs "Private room"

**Hypotheses**:
- \( H_0: \mu_{	ext{entire}} = \mu_{	ext{private}} \)
- \( H_1: \mu_{	ext{entire}} 
e \mu_{	ext{private}} \)

**Test**: Independent two-sample t-test

**Results**:
- p-value = 0.0000 → *Reject H₀*
- 95% CI for mean difference: (0.4385, 0.8972)

**Interpretation**: Entire home/apartment listings have significantly higher average `log_price` than private rooms.

---

### 3. Correlation Analysis

**Variables**: Correlation between `log_price` and:
- `accommodates`
- `bedrooms`

**Correlation Results**:

- `log_price` vs `accommodates`:
  - \( r = 0.328 \), p = 0.0009, 95% CI: (0.140, 0.492)
- `log_price` vs `bedrooms`:
  - \( r = 0.256 \), p = 0.0101, 95% CI: (0.063, 0.431)

**Interpretation**: Both features show statistically significant, moderate positive correlations with `log_price`.

---

### 4. Simple Linear Regression

**Model**: `log_price ~ accommodates`

**Equation**:
\[
	ext{log\_price} = 4.4749 + 0.1025 	imes 	ext{accommodates}
\]

**Performance**:
- \( R^2 = 0.1074 \)
- MSE = 0.3674

**Interpretation**: The model explains ~10.7% of the variance. Residual plots indicate heteroscedasticity. This is not a strong standalone predictor model.

---

### 5. Multiple Linear Regression Models

| Model | Predictors                               | R²     | Adjusted R² | MSE     |
|-------|------------------------------------------|--------|-------------|---------|
| 1     | accommodates                             | 0.1074 | 0.0983      | 0.3674  |
| 2     | accommodates, bedrooms                   | 0.1199 | 0.1018      | 0.3622  |
| 3     | accommodates, bedrooms, cleaning_fee     | 0.1251 | 0.0977      | 0.3601  |

**Conclusion**:
- Model 3 has the highest R² but lowest Adjusted R², indicating overfitting.
- Model 2 achieves the best trade-off between model complexity and explanatory power.
- Thus, **Model 2 is recommended** for predicting `log_price`.

