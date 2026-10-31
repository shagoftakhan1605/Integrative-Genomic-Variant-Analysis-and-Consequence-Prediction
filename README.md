# Integrative Genomic Variant Analysis and Consequence Prediction

This project explores the ClinVar Conflicting dataset with data cleaning, feature selection, several machine learning models and SHAP explanations. All code is in [`Clinvar_Mutations.ipynb`](Clinvar_Mutations.ipynb).

---

## Dataset

* **Source:** [ClinVar Conflicting](https://www.kaggle.com/datasets/kevinarvai/clinvar-conflicting) (Kaggle, Kevin Arvai), built from ClinVar with VEP annotations. A zipped copy is included as `clinvar_conflicting.csv.zip`.
* **Size:** 65,188 variants, 46 columns
* **What `CLASS` means:** `CLASS = 1` if different laboratories gave the variant **conflicting clinical classifications**, and `0` if they agreed. It is **not** a pathogenic vs benign label. About 25% of variants are conflicting.

---

## Tasks

1. Predict whether a variant has conflicting classifications (`CLASS`)
2. For missense variants, predict the PolyPhen category (benign, possibly damaging, probably damaging, unknown)
3. Explain the PolyPhen model with SHAP

---

## Method (as implemented in the notebook)

### Cleaning
* Dropped columns with more than 99% missing values and several identifier or free-text columns
* Filled remaining gaps (forward fill for categorical and BLOSUM62)
* Allele frequencies of `0` treated as missing, then filled by interpolation after comparing mean, median and interpolation by skewness and kurtosis
* Box-Cox transformation applied to allele frequencies; winsorisation was also tested for comparison
* Categorical columns label-encoded

### Exploration and feature engineering
* Correlation heatmaps, histograms, boxplots and scatter plots of allele frequencies
* `AF_avg` (mean of AF_ESP, AF_EXAC and AF_TGP) and one PCA component of the allele frequencies (87% of their variance)
* Feature selection with chi-squared `SelectKBest`, RFE and Lasso

### Models
* `CLASS`: Logistic Regression, Random Forest, XGBoost (80/20 split)
* `PolyPhen` (32,408 missense variants): Logistic Regression and XGBoost
* SHAP `TreeExplainer` on the PolyPhen XGBoost model

---

## Results

| Task | Model | Accuracy |
| --- | --- | --- |
| `CLASS` (conflicting or not) | Majority class baseline | 74.9% |
| `CLASS` | Logistic Regression | 74.9% (predicts only class 0) |
| `CLASS` | Random Forest | 76.3% |
| `CLASS` | XGBoost | 76.6% |
| `PolyPhen` (missense) | Logistic Regression | 74.0% |
| `PolyPhen` (missense) | XGBoost | 78.7% |

* For `CLASS`, the best model is only about 2 points above simply predicting "not conflicting" every time. Predicting conflicting classifications from these features is hard.
* For `PolyPhen`, XGBoost separates benign and probably damaging variants well but struggles with possibly damaging ones.

---

## Visualisations

Correlation heatmap:
![5](https://github.com/user-attachments/assets/d594e274-0879-4918-a22a-1354cb5dbc84)

SHAP plots for the PolyPhen model:
![9](https://github.com/user-attachments/assets/ded383cc-7353-41dc-b57d-b6d5eee564c3)
![10](https://github.com/user-attachments/assets/fc371415-132f-4d6d-a676-cefc7b712dd9)
![11](https://github.com/user-attachments/assets/8efe6005-a5ef-4b4f-ad24-25319b7f6d7a)

Confusion matrix for the PolyPhen model:
![12](https://github.com/user-attachments/assets/61821c7a-f3ca-4821-9186-19418aa3812b)

See the notebook for all plots.

---

## Limitations

* **Consequence regression is not valid.** The notebook also fits an XGBoost regressor to `Consequence` (R² 0.946). `Consequence` is a categorical label encoded as integers, so regression on it is not meaningful. The features also include `IMPACT`, which VEP derives directly from `Consequence`, so the target leaks into the inputs. This result is excluded from the table above.
* **Confusion matrix labels.** The label encoder orders PolyPhen classes alphabetically (benign, possibly damaging, probably damaging, unknown), but the plot labels the second and third classes the other way round.
* **Imbalance.** No class weighting or resampling was used for `CLASS`, so accuracy is close to the majority baseline. Balanced accuracy, F1 for the minority class and ROC-AUC would be more informative.
* **Single split.** All results come from one 80/20 split without cross-validation.

---

## Next Steps

* Report balanced accuracy, minority-class F1 and ROC-AUC for `CLASS`, with class weights
* Treat `Consequence` as a classification task and remove `IMPACT` from its features
* Fix the PolyPhen confusion matrix labels
* Use cross-validation and keep variants from the same gene out of both train and test sets

---

## Tools

Python, pandas, NumPy, SciPy, scikit-learn, XGBoost, SHAP, Matplotlib, Seaborn
