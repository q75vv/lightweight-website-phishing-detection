# Hooked No More: Using Machine Learning to Catch Phishing Websites Before They Catch You

**CS4403 Data Mining — University of New Brunswick, Saint John**
**Jackson McIntyre | April 2026**

---

## Overview

This project investigates the performance-efficiency trade-off in phishing website detection using machine learning. Four classification algorithms — Logistic Regression, Decision Tree, Random Forest, and XGBoost — are trained and evaluated across four experimental conditions, varying both the feature set size and the depth of hyperparameter tuning. The goal is to identify which models offer the best balance between predictive accuracy and computational efficiency for real-world deployment in resource-constrained environments such as browser extensions and mobile security applications.

---

## Dataset

The dataset used is the **Phishing Websites Dataset** published by Vrbančič, Fister Jr., and Podgorelec (2020).

- **Source:** https://github.com/GregaVrbancic/Phishing-Dataset
- **Instances:** 88,647 websites
- **Features:** 111 (98 after preprocessing)
- **Target:** Binary — `0` (legitimate) or `1` (phishing)
- **Class distribution:** 65.43% legitimate, 34.57% phishing

> Vrbančič, G., Fister Jr., I., & Podgorelec, V. (2020). Datasets for phishing websites detection. *Data in Brief, 33*, 106438. https://doi.org/10.1016/j.dib.2020.106438

---

---

## Methodology

### Preprocessing
- Removed 13 zero-variance features (domain special character counts — prohibited by DNS naming conventions)
- 80/20 stratified train/test split preserving the 1.89:1 class imbalance ratio
- Feature scaling with StandardScaler (mean=0, std=1)
- Class imbalance handled via balanced class weights and `scale_pos_weight` for XGBoost

### Feature Selection
- Top 20 features selected using a combined score averaging normalized Pearson correlation and normalized Random Forest importance

### Models
- Logistic Regression
- Decision Tree
- Random Forest
- XGBoost

### Experimental Conditions
| Condition | Feature Set | Hyperparameter Grid |
|---|---|---|
| Full | 98 features | Initial grid |
| Reduced | 20 features | Initial grid |
| Tuned | 98 features | Expanded grid |
| Reduced + Tuned | 20 features | Expanded grid |

All models trained using **GridSearchCV with 5-fold cross-validation**. Primary evaluation metric: **F1-score**.

---

## Key Results

| Model | Condition | F1-Score | Accuracy | Training Time (s) |
|---|---|---|---|---|
| XGBoost | Full | **0.962** | **97.3%** | 44.8 |
| Random Forest | Full | 0.959 | 97.2% | 152.4 |
| XGBoost | Tuned | 0.960 | 97.2% | 120.8 |
| Decision Tree | Full | 0.938 | 95.6% | 13.4 |
| Logistic Regression | Full | 0.900 | 92.7% | 39.3 |
| XGBoost | Reduced | 0.910 | 93.7% | 16.2 |

---

## Key Findings

- **XGBoost on the full feature set** is the best overall model — highest F1 (0.962), highest accuracy (97.3%), and fast training (44.8s)
- **Feature reduction has a real cost** — dropping to 20 features consistently reduced F1 by ~5 points across all models
- **Advanced tuning offers diminishing returns** — the initial grid search already found near-optimal configurations; extended tuning added minimal gains at significant computational cost
- **Logistic Regression is the strongest lightweight option** — 92.7% accuracy, F1 of 0.900, trains in ~40 seconds, predicts in under 6ms
- **URL complexity is the dominant phishing signal** — the most predictive features were overwhelmingly path length and slash-count features

---

## Requirements
- python >= 3.10
- numpy
- pandas
- matplotlib
- scikit-learn
- xgboost

Install dependencies: 
```
pip install numpy pandas matplotlib scikit-learn xgboost
```

## Usage
- Clone the repository and download the dataset from the link above. Place it at data/raw/dataset_full.csv. 
- Open main.ipynb in Jupyter Notebook, Jupyterlab, Colab, or your desired code editor that supports notebook files. 
 - Run all cells in order. 

 This project was developed as part of the CS4403 Data Mining course at the Univeristy of New Brunswick, Saint John. 