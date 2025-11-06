# ⚡️ PySpark Time Series Forecasting Project

## 🚀 Overview

This project leverages **Apache Spark** and **PySpark** for **distributed time series forecasting**. The goal is to forecast hourly energy consumption based on historical data using techniques such as **feature engineering**, **lag features**, and **gradient boosting**.

By utilizing Spark's distributed capabilities, we aim to scale up traditionally CPU-bound tasks like data transformation and model training, making this approach suitable for large datasets.

### Reference Methodology

The data cleaning and feature engineering (especially Lag Features) were inspired by the [Kaggle Notebook: PT2 - Time Series Forecasting with XGBoost](https://www.kaggle.com/code/robikscube/pt2-time-series-forecasting-with-xgboost/notebook), which applies XGBoost for energy consumption forecasting. This implementation uses PySpark for distributed processing.

## 🛠️ Requirements and Setup

### Prerequisites

1. **Java Development Kit (JDK)**: Version 11 or 17 (required for PySpark to work). Make sure `JAVA_HOME` is correctly set.
2. **Python**: Version 3.8 or higher.

### Installation

To install the required libraries, run:

```bash
pip install pyspark pandas scikit-learn
```

If you'd like to use the distributed **XGBoost** model (as an alternative to the native GBTRegressor), install the optimized version:

```bash
pip install xgboost-spark
```

## 📈 Methodology

The project follows a **5-step process** to process and model time series data using **PySpark**:

1. **Data Ingestion & Cleaning**: Load the dataset, cast the necessary columns (e.g., `Timestamp`, `Double`), and remove outliers (energy consumption < 19,000 MW).
2. **Time-Based Feature Engineering**: Extract time-related features like `hour`, `dayofweek`, `month`, and `year` from the `Datetime` column.
3. **Advanced Feature Engineering**: Create lag features (e.g., energy consumption from 364, 728, and 1092 days prior) using **PySpark Window Functions** (`Window.orderBy("Datetime")` and `lag()`).
4. **Data Preparation**: Use `VectorAssembler` to transform features into a format that can be processed by **MLlib**.
5. **Modeling**: Train a **Gradient-Boosted Tree (GBT)** model and validate its performance using **temporal cross-validation**.

## 📊 Results and Validation

The model's performance was evaluated using **3-Fold Temporal Cross-Validation**, where each fold corresponds to testing on different years (2015, 2016, and 2017).

### Model Performance (RMSE)

Here are the results from the **3-fold cross-validation**:

| Fold | Testing Period (Year) | Training Data Size | RMSE (Root Mean Squared Error) |
| :--- | :---                  | :---               | :---                            |
| **Fold 1** | 2015 | 87,703 rows | 3,713.54 MW |
| **Fold 2** | 2016 | 96,463 rows | 3,967.38 MW |
| **Fold 3** | 2017 | 105,247 rows | 4,247.95 MW |

**Overall Average RMSE: 3,976.29 MW**

### Conclusion

The model achieved a robust performance with an overall RMSE of **3,976.29 MW**, which is very close to the performance of the XGBoost model in the original Kaggle notebook (~3,700 MW). This indicates that the **PySpark implementation** for distributed time series forecasting with **Lag Features** is efficient and provides good results.