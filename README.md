# 🔍 Detection of Malicious URLs - Mini-Project

A comprehensive machine learning and deep learning pipeline for detecting malicious URLs using the **ISCX-URL2016** dataset. This project combines classical ML, Deep Learning, and advanced ensemble methods to classify URLs into 5 categories: Benign, Defacement, Malware, Phishing, and Spam.

---

## 📊 Project Overview

This project demonstrates a complete end-to-end machine learning workflow:
- **Data Preprocessing & EDA** (Notebook 1)
- **Classical ML Models** (Notebook 2)
- **Deep Learning Models** (Notebook 3)
- **Ensemble Methods & Hyperparameter Tuning** (Notebook 4)
- **Final Evaluation & Benchmarking** (Notebook 5)

### 🏆 Key Results

| Metric | LightGBM (Champion) |
|--------|-------------------|
| **Accuracy** | **98.73%** |
| **Macro F1-Score** | 0.9874 |
| **ROC-AUC** | 0.9996 |
| **Macro FPR** | 0.0032 |

---

## 📁 Project Structure

```
Detection_of_Malicious_URLs/
├── src/
│   ├── 01_EDA_and_Preprocessing.ipynb      # Data loading, cleaning, feature selection
│   ├── 02_ML_Models.ipynb                   # Classical ML algorithms (7 models)
│   ├── 03_DL_Models.ipynb                   # Deep Learning (MLP, CNN, TabNet)
│   ├── 04_Ensemble_and_Tuning.ipynb        # Ensemble methods & Optuna tuning
│   └── 05_Final_Evaluation.ipynb            # ROC curves, cross-validation, benchmarks
├── output/
│   ├── processed_data.csv                   # 40 selected features + labels
│   ├── label_map.json                       # Class label mapping
│   └── top_features.json                    # 40 selected feature names
├── models/
│   ├── ml_results.csv                       # ML model performance metrics
│   ├── dl_results.csv                       # DL model performance metrics
│   ├── MLP_best.pt                          # Best PyTorch model weights
│   └── n_features.json                      # Feature count metadata
└── Mini_Projet_Report.tex                   # Full LaTeX report (English)
```

---

## 🎯 Dataset

**ISCX-URL2016**: A publicly available dataset from the University of New Brunswick
- **30,000+** URL examples
- **80+** initial features extracted from URLs
- **5 classes**: Benign, Defacement, Malware, Phishing, Spam
- **Feature types**: Structural, entropy, composition, and argument-based features

### Top 10 Most Important Features
1. `domain_token_count`
2. `ArgUrlRatio`
3. `domainlength`
4. `NumberofDotsinURL`
5. `avgdomaintokenlen`
6. `pathurlRatio`
7. `CharacterContinuityRate`
8. `argPathRatio`
9. `Entropy_Domain`
10. `Domain_LongestWordLength`

---

## 🔧 Models Evaluated

### Classical Machine Learning (7 Models)
| Model | Accuracy | F1-Score | FPR |
|-------|----------|----------|-----|
| **LightGBM** | **0.9873** | 0.9874 | 0.0032 |
| XGBoost | 0.9837 | 0.9838 | 0.0041 |
| Random Forest | 0.9752 | 0.9755 | 0.0062 |
| Decision Tree | 0.9516 | 0.9517 | 0.0121 |
| Logistic Regression | 0.8001 | 0.7991 | 0.0501 |
| SVM (Linear) | 0.7700 | 0.7655 | 0.0576 |
| AdaBoost | 0.7126 | 0.7067 | 0.0718 |

### Deep Learning (3 Models)
| Model | Accuracy | F1-Score | AUC |
|-------|----------|----------|-----|
| MLP | 0.9785 | 0.9787 | 0.9988 |
| 1D-CNN | 0.9783 | 0.9786 | 0.9975 |
| TabNet | 0.9781 | 0.9785 | 0.9984 |

### Ensemble Methods
- **Optuna-Tuned LightGBM**: 98.74% accuracy
- **Binary Specialist Ensemble**: ~98.70% accuracy
- **Stacking Ensemble**: ~98.68% accuracy
- **Weighted Soft Voting**: ~98.65% accuracy

---

## 📈 Benchmark Comparison

Performance comparison against alternative feature selection methods:

| Method | Accuracy |
|--------|----------|
| **Our Pipeline (40 RF-selected features)** | **0.9873** |
| BestFirst (8 features) | ~0.92 |
| Infogain (12 features) | ~0.89 |

Our feature selection pipeline significantly outperforms simpler feature selection approaches.

---

## 📊 Preprocessing Pipeline

### Step 1: Data Cleaning
- Remove constant columns (zero variance)
- Replace -1 sentinels with NaN
- Impute missing values with median

### Step 2: Feature Engineering
- **Variance Threshold**: Remove zero-variance features
- **Correlation Filter**: Drop pairs with |r| > 0.95
- **Random Forest Importance**: Select top 40 features

### Step 3: Data Preparation
- Standard scaling (mean=0, std=1)
- Train-Test split (80/20, stratified)
- SMOTE for class imbalance handling

---

## 🚀 Quick Start

### Requirements
```bash
pip install pandas numpy scikit-learn lightgbm xgboost
pip install torch pytorch-tabnet optuna
pip install matplotlib seaborn
pip install imbalanced-learn
```

### Running the Notebooks (Google Colab)

1. **Mount Google Drive** and update paths in each notebook
2. **Run sequentially** (Notebooks 1→2→3→4→5)
3. **Check outputs** in the `models/` and `output/` directories

```python
# Example: Load and use the champion model
import joblib
import json

champion = joblib.load('lightgbm_champion.joblib')
with open('label_map.json') as f:
    label_map = json.load(f)

# Make predictions
predictions = champion.predict(X_new_scaled)
probabilities = champion.predict_proba(X_new_scaled)
```

---

## 📋 Notebook Summary

### 📓 Notebook 1: EDA & Preprocessing
- Load ISCX-URL2016 dataset
- Exploratory data analysis (class distribution, correlations)
- Data cleaning & sentinel value handling
- Three-stage feature selection
- Save 40 selected features for downstream use

### 📓 Notebook 2: ML Models
- Train 7 classical algorithms
- Handle class imbalance with SMOTE
- Compute metrics: Accuracy, F1, FPR, ROC-AUC
- Generate confusion matrices
- Save best 3 models (RF, XGBoost, LightGBM)

### 📓 Notebook 3: DL Models
- Implement MLP, 1D-CNN, and TabNet
- Early stopping & learning rate scheduling
- Label smoothing for regularization
- Cross-entropy loss with class weights
- Export models in `.pt` format

### 📓 Notebook 4: Ensemble & Tuning
- **Optuna** Bayesian hyperparameter tuning (50 trials)
- Binary Specialist Ensemble (one RF per class)
- Stacking Ensemble (RF + XGB + LightGBM → LR)
- Weighted Soft Voting
- Generate final leaderboard

### 📓 Notebook 5: Final Evaluation
- Load champion model (LightGBM)
- ROC curves (one-vs-rest per class)
- Precision-Recall curves
- 5-fold cross-validation
- Benchmark against BestFirst & Infogain splits
- Interactive prediction demo





---

## 🎓 References

- ISCX-URL2016 Dataset: University of New Brunswick
- LightGBM: https://lightgbm.readthedocs.io/
- Scikit-learn: https://scikit-learn.org/
- XGBoost: https://xgboost.readthedocs.io/
- PyTorch: https://pytorch.org/

---


# Detection-of-Malicious-URLs-Mini-Project
