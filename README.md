# TB Treatment Outcome Prediction

Comparison of XGBoost, Random Forest, CatBoost and an artificial neural network (ANN) for classifying pulmonary tuberculosis (TB) treatment outcomes in Health Region 1, Chiang Mai.

**Paper:** "Comparative Analysis of Artificial Neural Networks and Machine Learning Models for Predicting Pulmonary Tuberculosis Treatment Outcomes"
12th National Conference on Science and Technology (KKR-NCST), Krung Kao Rajabhat University, 29–30 June 2026 — **First Prize, student oral presentation** (Engineering and Industrial Innovation / Computer and Digital Technology).

## Task
Binary classification of pulmonary TB treatment outcome, following WHO outcome definitions:
- **Class 0 (successful):** treatment completed, cured
- **Class 1 (unsuccessful):** died, lost to follow-up, diagnosis changed (not evaluated), treatment failed, RR/MDR before the fifth month

## Data (not included)
25,239 patient treatment records from the Office of Disease Control 1, Chiang Mai. The dataset contains sensitive health information and is **not included** in this repository. [Add: how access was obtained and who to contact.]

Input variables (17): gender, BMI, age, HIV status, chronic kidney disease, liver disease, antiretroviral therapy, diabetes mellitus, co-trimoxazole treatment, chronic obstructive pulmonary disease, ICD-10 disease group, patient registration type, TB follow-up status, TB cohort, first-month AFB result, site of tuberculosis, treatment duration.

## Method
1. Input: de-identified, already-encoded dataset; rows with missing values removed (1,479 of 25,239), leaving 23,760 records
2. Stratified 80/20 train/test split (random_state=42); test set: 4,659 records
3. Class imbalance: class weights for XGBoost, Random Forest and CatBoost; SMOTE on the training set for the ANN
4. Models: XGBoost, Random Forest, CatBoost, ANN
5. Hyperparameter tuning: Random Search (10 iterations, 3-fold cross-validation, macro F1) for XGBoost, Random Forest and CatBoost; hyperparameter search for the ANN
6. Evaluation: accuracy, macro-averaged precision / recall / F1, AUC, confusion matrices

## Results (test set, 4,659 records)
| Model | Accuracy | Macro F1 | AUC |
|---|---|---|---|
| XGBoost | 93.97% | 90.75% | 0.9518 |
| CatBoost | 93.88% | 90.64% | 0.9537 |
| Random Forest | 93.78% | 90.52% | 0.9475 |
| ANN | 92.53% | 88.91% | 0.9434 |

Precision, recall and F1 are macro averages. XGBoost had the best overall performance (macro precision 92.08%, macro recall 89.58%); CatBoost had the highest AUC. Across the models, treatment duration and TB follow-up status were the most important features. Full results are in the paper.

## Limitations
- Single-region dataset (Health Region 1, Chiang Mai); no external validation
- Treatment duration and TB follow-up status are recorded during treatment, so the models classify outcomes from treatment records rather than predicting them at the start of treatment
- Class 1 is the minority class (995 of 4,659 test records); results use a single stratified train/test split
- The grouping of outcomes into two classes (for example, counting "diagnosis changed" as unsuccessful) affects the results
- Hyperparameter search was limited to 10 random iterations per model
- Research prototype, not a clinical tool

## Repository contents
- `TB_Treatment_Outcome_Prediction.ipynb` — cleaning, train/test split, tuning, training and evaluation of all four models
- `images/` — result figures (confusion matrices, ROC curves; aggregate statistics only)

## How to run
The original dataset is not provided. To reproduce the pipeline, prepare an encoded table with the variables above and a `result` column (treatment outcome), name it `TB_Dataset_encode_clus.xlsx`, and run the notebook in Google Colab or Jupyter.

Main libraries: pandas, NumPy, scikit-learn, imbalanced-learn, XGBoost, CatBoost, TensorFlow/Keras, matplotlib, seaborn, openpyxl.

## My contribution
Cleaned the dataset (removed incomplete records), trained the Random Forest model, and tuned the hyperparameters of XGBoost, CatBoost and Random Forest with Random Search. The paper and slides were prepared by the whole team.

## Authors
Siriprapa Tewee, Tanaporn Chaisinratham, Lawitra Phanpanich, Siriyakon Saenpor, Apichaya Buajee, Chalermrat Nontapa
Data Science Research Center, Faculty of Science, Chiang Mai University
