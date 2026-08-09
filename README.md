# 🚗 End-to-End Used Car Price Prediction Using Machine Learning

## 📌 Project Overview

This project develops an end-to-end Machine Learning system to predict the selling price of used cars based on various vehicle characteristics.

The project covers the complete machine learning workflow, including:

- Data Understanding
- Data Cleaning
- Feature Engineering
- Exploratory Data Analysis (EDA)
- Categorical Encoding
- Train-Test Splitting
- Regression Model Training
- Model Comparison
- Model Evaluation
- Hyperparameter Tuning
- Final Price Prediction
- Model Saving


The final model selected for this project is a **Tuned XGBoost Regressor**, which achieved an **R² score of approximately 93.55%** on the test dataset.

---

## 🎯 Project Objective

The main objective of this project is to build a Machine Learning model that can estimate the price of a used car based on information such as:

- Brand
- Location
- Car Age
- Kilometers Driven
- Fuel Type
- Transmission
- Owner Type
- Mileage
- Engine Capacity
- Power
- Number of Seats

The trained model can be integrated into a web application where users enter car details and receive an estimated used-car price.

---

## 📊 Dataset

The dataset contains information about used cars, including their specifications and selling prices.

After data cleaning and preprocessing, the final dataset contained:

- **6,015 records**
- **11 input features**
- **1 target variable — Price**

### Final Features

| Feature | Description |
|---|---|
| Location | City where the car is located |
| Kilometers_Driven | Total kilometers driven |
| Fuel_Type | Petrol, Diesel, CNG, LPG, Electric, etc. |
| Transmission | Manual or Automatic |
| Owner_Type | First, Second, Third, etc. |
| Mileage | Fuel efficiency |
| Engine | Engine capacity in CC |
| Power | Engine power in BHP |
| Seats | Number of seats |
| Brand | Manufacturer of the car |
| Car_Age | Age of the car |

### Target Variable

**Price** — Selling price of the used car in lakhs.

---

## 🧹 Data Cleaning

Several preprocessing steps were performed before model training.

### Cleaning performed:

- Removed unnecessary index column
- Removed `New_Price` because of a large number of missing values
- Converted `Mileage` from text to numerical format
- Converted `Engine` from values such as `998 CC` to numerical values
- Converted `Power` from values such as `58.16 bhp` to numerical values
- Handled `null bhp` values in Power
- Handled missing values in:
  - Mileage
  - Engine
  - Power
  - Seats
- Investigated invalid zero values
- Examined extreme values in `Kilometers_Driven`
- Removed clearly unrealistic kilometer records

After cleaning, the dataset contained **6,015 records with no missing values**.

---

## 🛠️ Feature Engineering

New features were created to improve the quality of the model.

### Brand

The car manufacturer was extracted from the original car name.

Example:

Maruti Swift VDI → Maruti  
Hyundai i20 Sportz → Hyundai  
BMW X5 → BMW

Different representations of the same brand were also standardized where necessary.

### Car Age

Instead of directly using the manufacturing year, the age of the vehicle was calculated and used as a feature.

This provides the model with a more meaningful representation of vehicle depreciation.

---

## 📈 Exploratory Data Analysis

EDA was performed to understand the relationship between vehicle characteristics and selling price.

Important visualizations included:

1. Price Distribution
2. Median Price by Brand
3. Car Age vs Price
4. Power vs Price
5. Correlation Heatmap

### Key Findings

The target variable `Price` was highly right-skewed.

**Price Skewness:**

3.34

This indicates that most vehicles are concentrated in lower price ranges, while a smaller number of luxury vehicles have much higher prices.

### Correlation with Price

| Feature | Correlation |
|---|---:|
| Power | +0.77 |
| Engine | +0.66 |
| Mileage | -0.33 |
| Car Age | -0.30 |
| Kilometers Driven | -0.18 |
| Seats | +0.05 |

`Power` showed the strongest positive numerical relationship with car price.

Engine and Power were also highly correlated with each other.

---

## 🔄 Data Preprocessing

The dataset was separated into:

- **X** — Input features
- **y** — Car Price

Dataset shape:

X: (6015, 11)

y: (6015,)

### Categorical Features

- Location
- Fuel_Type
- Transmission
- Owner_Type
- Brand

### Numerical Features

- Kilometers_Driven
- Mileage
- Engine
- Power
- Seats
- Car_Age

Categorical variables were transformed using **One-Hot Encoding**.

The preprocessing was fitted only on the training data to reduce the risk of data leakage.

After encoding:

Training Data: (4812, 57)

Testing Data: (1203, 57)

---

## ✂️ Train-Test Split

The dataset was divided into:

- **80% Training Data**
- **20% Testing Data**

This allows model performance to be evaluated on previously unseen data.

---

## 🤖 Machine Learning Models

Five regression algorithms were trained and compared.

1. Linear Regression
2. Decision Tree Regressor
3. Random Forest Regressor
4. Gradient Boosting Regressor
5. XGBoost Regressor

---

## 📊 Baseline Model Comparison

| Model | MAE | RMSE | R² Score |
|---|---:|---:|---:|
| **XGBoost** | **1.3116** | **2.8461** | **0.9298** |
| Random Forest | 1.4059 | 2.9468 | 0.9248 |
| Gradient Boosting | 1.7466 | 3.3620 | 0.9021 |
| Decision Tree | 1.9499 | 4.1802 | 0.8486 |
| Linear Regression | 3.3673 | 5.2145 | 0.7644 |

XGBoost achieved the best baseline performance.

---

## 🏆 XGBoost Model

Baseline XGBoost achieved:

- **R² Score:** 0.9298
- **MAE:** 1.31 lakh
- **RMSE:** 2.85 lakh

### Train-Test Performance

Training R²: 0.9957

Testing R²: 0.9298

R² Gap: 0.0658

The model performed strongly on unseen data, although some train-test performance gap was observed.

---

## ⚙️ Hyperparameter Tuning

XGBoost was further optimized using **RandomizedSearchCV with 5-fold cross-validation**.

### Best Parameters

```python
{
    "subsample": 1.0,
    "n_estimators": 500,
    "max_depth": 7,
    "learning_rate": 0.1,
    "colsample_bytree": 0.7
}
