# AdClassifier Pro 🚀

> **Digital Advertisement Classification System Using Machine Learning**  
> AdClassifier Pro is a machine-learning-based text classification system that automatically classifies digital advertisements into predefined categories using Natural Language Processing (NLP) and machine learning.

The project was developed as part of an MSc Computer Science dissertation.

---



## Project Overview

The system analyses the textual content of a digital advertisement and predicts its most appropriate category.

The project includes:

- Digital advertisement text classification
- TF-IDF feature extraction
- Comparison of four machine learning algorithms
- Automatic selection of the best-performing model
- Single advertisement classification
- Batch CSV and Excel classification
- Prediction confidence information
- Model performance visualisations
- Interactive Streamlit web interface

The final selected classifier is **LinearSVC**, which achieved approximately **97.33% test accuracy**.

---

## Advertisement Categories

The system classifies advertisements into five categories:

| Category | Description |
|---|---|
| `Banking` | Banking, loans, credit and financial-service advertisements |
| `Jobs – IT` | Software development, cybersecurity, cloud, DevOps and other IT jobs |
| `Jobs – Retail` | Cashier, sales assistant, store manager and other retail jobs |
| `Rent – Apartment` | Apartments and residential properties available for rent |
| `Sell – House` | Houses and residential properties advertised for sale |

---

## Dataset

The final dataset contains **2,805 advertisement records** after data preparation and synthetic data augmentation.

### Final Class Distribution

```text
Sell – House        607
Jobs – Retail       552
Rent – Apartment    550
Jobs – IT           549
Banking             547

---

## Algorithm Comparison

Four algorithms were trained and evaluated on identical data splits. Results are ranked by **Test Accuracy**:

| Rank | Algorithm | Test Accuracy | Macro F1 | Weighted F1 | CV Mean (5-fold) | CV Std | Train Time |
|---|---|---|---|---|---|---|---|
| 🥇 1 | **LinearSVC** *(Final Model)* | **97.33%** | **97.28%** | **97.32%** | **97.61%** | ±0.47% | 1.59s |
| 🥈 2 | Logistic Regression | 96.97% | 96.92% | 96.97% | 97.58% | ±0.57% | 1.31s |
| 🥈 2 | Random Forest | 96.97% | 96.94% | 96.98% | 96.26% | ±0.77% | 1.60s |
| 4 | Multinomial Naive Bayes | 96.61% | 96.55% | 96.62% | 96.97% | ±0.61% | 0.65s |

> All algorithms use TF-IDF-based textual features for advertisement classification.


---

## Why Each Algorithm?

### ⚔️ LinearSVC *(Final Selected Model)*

**Why chosen:** LinearSVC is well suited to high-dimensional sparse text representations such as TF-IDF. It achieved the strongest overall performance in the final experiment.

`CalibratedClassifierCV` is used with LinearSVC so that confidence information can also be provided for predictions.

Final performance:

- Test Accuracy: **97.33%**
- Macro F1-score: **97.28%**
- Weighted F1-score: **97.32%**
- 5-Fold CV Mean: **97.61%**

---

### 📈 Logistic Regression

**Why included:** Logistic Regression provides a strong linear baseline for text classification and works effectively with TF-IDF features.

It achieved:

- Test Accuracy: **96.97%**
- Macro F1-score: **96.92%**
- 5-Fold CV Mean: **97.58%**

Its performance was very close to LinearSVC.

---


### 🧮 Multinomial Naive Bayes

**Why included:** Multinomial Naive Bayes is a traditional probabilistic algorithm commonly used for text classification.

It is computationally efficient and provides a useful baseline when working with TF-IDF-based textual features.

It achieved:

- Test Accuracy: **96.61%**
- Macro F1-score: **96.55%**
- 5-Fold CV Mean: **96.97%**

---


### 🌲 Random Forest

**Why included:** Random Forest is an ensemble learning algorithm that combines predictions from multiple decision trees.

It was included to compare an ensemble-based method against the linear and probabilistic classifiers.

It achieved:

- Test Accuracy: **96.97%**
- Macro F1-score: **96.94%**
- 5-Fold CV Mean: **96.26%**

Although its test accuracy was high, its cross-validation performance was lower than LinearSVC and Logistic Regression.

---

## Project Structure

```text
Digital-Advertisement-Classification/
│
├── streamlit_app.py          # Main Streamlit web application
├── retrain_model.py          # Generates synthetic data & retrains LinearSVC
├── compare_algorithms.py     # Trains & compares all 4 algorithms
├── requirements.txt          # Python dependencies
│
├── data/
│   ├── ConcatenatedDigitalAdData.xlsx  # Original labelled dataset
│   ├── synthetic_data.csv              # Augmented synthetic training samples
│   └── comparison_report/              # Auto-generated comparison results
│       ├── algorithm_comparison.csv
│       ├── algorithm_reasons.txt
│       ├── accuracy_comparison.png
│       ├── f1_comparison.png
│       ├── training_time.png
│       ├── radar_comparison.png
│       ├── cm_LinearSVC_Current.png
│       └── cm_*.png
│
└── notebook/
    ├── JobModelFinal.ipynb        # Final model development notebook
    │
    └── model/
        ├── adv_model.sav          # Final selected model
        └── adv_model_backup.sav   # Previous model backup

