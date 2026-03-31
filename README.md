# Spam Email Classification

A comparative study of classical ML classifiers for spam detection, benchmarking different preprocessing strategies against logistic regression, discriminant analysis, and support vector machines.

## Overview

This project takes 4,601 email messages (57 engineered features each) and evaluates how different **preprocessing** × **classifier** combinations affect spam detection accuracy. The goal: find the best-performing setup and understand which features actually matter.

### Dataset

The [Spambase dataset](https://archive.ics.uci.edu/dataset/94/spambase) from the UCI Machine Learning Repository, originally collected at Hewlett-Packard Labs.

- **4,601 emails** — 3,067 training / 1,534 test
- **57 features** per email:
  - 48 word-frequency percentages (`"free"`, `"business"`, `"george"`, etc.)
  - 6 character-frequency percentages (`;` `(` `[` `!` `$` `#`)
  - 3 capital-letter run statistics (average, max, total length)
- See `data/spam-names.txt` and `data/spam-info.txt` for full attribute documentation

### Preprocessing Approaches

| Method | Transformation | Rationale |
|--------|---------------|-----------|
| Standardization | z-score normalization | Puts all features on equal footing |
| Log transform | `log(x + 1)` | Tames skewed word-frequency distributions |
| Binarization | `I(x > 0)` | Reduces to presence/absence — tests if counts even matter |

### Classifiers

- **Logistic Regression** — interpretable baseline + statistical significance testing (Bonferroni correction)
- **LDA / QDA** — linear vs. quadratic discriminant analysis
- **SVM** — linear and RBF kernels

## Results

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

**Best performer:** Linear SVM on log-transformed features — 5.6% test error with no signs of overfitting.

### Key Takeaways

- **Log transform wins across the board** — compressing skewed word frequencies makes linear classifiers much more effective
- **Linear > quadratic** — QDA overfits badly with 57 features and limited per-class samples
- **~12 features carry most of the signal** — after Bonferroni correction, the same core set of word frequencies and capital-letter stats remain significant regardless of preprocessing

## Project Structure

```
├── README.md                     # You're reading it
├── spam_classification.Rmd       # Full analysis code (R Markdown)
├── study_guide.md                # Detailed walkthrough of every section
└── data/
    ├── spam-train.txt            # Training set (3067 emails)
    ├── spam-test.txt             # Test set (1534 emails)
    ├── spam-names.txt            # Feature/attribute names
    └── spam-info.txt             # Dataset documentation
```

## How to Run

1. Clone/download this repo
2. Open `spam_classification.Rmd` in RStudio
3. Install required packages: `ggplot2`, `gridExtra`, `MASS`, `e1071`
4. Knit to HTML — data files are already in `data/`

## Tech Stack

R · ggplot2 · MASS · e1071 · PCA · Logistic Regression · LDA/QDA · SVM · Bonferroni Correction
