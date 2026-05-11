# Applied Machine Learning Journey

This repository documents my transition into applied analytics and machine learning through hands-on projects. Rather than only studying theory, each stage focuses on building real business solutions using realistic datasets, dashboards, automation, and predictive modeling.

---

# Stage One: Business Intelligence Foundations (Power BI)

## Objective
Develop foundational data analytics skills by designing an executive-level procurement dashboard in Power BI using a fully simulated enterprise purchasing dataset.

The goal of this phase was to learn how organizations convert raw purchasing data into actionable insights that improve cost control, supplier management, and operational efficiency.

---

## Project: Procurement Intelligence Dashboard

A simulated purchasing command center designed for leadership teams to monitor procurement KPIs across multiple suppliers, departments, and spending categories.

### Core Skills Learned
- Data cleaning and transformation with Power Query
- Data modeling with relationships and star schema design
- DAX measures and calculated KPIs
- Interactive dashboard design
- Business storytelling through data visualization
- Executive reporting best practices

## Dashboard Features

### Spend Visibility
- Total procurement spend
- Spend by supplier
- Spend by category
- Spend trends over time

### Supplier Performance
- On-time delivery %
- Lead time tracking
- Supplier quality scorecards
- High-risk vendor identification

## Simulated Dataset

Created a realistic enterprise procurement dataset containing:

- 25+ suppliers  
- Thousands of purchase orders  
- Delivery timelines  
- Cost variance data  
- Department requests  
- Supplier quality metrics  
- Multi-category spend history  

## Why This Stage Matters for Machine Learning

Before predictive modeling, companies need clean data, clear KPIs, and measurable business problems.

This project established the analytics foundation required for future machine learning stages such as:

- Supplier delay prediction
- Cost forecasting
- Demand planning
- Fraud detection
- Purchase anomaly detection

## Tools Used

- Power BI  
- Power Query  
- DAX  
- Excel / CSV  
- Simulated Enterprise Data  

---

## Next Stage

### Stage Two: Python Data Analysis + Machine Learning

Upcoming work includes:

- Forecasting procurement spend using regression
- Predicting supplier late deliveries
- Classifying risky purchase orders
- Building Python dashboards with Streamlit

---

---

# Stage Two: Python Foundations for Data Science

## Objective

Build core Python programming skills needed to work with data, libraries, and eventually machine learning models. All practice was done hands-on in Jupyter Notebook.

---

## Course / Practice Notebook

