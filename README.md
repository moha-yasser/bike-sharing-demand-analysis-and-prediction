# Bike Sharing Demand Analysis & Prediction 🚴‍♂️

## Overview
This project focuses on analyzing and predicting bike rental demand using exploratory data analysis (EDA), feature engineering, and machine learning models.

The goal is to uncover the factors influencing bike rental behavior and build predictive models capable of estimating rental demand accurately.

The project includes:
- Data cleaning and preprocessing
- Exploratory Data Analysis (EDA)
- Feature engineering
- Pattern discovery and insights extraction
- Machine learning modeling and evaluation

---

## Dataset Description
The dataset contains bike rental records along with environmental and temporal features such as:
- Datetime
- Weather conditions
- temp  (temperature in Celsius)
- atemp ("feels like" temperature in Celsius)
- Humidity
- Wind speed
- Season information
- Holiday
- casual (number of non-registered user rentals initiated)
- registered (number of registered user rentals initiated)

### Target Variable
- `count` → Total bike rentals

---

# Exploratory Data Analysis (EDA)

## Target Variable Analysis
The target variable distribution was analyzed to better understand rental behavior.

### Key Findings
- The distribution is positively skewed.
- No significant outliers were detected.
- The median was considered a more reliable measure of central tendency due to skewness.

---

# Feature Engineering

## Datetime Feature Extraction
The `datetime` feature was converted into a proper datetime format, and several new features were extracted:

- `hour`
- `day_of_week`
- `month`
- `year`

These features help capture:
- Hourly demand behavior
- Weekly usage patterns
- Seasonal trends
- Long-term temporal changes

---

## Season and Weather Mapping
The `season` and `weather` features were mapped into descriptive labels to improve interpretability during analysis.

Examples:
- Spring, Summer, Fall, Winter
- Clear, Mist, Rain

This transformation was applied only during exploratory analysis.

---

# Seasonal & Weather Analysis

Grouped analysis was performed using combinations of:
- Season
- Weather conditions

### Key Findings
- Bike demand tends to increase during summer conditions.
- Clear weather generally shows more consistent and reliable high demand patterns.
- Rental behavior is influenced by interactions between season and weather rather than isolated features.

---

# Temperature-Based Analysis

The `atemp` feature was categorized using quantile-based binning into:
- Cold
- Mild
- Warm
- Hot

This helped simplify comparisons between temperature ranges while maintaining balanced category sizes.

### Key Findings
- Higher rental demand was observed during hot and clear weather conditions.
- Summer days with clear weather showed the most representative high-demand behavior.

---

# Day of Week Analysis

Bike rental patterns were analyzed across:
- Days of the week
- Working days vs holidays

### Key Findings
- Some apparent weekday patterns were heavily influenced by seasonal effects.
- This highlighted the importance of considering hidden relationships between features during analysis.

---

# Hourly Demand Analysis

Hourly rental behavior was analyzed for:
- Working days
- Weekends

### Working Days
- Strong peaks appeared during:
  - Morning commute hours
  - Evening commute hours

This suggests that registered users rely heavily on bikes for transportation and commuting.

### Weekends
- Demand was more evenly distributed throughout the day.
- Usage patterns appeared more leisure-oriented compared to working days.

---

# Registered vs Casual Users

## Registered Users
- Show clear commuting patterns
- Strong demand peaks during rush hours

## Casual Users
- Usage is more spread throughout the day
- Higher activity during weekends
- More associated with leisure behavior

### Conclusion
Registered and casual users demonstrate significantly different rental behaviors, highlighting the importance of user segmentation in demand analysis.

---

# Modeling & Preprocessing

## Preprocessing Pipeline

A preprocessing pipeline was created to prepare the data for machine learning models.

### Cyclic Encoding
Applied to:
- `hour`
- `month`

These features are cyclical in nature, so cyclic encoding helps preserve their circular relationships.

### One-Hot Encoding
Applied to:
- `dayofweek`

This allows the model to treat weekdays as categorical features without introducing ordinal relationships.

### Passthrough Features
The following features were passed directly without transformation:
- `year`
- `atemp`
- `workingday`

### Feature Selection
Feature selection was guided by:
- Permutation Importance analysis
- Experimental model evaluation

Selected features consistently showed meaningful contribution to model performance.

---

# Target Transformation

`TransformedTargetRegressor` was used to integrate target transformation directly into the modeling workflow.

This approach simplifies:
- Target transformation
- Inverse transformation
- Pipeline integration

while maintaining a cleaner and more reliable training process.

---

# Cross Validation Strategy

`GridSearchCV` was used to search for the best hyperparameter combinations and optimize model performance.

Since the dataset contains time-dependent observations, `TimeSeriesSplit` was used instead of standard random cross-validation.

### Why TimeSeriesSplit?
Time-dependent datasets require preserving chronological order to avoid data leakage.

`TimeSeriesSplit`:
- Trains on past observations
- Validates on future observations

This provides:
- More realistic validation results
- Better generalization assessment
- Reduced risk of overly optimistic performance estimates

---

# Feature Importance

Permutation Importance was used to evaluate feature contributions to model performance.

The process works by:
- Randomly shuffling a feature
- Measuring the resulting performance drop

Features causing larger performance decreases are considered more important.

This helped support:
- Feature selection
- Model interpretability
- Better understanding of feature influence
