# Miami Housing Price Prediction

**Academic project | M.S. in Data Analytics | DA 523, Spring 2024**  
**Author:** Andreza Eufrasio

---
![Miami Housing Market](image/miami.png)

## Project overview

This project investigates housing sale prices in a dataset of **13,932 Miami single-family property sales**. It compares multiple linear regression and K-nearest neighbors (KNN) regression to predict sale prices from recorded property characteristics. The GitHub notebook is a revised, portfolio-oriented version of the original Spring 2024 course analysis; the research question and model families are retained, while parts of preprocessing and model selection have been updated.

---

## Questions explored

 * **What patterns can we identify in Miami housing prices and property-characteristics?**
 * **How do Linear Regression and KNN compare in predicting housing sale prices?**
 * **What are the limitations of the models, and how well might they perform on new data?**

---

## Dataset

The **Miami Housing Dataset**, originally obtained from Kaggle for the course project, contains **13,932 rows and 17 original columns**. The outcome is `SALE_PRC` (sale price). Candidate predictors describe living and land area, building age, structural quality, geographic location, and distances to amenities. `PARCELNO` is an identifier and is excluded from modeling.

The notebook reads `miami-housing.csv` from the same directory as the notebook.This is a historical dataset, **not** a live view of Miami's housing market.

---

## Analysis and methods

The [Jupyter notebook](miami_housing_price_prediction_complete.ipynb) documents the reasoning, code, visualizations, and interpretation for each stage:

1. **Data inspection:** Review dimensions, data types, summary statistics, missing values, and duplicated rows.
2. **Exploratory analysis:** Examine sale-price and selected predictor distributions, plus descriptive correlations.
3. **Target transformation:** Model `ln(SALE_PRC)` because the original sale-price distribution is right-skewed. The transformation compresses high values; it does not guarantee normally distributed residuals or better accuracy.
4. **Preprocessing:** Exclude the target and property identifier, encode `month_sold` and `structure_quality` as dummy variables, and split the data randomly into **75% training / 25% validation** with a fixed random seed.
5. **Linear regression:** Fit a full-feature baseline and a reduced-feature model selected by forward sequential selection with five-fold cross-validation on the training data.
6. **KNN regression:** Standardize features inside a pipeline and select the number of neighbors (`k`, tested from 1 through 15) using five-fold cross-validation on the training data.
7. **Evaluation:** Compare the models using the **same validation dataset** evaluate their performance using R², RMSE, and MAE. Visualize actual versus predicted sale prices.

Feature selection and KNN hyperparameter tuning were performed using only the training data. The validation dataset was reserved for the final evaluation of model performance using R², RMSE, and MAE. This approach helps assess how well the models predict sale prices for properties not used during model development.

The analysis identifies relationships between property characteristics and sale prices that are useful for prediction. However, these relationships do not establish that changing a particular property characteristic would directly cause a change in sale price.

---

## Results from the revised notebook

The revised notebook reports the following results for the held-out validation set:

| Model | R² (log target) | RMSE (log units) | MAE (log units) |
| --- | ---: | ---: | ---: |
| Linear regression (all features) | 0.788 | 0.260 | 0.190 |
| Linear regression (selected features) | 0.787 | 0.261 | 0.191 |
| KNN (scaled, tuned; k = 3) | 0.825 | 0.237 | 0.165 |


**Key findings:** 
* In this revised experiment, scaled KNN has lower validation errors and higher R² than either linear regression specification.
* Feature selection did not improve the linear model's held-out accuracy relative to its full-feature baseline.
* These findings apply to this dataset and random split; they do not establish performance on future or external sales.

**Metric interpretation:** All three metrics above evaluate predictions of the **natural logarithm of sale price**. In particular, MAE and RMSE are in log-price units, **not dollars or percentages**. The notebook exponentiates predictions for the actual-versus-predicted dollar-price plots; it does not report dollar-scale error metrics.

### Original coursework versus revised analysis

The original Spring 2024 course report recorded **R² = 0.81, RMSE = 0.25, MAE = 0.18** for linear regression and **R² = 0.86, RMSE = 0.21, MAE = 0.15** for KNN (`k = 4`). Those values describe the **original submission**, not the revised notebook. The original report discussed correlation-guided and AIC/BIC feature selection; the revised notebook instead uses training-only cross-validation for selection and a consistently scaled KNN pipeline. The two sets of results should not be treated as the same experiment.

---

## Repository files

```text
miami-housing-price-prediction/
├── README.md
├── miami_housing_price_prediction_complete.ipynb
├── requirements.txt
├── miami-housing.csv                # Add only if redistribution is permitted
└── docs/
    └── original_course_report.pdf  # Optional: original Spring 2024 submission
```

The optional original report is supporting documentation, not a report of the revised notebook's results. Remove optional entries above if they are not included in your repository.

---

## How to Run the Notebook

1. Clone or download this repository.
2. Place the  `miami-housing.csv` file in the same directory as `miami_housing_price_prediction_complete.ipynb`.
4. Install the required Python packages using `requirements.txt`.

```bash
 pip install -r requirements.txt
```

5. Start Jupyter Notebook:
   
```bash
 jupyter notebook
```
   
7. Open and run
miami_housing_price_prediction_complete.ipynb

The notebook contains the complete analysis, visualizations, answers to questions, and supporting interpretations.

---

## Limitations and future work

The models were evaluated using a randomly selected validation set from the original Miami housing dataset. Their predictive performance on future housing sales or properties in other geographic areas has not been tested. Additionally, the results identify relationships between property characteristics and sale prices but do not establish cause-and-effect relationships.

Future work could evaluate the models using more recent housing data or properties from other geographic areas to assess their ability to generalize to new data. Additional regression models, such as Random Forest and Gradient Boosting, could also be explored to determine whether they improve predictive performance, as measured by R², RMSE, and MAE.
