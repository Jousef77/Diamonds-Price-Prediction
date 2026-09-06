# Diamonds Price Prediction

A machine learning project that explores the classic **diamonds dataset** and builds
regression models to predict diamond prices based on their physical and quality attributes.

## Dataset

The dataset (`diamonds.xlsx`) contains **53,794 records** with **14 features**:

| Column | Description |
|---|---|
| Carat | Weight of the diamond |
| Cut / Cut Rank | Quality of the cut (Fair → Ideal) and its numeric rank |
| Color / Color Rank | Diamond color grade (J → D) and its numeric rank |
| Clarity / Clarity Rank | Clarity grade (I1 → IF) and its numeric rank |
| Depth | Total depth percentage |
| Table | Width of the top of the diamond relative to its widest point |
| Length (x), Width (y), Height (z) | Physical dimensions in mm |
| Volume_mm3 | Engineered feature: x × y × z |
| **Price** | Price in US dollars (target variable) |

No missing values or duplicate rows were found. A small number of rows (≤19) had zero
dimensions and were flagged during EDA.

## Project Structure

```
Diamonds-Price-Prediction/
├── diamonds.xlsx                              # Dataset
├── eda_analysis.ipynb                         # Exploratory Data Analysis
├── Linear_Regression.ipynb                    # Simple linear regression (Carat only)
├── Multiple_Regression.ipynb                  # Multiple regression (with x, y, z)
├── Multiple_RegressionWithOut(x,y,z).ipynb     # Multiple regression (with Volume_mm3 instead)
└── Polynomial_Regression.ipynb                 # Polynomial regression (degrees 1–5)
```

## Exploratory Data Analysis

The EDA notebook covers:
- Data quality checks (nulls, duplicates, invalid zero values)
- Distribution plots for Price, Carat, and Volume
- Relationship plots between Price and key features
- Outlier detection via boxplots
- Average price breakdown by Cut, Color, and Clarity
- Correlation analysis and heatmap

**Key insight:** `Carat`, `Volume_mm3`, and the physical dimensions (x, y, z) are the
strongest predictors of price (correlation > 0.85), while `Depth` and `Table` show
little to no linear relationship with price.

## Models & Results

All models use an 80/20 train-test split (`random_state=42`) with a `StandardScaler` →
`LinearRegression` pipeline.

| Model | Features | MSE | R² Score |
|---|---|---|---|
| Linear Regression | Carat only | 2,317,072 | 0.848 |
| Multiple Regression | Carat, Volume_mm3, Cut/Color/Clarity Rank, Depth, Table, x, y, z | 1,400,333 | 0.908 |
| Multiple Regression (no x,y,z) | Carat, Volume_mm3, Cut/Color/Clarity Rank, Depth, Table | 1,448,488 | 0.905 |
| Polynomial Regression (degree 3) | Same as above (no x,y,z) | 366,392 | **0.961** |

### Polynomial degree comparison

| Degree | Validation MSE |
|---|---|
| 1 | 1,448,487 |
| 2 | 762,880 |
| 3 | **592,743** |
| 4 | 457,591,955 (overfitting) |
| 5 | 1,174,143,654,476 (severe overfitting) |

**Conclusion:** Replacing raw dimensions (x, y, z) with the engineered `Volume_mm3`
feature causes only a minor drop in performance, meaning it captures most of the same
information. The polynomial regression model (degree 3) significantly outperforms all
linear approaches, best capturing the non-linear relationship between diamond features
and price.

## Tech Stack

- Python
- pandas
- matplotlib / seaborn
- scikit-learn (Pipeline, StandardScaler, LinearRegression, PolynomialFeatures)

## Getting Started

```bash
pip install pandas matplotlib seaborn scikit-learn openpyxl
jupyter notebook
```

Run the notebooks in this order for the full workflow:
1. `eda_analysis.ipynb`
2. `Linear_Regression.ipynb`
3. `Multiple_Regression.ipynb`
4. `Multiple_RegressionWithOut(x,y,z).ipynb`
5. `Polynomial_Regression.ipynb`
