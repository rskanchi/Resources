Survival analysis  
------------------------------------------------------------
*Survival* analysis is the generic name given to the analysis when the outcome in a study is the *time to an event* of interest. There can be one or more events of interest. It differs from other types of data analysis as it accounts for **censored data**, where the event has not occurred for some subjects during the observation period.

## Use of Machine Learning in Survival Analysis

Machine learning (ML) has increasingly been adopted in survival analysis to:
- Handle high-dimensional datasets.
- Model complex, non-linear relationships.
- Enhance predictive accuracy beyond traditional models like Cox proportional hazards.

ML methods allow incorporating **feature selection**, **penalization**, and **ensemble techniques**, which are essential for high-throughput data like genomics and proteomics. 

### LASSO (Least Absolute Shrinkage and Selection Operator)

- **How it works**: LASSO introduces L1-penalty to shrink some coefficients to exactly zero, effectively selecting important features.
- **Strengths**: Effective for high-dimensional datasets, interpretable models.
- **Limitations**: May over-penalize correlated features, leading to arbitrary selection among them.

### Elastic Net

- **How it works**: Combines L1 (LASSO) and L2 (ridge regression) penalties, addressing the shortcomings of LASSO when features are correlated.
- **Strengths**: Balances feature selection and shrinkage, handles multicollinearity.
- **Limitations**: Requires tuning of two hyperparameters $\alpha$ and $\lambda$).

### Ranger (Random Survival Forest)

- **How it works**: A non-parametric random forest method that grows multiple decision trees, splitting data based on survival times to reduce heterogeneity.
- **Strengths**: Captures complex relationships, robust to non-linearity and interactions.
- **Limitations**: Less interpretable, computationally intensive for large data.

### CoxBoost

- **How it works**: Uses gradient boosting to iteratively improve a Cox proportional hazards model, adding predictors that most reduce the residual error at each step.
- **Strengths**: Handles high-dimensional data well, accounts for variable interactions.
- **Limitations**: Sensitive to overfitting, requires hyperparameter tuning.

## Comparing the Four Methods

| Feature                | LASSO                | Elastic Net         | Ranger               | CoxBoost            |
|------------------------|----------------------|---------------------|----------------------|---------------------|
| Feature Selection      | Yes (L1 penalty)     | Yes (L1 & L2 mix)  | No                    | Yes                 |
| Handling Non-linearity | No                   | No                 | Yes                   | Partially           |
| Handling Correlations  | Poor                 | Good               | Excellent             | Excellent           |
| Computational Cost     | Low                  | Moderate           | High                  | Moderate            |
| Interpretability       | High                 | High               | Moderate to Low       | Moderate            |

## Conclusion

Each method has its strengths and is suited to specific scenarios:
- Use **LASSO** or **Elastic Net** for simpler, interpretable models with feature selection.
- Use **Ranger** for datasets with complex relationships and non-linear interactions.
- Use **CoxBoost** for high-dimensional data with interactions.

References  
------------------------------------------------------------
[Survival analysis part I: basic concepts and first analyses](https://www.nature.com/articles/6601118). 

[Survival Analysis Part II: Multivariate data analysis – an introduction to concepts and methods](https://www.nature.com/articles/6601119). 

[Survival Analysis Part III: Multivariate data analysis – choosing a model and assessing its adequacy and fit](https://www.nature.com/articles/6601120). 

[Survival Analysis Part IV: Further concepts and methods in survival analysis](https://www.nature.com/articles/6601117). 

[Regularization Paths for Generalized Linear Models via Coordinate Descent](https://pmc.ncbi.nlm.nih.gov/articles/PMC2929880/). 

[Random survival forests for R](https://journal.r-project.org/articles/RN-2007-015/RN-2007-015.pdf). 

[Allowing for mandatory covariates in boosting estimation of sparse high-dimensional survival models](https://doi.org/10.1186/1471-2105-9-14). 
