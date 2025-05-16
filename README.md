**Name:** Kunovics Dávid Zoltán  
**Neptun Code:** SFCJ7H  
**GitHub Repository:** [ohhboi427/itds_end_term](https://github.com/ohhboi427/itds_end_term)

---

## 1. Univariate Regression on Analytical Functions

### Dataset

- Three mathematical functions evaluated:
    1. **f₁(x) = x·sin(x) + 2x**
    2. **f₂(x) = 10·sin(x) + x²**
    3. **f₃(x) = sign(x)·(x² + 300) + 20·sin(x)**
- Sampled at 100 points in interval [-20, 20]
- Random 70/30 train-test split

### Model Performance

#### f₁(x) Results

| Model         | R²    | Notes                           |
|---------------|-------|---------------------------------|
| Linear/Ridge  | ~0.89 | Moderate fit, linear assumption |
| SVR (RBF)     | 1.00  | Perfect nonlinear fit           |
| Random Forest | 0.99  | Excellent performance           |
| MLP Regressor | ~0.89 | Underfitting/not converging     |

#### f₂(x) Results

| Model         | R²    | Notes                     |
|---------------|-------|---------------------------|
| Linear/Ridge  | -0.02 | Fails on sin+x² structure |
| SVR (RBF)     | 1.00  | Ideal fit                 |
| Random Forest | 1.00  | Handles complexity well   |
| MLP Regressor | 0.98  | Good but suboptimal       |

#### f₃(x) Results

| Model         | R²    | Notes                   |
|---------------|-------|-------------------------|
| Linear/Ridge  | ~0.93 | Works surprisingly well |
| SVR (RBF)     | 0.98  | Strong performance      |
| Random Forest | 1.00  | Best overall            |
| MLP Regressor | 0.99  | Nearly perfect          |

### Feature Engineering

- **Trigonometric Features:** sin(kx), cos(kx) up to degree k
- **Polynomial Features:** Standard polynomial terms

| Function | Feature Type  | Best R² | Notes                   |
|----------|---------------|---------|-------------------------|
| f₁       | Trigonometric | 0.99    | Matches periodic nature |
| f₂       | Polynomial    | 0.99    | Captures quadratic term |
| f₃       | Both          | 1.00    | Handles all components  |

---

## 2. Synthetic Regression Dataset Evaluation

### Dataset Types

1. **Standard:** 20 informative features, no noise
2. **Hard:** 15 informative + 5 redundant features, noise=50

### Model Comparison

#### Standard Dataset

| Model         | MSE      | R²   | Notes                      |
|---------------|----------|------|----------------------------|
| Linear        | 0.00     | 1.00 | Perfect fit                |
| Ridge         | 0.04     | 1.00 | Nearly identical           |
| SVR (RBF)     | 16520.90 | 0.77 | Struggles with linear data |
| Random Forest | 22729.41 | 0.69 | Poor generalization        |

#### Hard Dataset

| Model         | MSE      | R²   | Notes                   |
|---------------|----------|------|-------------------------|
| Linear        | 0.00     | 1.00 | Ignores noise perfectly |
| Ridge         | 0.03     | 1.00 | Robust to noise         |
| SVR (RBF)     | 10085.78 | 0.78 | Slightly better than RF |
| Random Forest | 13942.38 | 0.70 | Affected by noise       |

**Redundant Feature Analysis:**
Linear Regression coefficients for last 5 features:

```
[ 6.66e-14 9.91e-14 -2.56e-14 1.84e-14 -5.73e-14 ]
```

Correctly ignored redundant features.

---

## 3. Weather Time Series Modeling

### Data Preparation

- Source: `SummaryofWeather.csv`
- Selected Station 22508
- Extracted `MeanTemp`, dropped missing values
- 7-day rolling windows → predict temperature 2 days ahead
- Train: pre-1945, Test: 1945

### Model Results

| Model             | R²     | MSE    | Notes                 |
|-------------------|--------|--------|-----------------------|
| Linear Regression | 0.5977 | 0.9320 | Best performance      |
| Random Forest     | 0.5204 | 1.1111 | Improved after tuning |
| MLP Regressor     | 0.5486 | 1.0457 | Convergence warnings  |

**Tuned Random Forest:**

- Parameters: n_estimators, max_depth, min_samples_split
- Improved to R²=0.5707, MSE=0.9945

### Visualization

- Linear model captured trends but missed short-term fluctuations
- Random Forest showed better pattern recognition after tuning

---

## Key Findings

1. **Nonlinear models** excel for complex analytical functions
2. **Linear models** dominate truly linear datasets
3. **Feature engineering** crucial for periodic functions
4. **Simple models** can outperform complex ones on time series
5. **Hyperparameter tuning** essential for tree-based methods