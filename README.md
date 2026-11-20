# 🌾 Rice Variety Classification & Quality Control Decision Support

A Machine Learning project that classifies rice grains into **Cammeo** and **Osmancik** varieties using morphological grain measurements and explores a quality-control decision-support approach.

---

## 📌 1. Project Overview

Rice variety identification is an important task in agricultural quality control. Traditional manual identification can be inconsistent, particularly when different rice varieties have similar physical characteristics.

This project develops a supervised Machine Learning classification system to identify two rice varieties:

- **Cammeo**
- **Osmancik**

The system uses measurable morphological characteristics of individual rice grains to predict their variety.

In addition to classification, the project explores a Quality Control (QC) Decision Support approach that compares the predicted rice variety with the expected variety and generates a MATCH or MISMATCH recommendation for further human review.

### Project Objectives

- Understand and explore the rice grain dataset.
- Perform Exploratory Data Analysis (EDA).
- Identify missing values, duplicates, invalid values and outliers.
- Apply appropriate data preprocessing techniques.
- Investigate feature relationships and feature selection.
- Develop and compare multiple classification algorithms.
- Evaluate model performance using appropriate classification metrics.
- Explore SMOTE and feature-ablation experiments.
- Design a QC decision-support workflow.

---

## 📊 2. Dataset Description

The project uses the **Rice Cammeo and Osmancik dataset**.

The dataset contains:

| Property | Description |
|---|---|
| Total observations | 3,810 |
| Input features | 7 numerical features |
| Target variable | Class |
| Classification type | Binary Classification |
| Classes | Cammeo and Osmancik |

### Dataset Features

| Feature | Description |
|---|---|
| Area | Number of pixels representing the grain area |
| Perimeter | Length of the grain boundary |
| Major_Axis_Length | Length of the major axis of the fitted ellipse |
| Minor_Axis_Length | Length of the minor axis of the fitted ellipse |
| Eccentricity | Measure of grain elongation |
| Convex_Area | Area of the grain's convex hull |
| Extent | Ratio of grain area to its bounding rectangle area |
| Class | Target rice variety |

### Class Distribution

- Cammeo: 1,630 observations (42.78%)
- Osmancik: 2,180 observations (57.22%)

The dataset contains a moderate class imbalance, which was considered during model development and evaluation.

**Dataset source and original license:** Add the original dataset publication/source URL and attribution here before publishing the repository.

---

## 🔍 3. Exploratory Data Analysis (EDA)

Exploratory Data Analysis was performed to understand the characteristics and relationships of rice grain measurements.

The analysis included:

- Dataset structure and descriptive statistics.
- Class distribution analysis.
- Feature distribution comparisons.
- Histograms and boxplots.
- Correlation heatmap.
- Feature separation analysis using Cohen's d.
- Investigation of relationships between morphological features.

### Key Findings

- Cammeo grains generally have higher Area, Perimeter and Major Axis Length measurements.
- Major_Axis_Length and Perimeter show strong class separation.
- Area and Convex_Area are highly correlated.
- Extent demonstrates relatively weak linear correlation with other features.

---

## 🧹 4. Data Preprocessing

The dataset was examined to ensure data quality before model development.

The preprocessing and data-quality investigation included:

- Missing value detection.
- Duplicate record detection.
- Numerical validity checks.
- Outlier analysis using the Interquartile Range (IQR) method.
- Stratified train-test splitting.
- Feature scaling using StandardScaler.
- Prevention of data leakage through training-data-based preprocessing.

### Train-Test Split

The dataset was divided using an 80:20 stratified split.

| Dataset | Observations |
|---|---:|
| Training set | 3,048 |
| Testing set | 762 |

The random state was set to 42 for reproducibility.

A total of 85 observations (2.23%) were flagged by the pooled IQR outlier analysis. These observations were retained because they were not confirmed to be invalid measurements.

---

## 🤖 5. Machine Learning Models

Five classification algorithms were investigated and compared.

1. Logistic Regression
2. Support Vector Machine (SVM)
3. K-Nearest Neighbors (KNN)
4. Decision Tree
5. Random Forest

### Model Development Techniques

- Stratified 5-fold cross-validation.
- Hyperparameter tuning.
- Grid Search.
- Randomized Search.
- StandardScaler.
- Feature selection.
- Principal Component Analysis (PCA).
- Classification performance comparison.

### Model Comparison

The following table presents the reported cross-validation development results.

| Model | CV Accuracy | Macro F1 |
|---|---:|---:|
| Logistic Regression | 93.37% | 93.23% |
| Tuned SVM | 93.34% | 93.19% |
| Tuned KNN | 93.21% | 93.06% |
| Tuned Random Forest | 93.18% | 93.03% |
| Decision Tree | 92.42% | 92.27% |

Logistic Regression achieved the highest reported cross-validation accuracy and was selected for the final evaluation.

---

## 🎯 6. Feature Selection & PCA

Feature selection experiments were conducted to investigate whether a smaller number of input features could provide competitive classification performance.

The final selected Logistic Regression model uses:

- Perimeter
- Major_Axis_Length

The reported 5-fold cross-validation accuracy of the selected two-feature model was 93.37%.

PCA was also investigated using three principal components, achieving approximately 93.18% cross-validation accuracy.

The experiments demonstrated that a reduced feature set could achieve competitive performance while simplifying the model inputs.

---

## ⚖️ 7. SMOTE & Feature Ablation Experiments

Additional experiments were conducted to investigate class imbalance and the contribution of individual features.

