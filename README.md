# Spam Email Classification

A comparative analysis of classical machine learning methods for detecting spam emails, using 57 engineered features extracted from 4,601 email messages.

## Overview

This project explores how different **preprocessing strategies** and **classification algorithms** affect spam detection accuracy. The goal was to find the best-performing combination and understand which email features are most predictive of spam.

### Dataset

- **4,601 emails** split into 3,065 training and 1,536 test samples
- **57 features** per email:
  - 48 word-frequency percentages (e.g., "business", "free", "george")
  - 6 character-frequency percentages (`;`, `(`, `[`, `!`, `$`, `#`)
  - Average, max, and total length of capital-letter runs

### Preprocessing Methods

Each method transforms the raw features differently before feeding them into classifiers:

| Method | Description |
|--------|-------------|
| Standardization | Zero mean, unit variance (z-score normalization) |
| Log transform | `log(x + 1)` — compresses skewed distributions |
| Binarization | `I(x > 0)` — reduces features to presence/absence indicators |

### Classifiers

- **Logistic Regression** — interpretable baseline with significance testing
- **LDA / QDA** — discriminant analysis (linear and quadratic decision boundaries)
- **SVM** — support vector machines with linear and RBF kernels

## Key Results

| Method | Preprocessing | Test Error |
|--------|--------------|------------|
| SVM Linear | Log | **5.6%** |
| SVM RBF | Log | 5.7% |
| Logistic | Log | 5.7% |
| LDA | Log | 6.5% |
| SVM Linear | Std | 6.8% |
| Logistic | Std | 7.3% |
| SVM Linear | Bin | 7.4% |
| Logistic | Bin | 8.1% |
| LDA | Std | 9.6% |
| QDA | Log | 15.7% |
| QDA | Std | 18.4% |

**Best model:** Linear SVM on log-transformed features (5.6% test error).

### Notable Findings

- **Log transformation consistently helps** — it compresses the heavy-tailed word-frequency distributions, making them easier for linear models to work with.
- **Linear models outperform quadratic ones** — QDA overfits on this dataset due to the large number of features relative to the class-conditional sample sizes.
- **A small subset of features drives most of the signal** — after Bonferroni correction for multiple testing, ~12 features remain statistically significant across all models (word frequencies for specific spam-indicative terms and capital-letter statistics).

## Project Structure

```
├── README.md                     # This file
├── spam_classification.Rmd       # Full analysis (R Markdown)
└── study_guide.md                # Detailed walkthrough of methods and code
```

## How to Run

1. Place `spam-train.txt` and `spam-test.txt` in the project directory
2. Open `spam_classification.Rmd` in RStudio
3. Install dependencies: `ggplot2`, `gridExtra`, `MASS`, `e1071`
4. Knit to HTML

## Tools

- **R** with `ggplot2`, `MASS`, `e1071`
- PCA for dimensionality reduction / visualization
- Bonferroni correction for multiple hypothesis testing