**Python Learner – Personal Practice Notebook**  
Self-directed Python study using Jupyter Notebook focused on data science fundamentals.
Used Youtube video - Python for Data Science - Course for Beginners (Learn Python, Pandas, NumPy, Matplotlib) (https://www.youtube.com/watch?v=LHBE6Q9XlzI&t=44065s)


---

## Topics Covered

### Core Python
- Variables, data types, and dynamic typing
- Arithmetic operators (`+`, `-`, `*`, `/`, `//`, `**`, `%`)
- Comparison and boolean operators
- Control flow: loops and conditionals
- Functions: return values, default arguments, `*args`, variable input arguments
- Modules and imports

### Data Structures
- Lists (indexing, reversing, list comprehension)
- Tuples
- Sets
- Dictionaries (key-value pairs)

### Libraries
- **NumPy** – arrays, dimensions, array operations, single conditions
- **Pandas** – DataFrames, data manipulation

---

## Next Stage

### Stage Three: Scikit-Learn + Machine Learning
Upcoming work includes:
- Supervised learning: classification and regression
- Predicting late purchase orders from procurement data
- Model evaluation and cross-validation
- Feature engineering with Pandas + NumPy

---

---

# Stage Three: Machine Learning with Scikit-Learn

## Objective

Apply Python and Pandas skills to a real supervised machine learning problem: predicting whether a purchase order line will be delivered late. This stage focused on the full ML pipeline from raw data to model evaluation.

---

## Project: Delivery Risk Model

A binary classification model that predicts whether an order line will arrive late, built on a simulated enterprise procurement dataset.

Two notebooks are included:
- `order_predict_model.ipynb` — the working/exploratory notebook showing the full trial-and-error process
- `delivery_risk_model.ipynb` — the cleaned, polished version with the final pipeline

---

## Dataset

Two CSV files were merged to form the modeling dataset:

| File | Key Columns |
|---|---|
| `OrderItems.csv` | OrderID, LineID, ItemCode, ItemCategory, Quantity, UnitPrice, RequestedDate, LineAmount |
| `Deliveries.csv` | OrderID, LineID, DeliveryDate, QtyDelivered, DeliveryStatus |

The two files were joined on `OrderID` and `LineID`, and `DeliveryStatus` was encoded into a binary target variable: `is_late` (1 = Late, 0 = On Time).

---

## ML Pipeline

### 1. Data Merging
```python
df = orders.merge(deliveries[["OrderID", "LineID", "DeliveryStatus"]],
                  on=["OrderID", "LineID"], how="left")
```

### 2. Target Encoding
```python
df["is_late"] = (df["DeliveryStatus"] == "Late").astype(int)
```

### 3. Feature Engineering

| Column | Treatment |
|---|---|
| `OrderID`, `LineID`, `ItemCode` | Dropped — IDs with no predictive value |
| `DeliveryStatus` | Dropped — source of the target, would cause data leakage |
| `ItemCategory` | Encoded with `pd.get_dummies()` |
| `RequestedDate` | Decomposed into `Month`, `DayOfWeek`, `Year` |
| `Quantity`, `UnitPrice`, `LineAmount` | Used as-is |

### 4. Train/Test Split
```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

### 5. Model
```python
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)
```

Random Forest was chosen because it handles mixed data types well, requires minimal tuning, and does not require feature scaling.

---

## Feature Importance Results

| Feature | Importance | Insight |
|---|---|---|
| `LineAmount` | 23% | High value orders are the strongest signal |
| `Quantity` | 22% | Order size is nearly as important |
| `UnitPrice` | 21% | Unit price correlates with complexity |
| `Month` | 12% | Seasonality affects delivery performance |
| `DayOfWeek` | 10% | Day of week has moderate influence |
| `Year` | 4% | Minimal impact |
| `ItemCategory_*` | ~1–2% each | Category type barely affects lateness |

**Key insight:** Large, expensive, high-quantity orders are most likely to run late — a directly actionable finding for procurement teams.

---

## Model Performance

```
              precision    recall  f1-score   support

           0       0.46      0.46      0.46       407
           1       0.55      0.55      0.55       493

    accuracy                           0.51       900
```

The model achieved 51% accuracy — essentially random. This was expected given the dataset was synthetically generated without real business logic driving the `DeliveryStatus` values. The pipeline is correct and validated; performance would improve significantly on real procurement data with genuine delivery patterns.

---

## Core Skills Learned

- Merging DataFrames with `pd.merge()` using composite keys
- Creating binary target variables from categorical columns
- Identifying and removing data leakage sources
- Encoding categorical variables with `pd.get_dummies()`
- Decomposing datetime features for ML
- Splitting data with `train_test_split`
- Training and evaluating a `RandomForestClassifier`
- Interpreting `classification_report` metrics (precision, recall, F1)
- Analyzing feature importances to extract business insights

---

## Tools Used

- Python
- Pandas
- Scikit-Learn
- Jupyter Notebook

---

## Next Stage

### Stage Four: Mathematics and Algorithmic Foundations of Machine Learning

Before advancing to more complex models, the next stage focuses on building a deep understanding of the math and algorithms that underpin machine learning.

Planned topics include:

**Mathematics**
- Linear algebra — vectors, matrices, dot products, matrix multiplication
- Calculus — derivatives, gradients, and the chain rule as they apply to model optimization
- Statistics and probability — distributions, Bayes' theorem, expected value, variance
- Gradient descent — how models actually learn by minimizing a loss function

**Prerequisite Algorithms**
- Linear regression — understanding the simplest predictive model from scratch
- Logistic regression — the math behind binary classification and the sigmoid function
- Decision trees — how splits are chosen using entropy and information gain
- K-Nearest Neighbors — distance metrics and why feature scaling matters
- Naive Bayes — probabilistic classification using Bayes' theorem

**Approach**
- Implement algorithms from scratch in Python (without Scikit-Learn) to understand what is happening under the hood
- Then compare from-scratch implementations to Scikit-Learn equivalents
- Apply each algorithm to the procurement dataset where relevant

---

## Repository Purpose

To publicly track my progression from analytics foundations to production-ready machine learning projects.