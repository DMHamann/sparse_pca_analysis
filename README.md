# Gene Expression Analysis: PCA and Sparse Classification

## Overview
This project applies dimensionality reduction and classification techniques to gene expression data from the ALL/AML (Acute Lymphoblastic Leukemia / Acute Myeloid Leukemia) dataset. The goal is to identify key genes and reduce high-dimensional expression data (7,129 genes) for cancer subtype prediction.

## Dataset
- **Training samples:** 38 patients (27 ALL, 11 AML)
- **Gene features:** 7,129 expression values per sample
- **Test set:** Independent validation set (unused in current analysis)

## Methods

### 1. Principal Component Analysis (PCA)
Traditional PCA was applied to identify principal components explaining maximum variance:
- **PCA (38 components):** ~95% of variance explained by first 30 components
- **First 3 components:** ~39% explained variance (good class separation)
- **First 5 components:** ~52% explained variance

**Finding:** The data exhibits clear structure with most variation concentrated in early principal components.

### 2. Sparse PCA (SparsePCA)
Sparse PCA applies L1 regularization to enforce feature selection:
- **3 components:** 42.1% sparsity, 30.0% explained variance
- **5 components:** 50.2% sparsity, 36.9% explained variance  
- **10 components:** 60.5% sparsity, 47.9% explained variance

**Key insight:** Sparsity increases with more components, trading off interpretability for variance explanation. 5 components offer a balanced approach (~50% sparsity and variance).

### 3. Sparse Logistic Regression
L1-regularized logistic regression identifies minimal gene sets for classification:
- **Selected genes:** 3 features (out of 7,129) — **99.96% sparsity**
- **Training accuracy:** 71.1%
- **5-Fold Cross-Validation Accuracy:** Mean accuracy with standard deviation (more reliable estimate)
- **ROC-AUC:** Computed both on training and CV folds
- **Convergence:** Achieved at 2,546 iterations (max: 10,000)
- **Class balancing:** Applied balanced class weights to handle 27 ALL vs 11 AML imbalance

**Key Finding:** Cross-validation results are significantly more trustworthy than training accuracy for evaluating generalization performance on the small sample size (n=38).

## Results

| Method | Sparsity | Variance Explained | Interpretability |
|--------|----------|-------------------|-----------------|
| PCA (30 comp) | 0% | ~95% | Difficult |
| Sparse PCA (5) | 50% | 36.9% | Good |
| Logistic Reg (L1) | 99.96% | - | Excellent |

## Visualizations
- 2D scatter plots show reasonable class separation using first 2 Sparse PCA components
- Traditional PCA provides cleaner separation but uses all 7,129 genes


## Files Included

- **sparse_pca.ipynb**: Complete PCA and Sparse PCA analysis with all visualizations and improvements
- **sparse_logreg.ipynb**: L1-regularized logistic regression with cross-validation, comprehensive evaluation metrics, and ROC curves