---

## Setup & Installation

**Requirements:** Python 3.9+

```bash
# 1. Clone the repository
git clone https://github.com/aashiqchalise/Digital-Advertisement-Classification.git
cd Digital-Advertisement-Classification

# 2. Create and activate virtual environment
python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate     # Mac/Linux

# 3. Install dependencies
pip install -r requirements.txt
```

`requirements.txt` includes:
```
streamlit
scikit-learn
pandas
numpy
openpyxl
matplotlib
seaborn
```

---

## Running the App

```bash
# Start the Streamlit dashboard
streamlit run streamlit_app.py
```

The app opens at `http://localhost:8501` with 4 pages:

| Page | Description |
|---|---|
| **Dashboard** | Data distribution, total samples, category counts |
| **Classifier** | Real-time single-ad classification with confidence scores |
| **Batch Processor** | Upload CSV/Excel for bulk classification and download |
| **Model Comparison** | Interactive charts and rankings for all 4 algorithms |

---

## Retraining the Model

### Retrain with synthetic data augmentation :
```bash
python retrain_model.py
```

### Run full multi-algorithm comparison + save best model:
```bash
python compare_algorithms.py
```

This will:
1. Train all 4 algorithms on the same data split
2. Print a full comparison table to the console
3. Save charts and CSVs to `data/comparison_report/`
4. Automatically save the **best model** to `notebook/model/adv_model.sav`
5. Back up the previous model to `adv_model_backup.sav`

After running, restart the Streamlit app — it will load the new best model automatically.

---

## Results Summary

The final system achieved strong classification performance across the four evaluated machine learning algorithms:

- **Best Model:** LinearSVC + TF-IDF
- **Test Accuracy:** 97.33%
- **Macro Precision:** 97.35%
- **Macro Recall:** 97.27%
- **Macro F1-score:** 97.28%
- **Weighted F1-score:** 97.32%
- **5-Fold CV Accuracy:** 97.61% (±0.47%)
- **Dataset Size:** 2,805 advertisements
- **Training Samples:** 2,244
- **Testing Samples:** 561
- **Categories:** 5
- **Algorithms Compared:** 4

> The results show that LinearSVC achieved the strongest overall performance among the four evaluated classifiers, with Logistic Regression producing very similar results. Multinomial Naive Bayes also achieved strong performance with relatively low computational requirements, while Random Forest produced a lower mean cross-validation score than the linear classifiers.

---

*Built with Python · scikit-learn · Streamlit · TF-IDF · LinearSVC*