# 🏢 Apartment value radar

## Overview
Project combining ML and Power BI to analyze 
the Polish residential real estate market. An XGBoost model estimates 
apartment market value. The difference between predicted and actual 
listing price is used to surface undervalued properties.

## Dashboard preview
<img width="1548" height="870" alt="dashboardscreen" src="https://github.com/user-attachments/assets/5b5a0916-9fde-428e-bb73-857d15f14bbb" />



https://github.com/user-attachments/assets/685b6fdf-0a21-4a82-82ce-807cc78e456b

## Objective
Analyze the key price drivers in the Polish housing market and develop a predictive model to identify undervalued properties and find market bargains.
A property is flagged as an opportunity when:

$$\frac{Predicted\ Price - Listing\ Price}{Listing\ Price} > 10\%$$

Since the model's MAPE is 8.31%, this 10% threshold ensures that the identified discount is statistically significant and is not a result of a standard model variance.

## Key findings
**Shap feature importance plot**

<img width="500" alt="apartmentsFI" src="https://github.com/user-attachments/assets/5245ef7a-0f0b-4456-83e8-ecc12d7eb47f" />

Based on exploratory data analysis and SHAP explainability, the model uncovered the absolute hierarchy of price drivers in the Polish apartment market:

- **Living space is the ultimate price driver (squareMeters):** Total area dominates the valuation model. The analysis shows a massive premium for larger apartments, which heavily inflate the total predicted price, while smaller apartments strictly cap the property's potential value.
- **Location dictates the baseline (city):** Using target encoding, the model confirms that geography acts as a massive multiplier. High-demand cities (like Warsaw) naturally elevate the baseline price of any property, while more affordable regional markets drag the relative value down regardless of the apartment's standard.
- **The downtown premium (centreDistance):** Proximity to the central business district commands a clear and significant premium. The SHAP plot reveals that low distances to the center forcefully push prices up, while properties located in the outskirts offer steep, consistent discounts.
- **The new build premium (buildYear):** While secondary to location and size, newer constructions and recent development years provide a slight but consistent boost to the predicted market value compared to older residential blocks.

## Project structure

**Stage 1: Modelling (Jupyter Notebook)**  
Data cleaning and EDA, feature engineering and XGBoost training inside 
a Scikit-Learn Pipeline, hyperparameter tuning with Optuna, model validation and
explainability using SHAP.

**Stage 2: Dashboard (Power BI)**  
Interactive dashboard:
- **Page 1 (Market overview):** contains charts of key metrics: price per sq meter by city, pricing trends by build year, and area vs. price distributions.
- **Page 2 (Opportunity map):** interactive Azure Map with clustering. 
  Clicking a listing shows model price, actual price, and estimated profit

## Model performance
Evaluating real estate prices involves high variance due to unmeasured, subjective factors (e.g., interior standard, view, neighborhood sentiment). The goal was not to build a perfect oracle, but a robust baseline valuation tool that significantly outperforms basic heuristics.

| Metric | XGBoost (Final) | Decision Tree (Baseline) |
|--------|---------|--------------------------------|
| MAE | **63.2K PLN** | 89.0K PLN |
| MAPE | **8.31%** | 11.55% |
| Relative RMSE | **14.11%** | 21.39% |
*Note: The mean apartment price in the dataset is ~768K PLN.*

- **Baseline comparison:** A standard Decision Tree Regressor was evaluated as a baseline. The optimized XGBoost model successfully outperformed it across all metrics, reducing the average estimation error (MAE) by over **25,000 PLN per property**. This demonstrates its superior ability to capture complex, non-linear market dynamics.
- **Business applicability (MAPE):** Achieving a mean absolute percentage error of 8.31% is highly practical for the real estate domain. It provides a tight enough valuation bracket to confidently flag properties listed at a 10%+ discount as genuine market opportunities, rather than mere statistical noise.
- **Addressing outliers (RMSE):** The substantial drop in Relative RMSE (from 21.39% to 14.11%) indicates that the XGBoost model is significantly more robust and makes far fewer extreme prediction errors compared to the baseline.

<img width="1355" height="618" alt="evaluationscreen" src="https://github.com/user-attachments/assets/db0c9760-65e7-4326-a1f6-f492601ba671" />

## Dataset 
[Apartment Prices in Poland](https://www.kaggle.com/datasets/krzysztofjamroz/apartment-prices-in-poland) over 100K apartment records from 15 largest Polish cities
## Tech stack
Pandas, Scikit-Learn, XGBoost, Optuna, SHAP, Power BI, DAX, Azure Maps

## Repository structure
```
├── apartmentNotebook.ipynb    # Jupyter notebook
├── apartmentpricesPbi.pbix    # Power BI report 
```
## How to explore
- Jupyter notebook: open apartmentNotebook.ipynb
- Dashboard: download apartmentpricesPbi.pbix and open in Power BI Desktop 
