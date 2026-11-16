# Hypertension Control Status Prediction

A machine learning project that predicts whether a patient's hypertension is **controlled** or **uncontrolled**, using demographic, clinical and lifestyle data from 100,000 patient records.

## Overview

The notebook walks through the full pipeline:

1. **Data loading**: reads the patient dataset from an Excel file.
2. **Preprocessing**: checks for missing values, converts the income ranges (e.g. `5001-10000`, `>15000`) into numeric values, and cleans column names.
3. **Encoding**: label-encodes binary columns and one-hot encodes multi-class columns (Sex, Education, Occupation, Residence, Treatment).
4. **Correlation analysis**: heatmap of numeric features and their correlation with the target.
5. **Data split**: 70% train / 10% validation / 20% test, stratified on the target.
6. **Model training**: six individual classifiers.
7. **Ensembles**: three soft-voting ensembles.
8. **Evaluation**: accuracy, precision, recall, F1 and confusion-matrix counts on validation and test sets.
9. **Feature importance**: top features from Random Forest, XGBoost and LightGBM.

## Dataset

- **File:** `Hypertension_dataset.xlsx` (100,000 rows, 21 columns)
- **Target:** `Control Status` (0 = Controlled, 1 = Uncontrolled)
- **Class balance:** about 72% uncontrolled, 28% controlled

| Type | Features |
|------|----------|
| Demographic | Age, Sex, Education, Occupation, Monthly Income, Residence |
| Clinical | Systolic BP, Diastolic BP, Elevated Creatinine, Diabetes Mellitus, Elevated Cholesterol, LVH, IHD, CVD, Retinopathy, Family History of CVD |
| Lifestyle | Smoking, Physical Activity, Dietary Habits |
| Treatment | Single Drug / Combination Drugs |

> The dataset is not included in this repository. Place `Hypertension_dataset.xlsx` in the project folder and update `file_path` in the notebook to point to it.

## Models

**Individual models:** Random Forest, XGBoost, LightGBM, CatBoost, SVC (with standard scaling), MLP (with standard scaling).

**Ensembles (soft voting):**
- **Tree ensemble:** Random Forest + XGBoost + CatBoost
- **Mixed ensemble:** Random Forest + CatBoost + SVC
- **Light ensemble:** Random Forest + LightGBM

## Results (test set)

| Model | Accuracy | Precision | Recall | F1 |
|-------|----------|-----------|--------|-----|
| Random Forest | 0.952 | 0.945 | 0.991 | 0.968 |
| XGBoost | 0.780 | 0.772 | 0.987 | 0.867 |
| LightGBM | 0.740 | 0.736 | 0.999 | 0.847 |
| CatBoost | 0.793 | 0.781 | 0.994 | 0.874 |
| SVC | 0.789 | 0.776 | 0.996 | 0.872 |
| MLP | 0.852 | 0.865 | 0.942 | 0.902 |
| Ensemble: Tree | 0.847 | 0.826 | 0.999 | 0.904 |
| Ensemble: Mixed (SVC) | 0.864 | 0.846 | 0.994 | 0.914 |
| Ensemble: Light | 0.873 | 0.851 | 0.999 | 0.919 |

**Best performer:** Random Forest.

**Top features** (Random Forest and LightGBM agree): Systolic BP, Age, Diastolic BP, Monthly Income, Family History of CVD, Smoking.

## Notes and limitations

- Individual numeric features have very weak linear correlation with the target (|r| < 0.02), yet Random Forest reaches about 95% accuracy while boosting models reach about 74-79%. This large gap suggests the target depends on nonlinear patterns or on how the dataset was constructed (it may be synthetic). Results should not be assumed to generalise to real clinical data.
- All models predict "uncontrolled" far more often than "controlled" (high recall, lower precision on the minority class). Class weighting or resampling could be explored.
- This project is for educational purposes and is **not** a medical tool.

## Setup

```bash
pip install pandas numpy scikit-learn xgboost lightgbm catboost seaborn matplotlib openpyxl joblib
```

Then open `Hypertension.ipynb` in Jupyter or Google Colab and run the cells in order.

If running on Colab, the notebook mounts Google Drive to read the dataset. Otherwise, change `file_path` to a local path.

## Saved model

The notebook saves the trained Random Forest as `rf_model.pkl` (about 200 MB). It is excluded from this repository because it exceeds GitHub's 100 MB file limit. Re-run the notebook to regenerate it.

## Project structure

```
.
├── Hypertension.ipynb   # Full analysis and modelling notebook
├── README.md
└── .gitignore           # Excludes *.pkl and dataset files
```

## Team

- Nanzeeba Ayman
- Umme Hafsa Mazumder
- Nadia Mahzabin
- Farzana Oyshi