### SMOTE Experiment

SMOTE was evaluated to investigate whether synthetic oversampling improved classification performance.

| Metric | Without SMOTE | With SMOTE |
|---|---:|---:|
| CV Accuracy | 93.37% | 93.08% |
| Macro F1 | 93.23% | 92.96% |
| Balanced Accuracy | 93.19% | 93.14% |

The results showed that SMOTE did not improve overall accuracy or macro F1 for the current classification objective.

Therefore, the original no-SMOTE approach was retained.

### Feature Ablation

Feature ablation experiments were conducted to investigate the impact of removing selected features.

The experiments provided additional insights into feature contributions and the effects of correlated predictors.

---

## 📈 8. Final Model Evaluation

The selected Logistic Regression model was evaluated using the independent test dataset containing 762 observations.

### Final Independent Test Results

| Evaluation Metric | Result |
|---|---:|
| Accuracy | **91.21%** |
| Macro Precision | 91.23% |
| Macro Recall | 90.77% |
| Macro F1-score | **90.97%** |
| ROC-AUC | **97.39%** |

### Confusion Matrix

| Actual / Predicted | Cammeo | Osmancik |
|---|---:|---:|
| Cammeo | 286 | 40 |
| Osmancik | 27 | 409 |

The model correctly classified 695 out of 762 test observations.

These results represent the independent test performance and are separate from the cross-validation development results.

---

## 🏭 9. Quality Control Decision Support

A secondary objective of the project is to explore a QC decision-support approach.

The proposed workflow uses:

1. Input morphological measurements.
2. Predicted rice variety.
3. Estimated class probabilities.
4. Expected rice variety supplied for verification.
5. Comparison between expected and predicted varieties.

### Decision Logic

- **MATCH:** Predicted variety agrees with the expected variety.
- **MISMATCH:** Predicted variety differs from the expected variety and additional verification is recommended.

This approach is intended to support quality-control personnel during rice variety verification.

**Important:** MATCH/MISMATCH indicates agreement between the predicted and expected variety. It does not certify grain quality, purity, authenticity or food safety. Human verification remains necessary.

---

## 🛠️ 10. Technologies & Libraries

**Programming Language**
- Python

**Data Processing**
- Pandas
- NumPy

**Data Visualization**
- Matplotlib
- Seaborn

**Machine Learning**
- Scikit-learn
- Imbalanced-learn

**Development Environment**
- Jupyter Notebook

**Model Storage**
- Pickle

---

## 📁 11. Project Structure

```text
rice-variety-classification/
│
├── dataset/
│   └── Rice_Cammeo_Osmancik.csv
│
├── notebooks/
│   ├── 01_Data_Understanding.ipynb
│   ├── 02_EDA_Data_Quality_Preprocessing.ipynb
│   ├── 03_Feature_Selection_Model_Development.ipynb
│   ├── 04_Model_Evaluation_QC_Decision_Support.ipynb
│   ├── 05_SMOTE_Correlation_Feature_Ablation.ipynb
│   └── Feature Ablation Experiment.ipynb
│
├── models/
│   ├── rice_qc_model.pkl
│   ├── rice_logistic_regression.pkl
│   ├── rice_svm_model.pkl
│   └── selected_features.pkl
│
├── reports/
│   └── IT3091_Rice_QC_Final_Group_Report.docx
│
├── README.md
├── requirements.txt
└── .gitignore
```

*Note: The filenames above represent the intended cleaned repository structure. Keep only model files that you have verified and actually need. Rename files consistently if necessary.*

---

## 🚀 12. Installation & Setup

### Prerequisites

- Python 3.9 or later
- Jupyter Notebook
- Git

### Step 1: Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/rice-variety-classification.git
```

### Step 2: Navigate to the Project Directory

```bash
cd rice-variety-classification
```

### Step 3: Install Dependencies

```bash
pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn jupyter
```

### Step 4: Launch Jupyter Notebook

```bash
jupyter notebook
```

### Step 5: Run the Notebooks

Open the notebooks in the `notebooks/` directory and execute them in the following order:

1. Data Understanding
2. EDA, Data Quality and Preprocessing
3. Feature Selection and Model Development
4. Model Evaluation and QC Decision Support
5. SMOTE, Correlation and Feature Ablation

**Note:** Update dataset paths in the notebooks to point to `dataset/Rice_Cammeo_Osmancik.csv` when running them from the repository root. Some original notebooks use relative paths based on their original folder structure.

---

## ⚠️ 13. Limitations & Future Improvements

Although the project achieved promising classification performance, several improvements can be explored:

- Develop and test a complete Streamlit web application.
- Perform nested cross-validation to obtain a less biased estimate of model-selection performance.
- Investigate probability calibration.
- Evaluate model performance on additional rice datasets.
- Investigate the impact of different classification error costs.
- Improve model interpretability and visualization.
- Validate the QC workflow with real-world operational data.
- Add automated testing and model reproducibility checks.

---

## 👥 14. Project Information

**Module:** IT3091 – Machine Learning

**Project Type:** Group Assignment

**Project Domain:** Agriculture / Machine Learning / Quality Control

**Primary Task:** Binary Rice Variety Classification

**Target Classes:** Cammeo and Osmancik

---

## 📜 15. Disclaimer

This project was developed for academic and educational purposes as part of a Machine Learning group assignment.

The model predictions are intended as decision-support outputs and should not be treated as a replacement for professional quality-control inspection.

---

⭐ If you find this project interesting, feel free to explore the notebooks and experiments!
