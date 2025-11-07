# Thermophysical-Property-Melting-Point
Predicting the melting point of organic molecules is a long-standing challenge in chemistry and chemical engineering. Melting point is critical for drug design, material selection, and process safety, yet experimental measurements are often costly, time-consuming, or unavailable.
I have evaluated the model with engineered features and compared its performance to the model trained with original features. The results indicate that the engineered features did not improve the model's performance.

### Feature Engineering Steps:
1.  **Analyzed Existing Features**: We examined the correlations of the original features with the target variable (`Tm`) and visualized the distributions of some features to understand their characteristics and potential relationships.
2.  **Generated New Features**: Based on the correlation analysis, we generated new features by creating interaction terms and polynomial features (squared terms) from the top 20 most correlated original features.
3.  **Evaluated New Features**: We calculated the correlations of the newly engineered features with the target variable to assess their potential usefulness. Many of the engineered features showed moderate to high absolute correlations with `Tm`.
4.  **Retrained the Model with New Features**: We split the data with engineered features, scaled them, and retrained the Linear Regression model using this expanded feature set.
5.  **Evaluated the Retrained Model**: We evaluated the performance of the model with engineered features on the validation set and compared the evaluation metrics (MAE, MSE, RMSE, R2) to the results obtained from the model trained with original features.

### Outcome:

The evaluation of the model trained with engineered features showed a significant decrease in performance compared to the model trained with only the original features.

*   **Mean Absolute Error (MAE)** increased from 36.90 to 42.77.
*   **Mean Squared Error (MSE)** increased from 3051.05 to 6049.84.
*   **Root Mean Squared Error (RMSE)** increased from 55.24 to 77.78.
*   **R-squared (R2)** decreased from 0.59 to 0.19.

This indicates that the specific feature engineering techniques applied (creating interaction and polynomial terms from the top correlated features) did not improve the Linear Regression model's ability to predict melting points and, in fact, led to worse performance. This could be due to various reasons, such as introducing multicollinearity, overfitting to the training data with the increased number of features, or the linear model not being suitable for capturing complex relationships introduced by these engineered features.

### Next Steps:

Based on this outcome, it would be beneficial to explore alternative feature engineering strategies or consider using more complex models that might be better suited to handle the data and potentially benefit from engineered features.

Compare feature importance of the two models:

Comparing feature importance directly between Linear Regression and Random Forest models is a bit tricky because they calculate and interpret feature importance differently.

Random Forest models provide a built-in feature_importances_ attribute, which typically measures how much each feature contributes to reducing impurity or error across all the trees in the forest.
Linear Regression models, on the other hand, use coefficients to indicate the strength and direction of the linear relationship between each feature and the target variable. The magnitude of the coefficient can be an indicator of importance, but it's influenced by the scale of the feature.

As mentioned earlier, Linear Regression models use coefficients to indicate feature importance. The magnitude of the coefficient (absolute value) suggests the strength of the relationship between the feature and the target variable, assuming other features are held constant.

I will display the coefficients of the Linear Regression model. Remember that these coefficients represent a linear relationship, unlike the potentially non-linear relationships captured by the Random Forest feature importances.

I have displayed the top 20 features based on the absolute values of their coefficients from the Linear Regression model, as well as their actual coefficient values.

These coefficients indicate the estimated change in melting point for a one-unit increase in the feature, holding other features constant. For example, 'Group 15' has a large positive coefficient (36.20), suggesting a strong positive linear relationship with the melting point according to this model. 'Group 169' has a large negative coefficient (-33.14), indicating a strong negative linear relationship.
I have analyzed the correlations of the engineered features with Tm. You can see the top 20 most correlated engineered features above.

It seems there were some RuntimeWarning messages during the correlation calculation, likely due to some engineered features having constant values. While the correlation for non-constant features is displayed, we should keep this in mind.
## Summary:

### Data Analysis Key Findings

*   Correlation analysis revealed varying degrees of linear relationships between the original features and the target variable `Tm`, with some features showing moderate positive correlations (e.g., 'Group 15') and others having very low or near-zero correlations.
*   Several original features exhibited `NaN` correlation values, indicating potential issues like constant values or missing data.
*   Histograms of selected original features showed different distribution shapes, including some skewed distributions, and box plots highlighted the presence of potential outliers.
*   After generating interaction and polynomial features from the top 20 most correlated original features, several of these engineered features showed moderate to high absolute correlations with the target variable.
*   Retraining a Linear Regression model with the engineered features resulted in significantly worse performance compared to the model trained with original features. The MAE increased, the MSE increased, the RMSE increased, and the R-squared value decreased from 0.59 to 0.19.

### Insights or Next Steps

*   The current feature engineering approach (creating interaction and polynomial terms from the top correlated features) did not improve the model performance and likely introduced noise or multicollinearity.
*   Further feature engineering efforts should explore different techniques, such as handling outliers, addressing skewed distributions, or using dimensionality reduction methods, rather than simply adding interaction and polynomial terms.
I have trained an XGBoost Regressor model on the PCA-transformed data and evaluated its performance. The evaluation metrics and a comparison to the XGBoost model trained on the original features are displayed above.

It appears that using PCA with 10 components resulted in worse performance compared to using the original features for the XGBoost model. This suggests that reducing the dimensionality to just 10 components might have led to a loss of important information for predicting melting points.

Based on these results, we can conclude that PCA with this number of components was not beneficial for this model. 
## Summary:

### Data Analysis Key Findings

*   The XGBoost model achieved the best performance on the validation set with the lowest MAE (36.11), MSE (3011.93), and RMSE (54.88), and the highest R2 score (0.60).
*   The LightGBM model performed the worst among the evaluated models.
*   According to the XGBoost model, "Group 373", "Group 129", and "Group 15" were identified as the top three most important features for predicting melting points.
*   A comparison of feature importances across Linear Regression, Random Forest, and XGBoost models showed differing rankings of features by each model.

### Insights or Next Steps

*   The superior performance of the XGBoost model suggests it is the most suitable model among those evaluated for predicting melting points based on the provided group contribution features. Further optimization of the XGBoost model's hyperparameters could potentially improve performance.
*   Investigating the specific chemical significance of the top-ranked features identified by XGBoost (e.g., "Group 373", "Group 129", "Group 15") could provide valuable chemical insights into the factors most influencing melting points.
*  The evaluation_df DataFrame contains the performance metrics for the Linear Regression, Random Forest, default XGBoost, and tuned XGBoost models on the validation set.

Looking at the table:

MAE (Mean Absolute Error): This is the primary metric for the competition (lower is better). The Tuned XGBoost model has the lowest MAE (35.86), followed closely by the Default XGBoost (36.11), Random Forest (36.49), and Linear Regression (36.90).
MSE (Mean Squared Error): This metric penalizes larger errors more (lower is better). The Tuned XGBoost model also has the lowest MSE (2951.27).
RMSE (Root Mean Squared Error): This is in the same units as the target variable and represents typical prediction error (lower is better). Again, the Tuned XGBoost model has the lowest RMSE (54.33).
R2 (R-squared): This represents the proportion of variance explained by the model (higher is better). The Tuned XGBoost model has the highest R2 score (0.61).
Based on these metrics, particularly the MAE which is the competition's scoring metric, the Tuned XGBoost model is the best performing model among those we have evaluated so far on the validation set.

This analysis confirms that hyperparameter tuning improved the performance of the XGBoost model, making it the most suitable model for this task among the ones I've tried. 
