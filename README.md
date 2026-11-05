# Photocatalysis: Ridge and XGBoost Comparison

**Author: Shahid Ali**

Machine Learning reanalysis of experimental data on malachite green removal using bismuth ferrite-based photocatalysts.

## Objective

Compare Ridge regression and XGBoost for predicting degradation efficiency, with attention to grouped validation, overfitting, and duplicate-record sensitivity.

## Data source and credit

The original researchers produced the experimental data. My contribution is the computational reanalysis, model comparison, validation, and visualization.

Jaffari et al., “Machine learning approaches to predict the photocatalytic performance of bismuth ferrite-based materials in the removal of malachite green.”

- [Original paper](https://doi.org/10.1016/j.jhazmat.2022.130031)
- [Original dataset and code](https://github.com/ZeeshanHJ/Photocatalytic_Performance_Prediction)

Download `Raw data.csv` from the original repository. The raw dataset is not redistributed here.

## Methods

- Dataset: 1,200 observations.
- Target: degradation efficiency (%).
- Final concentration was excluded from predictors to prevent target leakage.
- Valid zero values, including experimental controls, were preserved.
- Categorical features were one-hot encoded within each training pipeline.
- Measurements with matching recorded experimental conditions, excluding time, were assigned to the same group.
- The training partition contained 960 observations across 31 condition groups.
- The held-out testing partition contained 240 observations across 8 condition groups.
- Nested validation used five outer folds and four inner grouped folds on the training partition.
- Ridge and XGBoost were compared with mean and median baselines.
- Exact duplicate records were retained in the primary analysis and removed in a sensitivity analysis.
- Both duplicate scenarios used the same outer condition-group assignments.

## Results

### Nested grouped validation

The following results were obtained on the training partition using nested grouped cross-validation:

| Duplicate handling | Model | Pooled R² | MAE | RMSE |
|---|---|---:|---:|---:|
| Retained | Ridge | 0.755 | 11.65 | 15.85 |
| Retained | XGBoost | 0.640 | 13.10 | 19.22 |
| Removed | Ridge | 0.711 | 11.79 | 15.65 |
| Removed | XGBoost | 0.600 | 13.47 | 18.44 |

MAE and RMSE are measured in efficiency percentage points.

The duplicate-retained scenario evaluates 960 observations, while the duplicate-removed scenario evaluates 826 observations. For a direct comparison on identical evaluation rows, see `Duplicate_Sensitivity_Common_Rows.csv`.

**Main finding:** Ridge outperformed XGBoost under this nested grouped validation design.

![Nested model comparison](Nested_Model_Comparison.png)

### Held-out XGBoost evaluation

| Metric | Result |
|---|---:|
| R² | 0.632 |
| MAE | 16.32 percentage points |
| RMSE | 19.12 percentage points |
| Mean prediction bias | +7.30 percentage points |
| Mean-baseline MAE | 27.54 percentage points |

A final held-out Ridge evaluation has not yet been completed.

### Error analysis

XGBoost showed substantial errors on the no-catalyst control and some experimental conditions involving 2 wt% Pd-BFO. These failures are retained and reported rather than removed to improve scores.

## Repository contents

- `Shahid_Ali_Photocatalysis_ML_Comparison.ipynb`: modelling, diagnostics, and nested validation.
- `Nested_Summary.csv`: pooled metrics and summaries across validation folds.
- `Nested_Fold_Metrics.csv`: individual outer-fold evaluation metrics.
- `Duplicate_Sensitivity_Common_Rows.csv`: duplicate sensitivity on matching evaluation rows.
- `Nested_Model_Comparison.png`: comparison of nested validation MAE.
- `figures/`: 25 XGBoost diagnostic and interpretation figures.
- `requirements.txt`: Python dependencies.

## Running the notebook

1. Install dependencies with `pip install -r requirements.txt`.
2. Download the original `Raw data.csv` from the authors’ repository.
3. Open `Shahid_Ali_Photocatalysis_ML_Comparison.ipynb` in VS Code with Python and Jupyter support.
4. Change `PATH` to your dataset location and `OUT` to your preferred output directory.
5. Run the first cell, followed by the nested-validation cell.

The saved run used `D:\Raw data.csv` and wrote results to `D:\BFO_XGBoost_Results`.

The first cell generates 25 figures in PNG and PDF formats. The second cell performs nested grouped validation, baseline comparisons, duplicate sensitivity analysis, and error breakdowns.

Dependencies are currently unpinned, so results may vary across package versions.

## Limitations

- The 1,200 observations are not 1,200 independent experiments.
- Condition groups are proxies based on recorded features, not verified experiment identifiers.
- Some held-out groups can contain catalyst categories absent from training.
- Strong XGBoost training performance did not translate into equally strong validation performance.
- XGBoost performed poorly on the no-catalyst control.
- The first cell’s ordinary out-of-fold diagnostics reuse tuned parameters; the second cell performs nested tuning.
- Permutation importance and learning-curve diagnostics also reuse selected hyperparameters.
- Feature importance, SHAP values, and partial dependence describe model behaviour and do not establish causal chemical mechanisms.
- The provenance of exact duplicate records has not been resolved; retaining and removing them are sensitivity scenarios.
- Dataset inspection informed the analysis, and held-out results have already been inspected.
- Results describe internal validation on this dataset; external validation remains outstanding.

## Status

Exploratory research portfolio project. Further work includes a final Ridge test comparison, recording package versions, and external validation.

## XGBoost Diagnostic and Interpretation Gallery

The following 25 figures describe the XGBoost analysis. Ridge comparisons are reported in the results above.

Training performance should be interpreted alongside validation and held-out testing. These figures are not all outputs of nested cross-validation.

### 1. Training predictions

Predictions for observations used to fit the selected XGBoost model.

![XGBoost training parity](figures/01_Training_Parity.png)

### 2. Out-of-fold predictions

These predictions use the selected hyperparameters and are diagnostic estimates, separate from the nested validation reported above.

![Training out-of-fold parity](figures/02_Training_OOF_Parity.png)

### 3. Held-out testing predictions

Predictions for the testing partition, whose condition groups were excluded from training.

![Testing parity](figures/03_Testing_Parity.png)

### 4. Training and testing comparison

![Training and testing comparison](figures/04_Training_Testing_Comparison.png)

### 5. Residuals versus predictions

Residuals are calculated as actual efficiency minus predicted efficiency.

![Residuals versus predictions](figures/05_Residual_Versus_Prediction.png)

### 6. Residuals versus actual efficiency

![Residuals versus actual efficiency](figures/06_Residual_Versus_Actual.png)

### 7. Residual distribution

![Residual histogram](figures/07_Residual_Histogram.png)

### 8. Residual Q-Q plot

This plot compares the test-residual distribution with a normal distribution. Normal-looking residuals alone do not establish predictive reliability.

![Residual Q-Q plot](figures/08_Residual_QQ_Plot.png)

### 9. Sorted test predictions

Observations are sorted by actual efficiency, not by reaction time.

![Sorted testing predictions](figures/09_Sorted_Testing_Predictions.png)

### 10. Absolute prediction errors

![Absolute error versus actual efficiency](figures/10_Absolute_Error_Versus_Actual.png)

### 11. Cumulative error distribution

The curve shows the fraction of test observations with absolute errors at or below each threshold.

![Absolute error cumulative distribution](figures/11_Absolute_Error_ECDF.png)

### 12. Actual and predicted distributions

![Testing target distributions](figures/12_Testing_Target_Distributions.png)

### 13. Error distribution comparison

Training, ordinary out-of-fold, testing, and mean-baseline errors are compared.

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

Importance measures the increase in validation MAE after a feature is shuffled. Error bars show variation across folds. These diagnostics reuse the selected hyperparameters.

![Cross-validated permutation importance](figures/18_Cross_Validated_Permutation_Importance.png)

### 19. XGBoost gain importance

Normalized gain importance is reported for encoded model features.

![XGBoost gain importance](figures/19_XGBoost_Gain_Importance.png)

### 20. Grouped learning curve

Training subsets are selected by condition group. The shaded band represents variation in validation MAE across folds, not a confidence interval.

![Grouped learning curve](figures/20_Grouped_Learning_Curve.png)

### 21. Validation-fold stability

These fold errors use the selected XGBoost parameters and are separate from the nested outer-fold results.

![Validation fold stability](figures/21_Validation_Fold_Stability.png)

### 22. SHAP feature contributions

SHAP values explain the fitted model’s predictions on training observations; they do not establish chemical mechanisms.

![Training SHAP beeswarm](figures/22_Training_SHAP_Beeswarm.png)

### 23. Mean absolute SHAP importance

![Training SHAP importance](figures/23_Training_SHAP_Importance.png)

### 24. Partial dependence

These curves describe the fitted model’s average response to individual features. Interpret them cautiously where experimental coverage is sparse or feature combinations are unrealistic.

![Training numeric partial dependence](figures/24_Training_Numeric_Partial_Dependence.png)

### 25. Training, validation, and testing dashboard

The dashboard summarizes training fit, ordinary out-of-fold predictions, and held-out testing. Nested validation results are reported separately above.

![Training validation testing dashboard](figures/25_Training_Validation_Testing_Dashboard.png)
