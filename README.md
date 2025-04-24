# 🧬 Integrative-Genomic-Variant-Analysis-and-Consequence-Prediction

This project presents a comprehensive machine learning pipeline for analyzing genetic mutations using the **ClinVar Conflicting Dataset**. By leveraging advanced data preprocessing, feature engineering, multiple ML models, and explainable AI techniques, this study aims to improve the prediction and interpretation of pathogenic genetic variants.

---

## 📂 Dataset
- **Source:** [Genetic Variant Classifications](https://www.kaggle.com/datasets/kevinarvai/clinvar-conflicting)
- **Samples:** 65,188 genetic variants
- **Features:** 46 initial features including allele frequencies, mutation consequences, protein impact scores (SIFT, PolyPhen), and conservation scores (CADD, BLOSUM62).
- Focus on **missense mutations**, pathogenicity classification, and functional impact prediction.

---

## 🎯 Research Objectives
- Predict **pathogenicity (CLASS)** of genetic mutations.
- Model the **PolyPhen** score for missense mutations.
- Analyze the impact of genetic variants on protein function (**Consequence** prediction).
- Apply **SHAP** for model interpretability to identify key biological features driving predictions.

---

## ⚙️ Methodology

### 1. **Data Cleaning & Preprocessing**
- Removed features with >99% missing values.
- Handled hidden null values (e.g., allele frequencies with `0` as missing indicator).
- Applied:
  - **Interpolation** for missing data.
  - **Box-Cox Transformation** for skewness reduction.
  - **Winsorization** for outlier mitigation.

### 2. **Exploratory Data Analysis (EDA)**
- Generated:
  - Correlation heatmaps to detect multicollinearity.
  - Histograms & boxplots for distribution analysis.
  - Scatter plots for key feature relationships (e.g., AF_ESP vs AF_EXAC).

### 3. **Feature Engineering**
- Created composite features like `AF_avg`.
- Applied **PCA** for dimensionality insights.

### 4. **Modeling Approaches**
- **Classification Tasks**:
  - Logistic Regression
  - Lasso Regression (for feature importance)
  - **XGBoost Classifier** (best performance for `PolyPhen` with ~78.5% accuracy)
- **Regression Tasks**:
  - Lasso Regression
  - **XGBoost Regressor** (achieved R² ≈ 0.945 for `Consequence` prediction)

### 5. **Model Interpretability**
- Utilized **SHAP (SHapley Additive exPlanations)** to:
  - Visualize feature contributions.
  - Enhance biological understanding of mutation impacts.

---

## 📈 Key Results

| **Target Variable** | **Best Model** | **Accuracy / R²** |
|---------------------|----------------|-------------------|
| `CLASS`             | XGBoost        | ~77% Accuracy     |
| `PolyPhen`          | XGBoost        | ~78.5% Accuracy   |
| `Consequence`       | XGBoost        | 0.945 R²          |

- Strong predictive performance, especially with XGBoost models.
- SHAP analysis highlighted critical features like:
  - **Allele Frequencies (AF_ESP, AF_EXAC, AF_TGP)**
  - **SIFT & PolyPhen scores**
  - **CADD_PHRED**, **LoFtool**, and **Codons**

---

## 🖼️ Visualizations
The project includes:
- Correlation heatmaps for feature analysis.
  ![5](https://github.com/user-attachments/assets/d594e274-0879-4918-a22a-1354cb5dbc84)

- Distribution plots pre- and post-transformation. (Please refer to the notebook for full visual outputs.)
- SHAP summary plots & force plots for individual prediction explanations.

![9](https://github.com/user-attachments/assets/ded383cc-7353-41dc-b57d-b6d5eee564c3)
![10](https://github.com/user-attachments/assets/fc371415-132f-4d6d-a676-cefc7b712dd9)
![11](https://github.com/user-attachments/assets/8efe6005-a5ef-4b4f-ad24-25319b7f6d7a)
- Confusion matrices for classification evaluations.
![12](https://github.com/user-attachments/assets/61821c7a-f3ca-4821-9186-19418aa3812b)

*(Due to GitHub limitations, please refer to the notebook for full visual outputs.)*

