# Batch Correction Methods

Batch effects occur in high-throughput experiments when samples are processed in different batches, 
leading to systematic, non-biological variability. If these technical differences between experimental batches 
are not corrected, the batch effects can lead to misleading biological conclusions. 

Overview of batch correction methods, their computational details, and their pros and cons
============================================================

## 1. ComBat (Empirical Bayes Method)
### Computational Details
- Uses an empirical Bayes framework to estimate batch effects and remove them.
- Assumes that batch effects can be modeled as additive and multiplicative effects.
- Adjusts for known covariates to preserve biological signal.

### Pros
- Effective for removing batch effects in large datasets.  
- Can account for covariates, preserving biological variation.  
- Works well for gene expression, proteomics, and metabolomics data.  

### Cons
- Assumes batches have a common variance structure.  
- May overcorrect if batch effects are confounded with biological signals.  
- Requires batch labels.  

## 2. NIPALS (Nonlinear Iterative Partial Least Squares)
### Computational Details
- Iterative algorithm for performing PCA when data have missing values.
- Extracts principal components one at a time by maximizing variance.
- Missing values are estimated in each iteration until convergence.

### Pros
- Handles missing data well.  
- Computationally efficient for small datasets.  
- Works for both complete and incomplete datasets.  

### Cons
- Can be slow for very large datasets.  
- Sensitive to noise and outliers.  

## 3. rNIPALS (Robust NIPALS)
### Computational Details
- Modified version of NIPALS that is robust to outliers.
- Uses robust covariance estimation instead of standard covariance.

### Pros
- More stable than standard NIPALS.  
- Handles missing values while reducing the impact of outliers.  
- Useful for datasets with high variability.

### Cons
- Slightly more computationally intensive than standard NIPALS.  
- Requires parameter tuning to balance robustness.  

## 4. BPCA (Bayesian Principal Component Analysis)
### Computational Details
- Uses a Bayesian probabilistic model to estimate missing values and correct batch effects.
- Infers posterior distributions for missing values.
- Utilizes an Expectation-Maximization (EM) algorithm for optimization.

### Pros
- Performs well with small sample sizes.  
- Accounts for uncertainty in missing values.  
- Less prone to overfitting compared to standard PCA.  

### Cons
- Computationally intensive.  
- Requires careful choice of priors and hyperparameters.  

## 5. PPCA (Probabilistic Principal Component Analysis)
### Computational Details
- Probabilistic extension of PCA using a latent variable model.
- Estimates principal components through a Gaussian likelihood framework.
- Missing values are inferred from a probability distribution.

### Pros
- Handles missing data naturally without imputation.  
- More flexible than standard PCA.  
- Suitable for high-dimensional datasets.

### Cons
- Requires probabilistic modeling expertise.  
- Can be slow for very large datasets.  

## 6. Limma's removeBatchEffect
### Computational Details
- Uses a linear model to estimate and remove batch effects.
- Fits a design matrix to adjust for batch variation.
- Does not introduce new variance into the data.

### Pros
- Simple and efficient for small datasets.  
- Preserves overall data structure.  
- Works well when batch effects are known and additive.

### Cons
- Assumes a linear relationship between batch effects and data.  
- May not work well if batch effects are highly non-linear.  

## Summary of Batch Correction Methods
| Method | Handles Missing Data | Robust to Outliers | Requires Batch Labels | Model Type |
|--------|----------------------|--------------------|----------------------|------------|
| ComBat | No | No | Yes | Empirical Bayes |
| NIPALS | Yes | No | No | PCA |
| rNIPALS | Yes | Yes | No | Robust PCA |
| BPCA | Yes | Yes | No | Bayesian PCA |
| PPCA | Yes | Yes | No | Probabilistic PCA |
| Limma | No | Yes | Yes | Linear Model |

## Choosing the Right Method
- **For large datasets with known batch labels** Use **ComBat**.
- **For datasets with missing values** Use **NIPALS, BPCA, or PPCA**.
- **For robust correction with outliers** Use **rNIPALS or BPCA**.
- **For simple correction without complex modeling:** Use **Limma's removeBatchEffect**.
