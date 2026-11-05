# XGBoost Diagnostics and Interpretation Gallery

These 25 figures show the XGBoost analysis of published photocatalysis data. They include training, validation, testing, and interpretation outputs.

Training fit does not establish predictive accuracy. Ordinary out-of-fold plots reuse tuned parameters; nested-validation results are reported separately in the main README.

## Overall performance

![Training, validation, and testing dashboard](25_Training_Validation_Testing_Dashboard.png)

## Prediction quality

![Training parity](01_Training_Parity.png)
![Training out-of-fold parity](02_Training_OOF_Parity.png)
![Held-out testing parity](03_Testing_Parity.png)
![Training and testing comparison](04_Training_Testing_Comparison.png)

## Residuals and prediction errors

![Residual versus prediction](05_Residual_Versus_Prediction.png)
![Residual versus actual](06_Residual_Versus_Actual.png)
![Residual histogram](07_Residual_Histogram.png)
![Residual Q-Q plot](08_Residual_QQ_Plot.png)
![Sorted testing predictions](09_Sorted_Testing_Predictions.png)
![Absolute error versus actual](10_Absolute_Error_Versus_Actual.png)
![Absolute error cumulative distribution](11_Absolute_Error_ECDF.png)
![Testing target distributions](12_Testing_Target_Distributions.png)
![Error distribution comparison](13_Error_Distribution_Comparison.png)
![Residual scale plot](14_Residual_Scale_Plot.png)
![Numeric feature residuals](15_Numeric_Feature_Residuals.png)

## Data exploration

![Training correlation matrix](16_Training_Correlation_Matrix.png)
![Numeric feature distributions](17_Numeric_Feature_Distributions.png)

## Feature importance and learning

Importance and dependence plots describe model behaviour and do not establish causal chemical mechanisms.

![Cross-validated permutation importance](18_Cross_Validated_Permutation_Importance.png)
![XGBoost gain importance](19_XGBoost_Gain_Importance.png)
![Grouped learning curve](20_Grouped_Learning_Curve.png)
![Validation fold stability](21_Validation_Fold_Stability.png)
![Training SHAP beeswarm](22_Training_SHAP_Beeswarm.png)
![Training SHAP importance](23_Training_SHAP_Importance.png)
![Training numeric partial dependence](24_Training_Numeric_Partial_Dependence.png)

[Return to the project overview](../README.md)
