# Photocatalysis: Ridge and XGBoost Comparison

**Author: Shahid Ali**

Independent computational reanalysis of published experimental data on malachite green removal using bismuth ferrite-based photocatalysts.

## Objective

Compare Ridge regression and XGBoost for predicting degradation efficiency, with attention to grouped validation, overfitting, and duplicate-record sensitivity.

## Data source and credit

The experimental data were produced by the original researchers, not by me.

Jaffari et al., “Machine learning approaches to predict the photocatalytic performance of bismuth ferrite-based materials in the removal of malachite green.”

- Paper: https://doi.org/10.1016/j.jhazmat.2022.130031
- Original data and code: https://github.com/ZeeshanHJ/Photocatalytic_Performance_Prediction

Download `Raw data.csv` from the original repository. The raw dataset is not redistributed here.

## Methods

- Dataset: 1,200 observations.
- Target: degradation efficiency (%).
- Final concentration was excluded from predictors to prevent target leakage.
- Categorical features were one-hot encoded within each training pipeline.
- Measurements with matching recorded experimental conditions, excluding time, were assigned to the same group.
- A held-out partition contained 8 condition groups; the training partition contained 31 groups.
- Nested validation used five outer folds and up to four inner grouped folds on the training partition.
- Ridge and XGBoost were compared with mean and median baselines.
- Exact duplicate records were retained in the primary analysis and removed in a sensitivity analysis.

## Results

Nested grouped validation on the training partition:

| Duplicate handling | Model | Pooled R² | MAE | RMSE |
|---|---|---:|---:|---:|
| Retained | Ridge | 0.755 | 11.65 | 15.85 |
| Retained | XGBoost | 0.640 | 13.10 | 19.22 |
| Removed | Ridge | 0.711 | 11.79 | 15.65 |
| Removed | XGBoost | 0.600 | 13.47 | 18.44 |

MAE and RMSE are measured in efficiency percentage points. The two duplicate scenarios contain different evaluation-row counts; see `Duplicate_Sensitivity_Common_Rows.csv` for comparison on identical rows.

XGBoost achieved R² = 0.632, MAE = 16.32, and RMSE = 19.12 on the held-out partition. A final held-out Ridge evaluation has not yet been completed.

**Main finding:** Ridge outperformed XGBoost under the nested grouped validation design.

![Nested model comparison](Nested_Model_Comparison.png)

## Repository contents

- `Shahid_Ali_Photocatalysis_ML_Comparison.ipynb`: modelling, diagnostics, and nested validation.
- `Nested_Summary.csv`: pooled and fold-level summary statistics.
- `Nested_Fold_Metrics.csv`: individual outer-fold evaluation metrics.
- `Duplicate_Sensitivity_Common_Rows.csv`: duplicate sensitivity on matching evaluation rows.
- `Nested_Model_Comparison.png`: comparison of nested validation MAE.

## Running the notebook

1. Install Python packages: pandas, numpy, matplotlib, scipy, scikit-learn, xgboost, shap, and joblib.
2. Download the original `Raw data.csv`.
3. Open the notebook in VS Code with Jupyter support.
4. Change `PATH` to your dataset location and `OUT` to your preferred output directory.
5. Run the first cell, followed by the nested-validation cell.

The saved run used `D:\Raw data.csv` and wrote results to `D:\BFO_XGBoost_Results`. The first cell generates 25 figures in PNG and PDF formats.

## Limitations

- Condition groups are proxies, not verified experiment identifiers.
- Strong XGBoost training performance did not translate into equally strong validation performance.
- XGBoost performed poorly on the no-catalyst control.
- The first cell’s ordinary out-of-fold diagnostics reuse tuned parameters; the second cell performs nested tuning.
- Dataset inspection informed the analysis, and the held-out results have already been inspected.
- Results describe internal validation on this dataset; external validation remains outstanding.

## Status

Exploratory research portfolio project. Further work includes a final Ridge test comparison and external validation.

## Graphs

## XGBoost Diagnostic and Interpretation Gallery

The following figures describe the XGBoost analysis. Ridge comparisons are reported in the results above. Training performance should be interpreted alongside validation and held-out testing.

### 1. Training predictions

![XGBoost training parity](figures/01_Training_Parity.png)

### 2. Out-of-fold predictions

These predictions use the selected hyperparameters and are diagnostic estimates, separate from the nested validation reported above.

![Training out-of-fold parity](figures/02_Training_OOF_Parity.png)

### 3. Held-out testing predictions

![Testing parity](figures/03_Testing_Parity.png)

### 4. Training and testing comparison

![Training and testing comparison](figures/04_Training_Testing_Comparison.png)

### 5. Residuals versus predictions

![Residuals versus predictions](figures/05_Residual_Versus_Prediction.png)

### 6. Residuals versus actual efficiency

![Residuals versus actual efficiency](figures/06_Residual_Versus_Actual.png)

### 7. Residual distribution

![Residual histogram](figures/07_Residual_Histogram.png)

### 8. Residual Q-Q plot

![Residual Q-Q plot](figures/08_Residual_QQ_Plot.png)

### 9. Sorted test predictions

Observations are sorted by actual efficiency, not by reaction time.

![Sorted testing predictions](figures/09_Sorted_Testing_Predictions.png)

### 10. Absolute prediction errors

![Absolute error versus actual efficiency](figures/10_Absolute_Error_Versus_Actual.png)

### 11. Cumulative error distribution

![Absolute error cumulative distribution](figures/11_Absolute_Error_ECDF.png)

### 12. Actual and predicted distributions

![Testing target distributions](figures/12_Testing_Target_Distributions.png)

### 13. Error distribution comparison

![Error distribution comparison](figures/13_Error_Distribution_Comparison.png)

### 14. Residual scale

![Residual scale plot](figures/14_Residual_Scale_Plot.png)

### 15. Errors across experimental features

![Numeric feature residuals](figures/15_Numeric_Feature_Residuals.png)

### 16. Training-data correlations

Correlations describe associations within the training data, not causal relationships.

![Training correlation matrix](figures/16_Training_Correlation_Matrix.png)

### 17. Training and testing feature distributions

![Numeric feature distributions](figures/17_Numeric_Feature_Distributions.png)

### 18. Cross-validated permutation importance

Error bars show variation across folds. These diagnostics reuse the selected hyperparameters.

![Cross-validated permutation importance](figures/18_Cross_Validated_Permutation_Importance.png)

### 19. XGBoost gain importance

![XGBoost gain importance](figures/19_XGBoost_Gain_Importance.png)

### 20. Grouped learning curve

![Grouped learning curve](figures/20_Grouped_Learning_Curve.png)

### 21. Validation-fold stability

![Validation fold stability](figures/21_Validation_Fold_Stability.png)

### 22. SHAP feature contributions

SHAP values explain the fitted model's predictions; they do not establish chemical mechanisms.

![Training SHAP beeswarm](figures/22_Training_SHAP_Beeswarm.png)

### 23. Mean absolute SHAP importance

![Training SHAP importance](figures/23_Training_SHAP_Importance.png)

### 24. Partial dependence

These curves describe the fitted model's average response to individual features and should be interpreted cautiously where experimental coverage is sparse.

![Training numeric partial dependence](figures/24_Training_Numeric_Partial_Dependence.png)

### 25. Training, validation, and testing dashboard

![Training validation testing dashboard](figures/25_Training_Validation_Testing_Dashboard.png)
