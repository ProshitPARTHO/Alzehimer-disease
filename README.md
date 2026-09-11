# EEG-Based Alzheimer's Disease Classification

A machine learning pipeline for classifying Alzheimer's Disease (AD), Mild Cognitive Impairment (MCI), and Normal Control (CT) subjects using EEG signal analysis and FFT-based feature extraction.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Dataset](#-dataset)
- [Project Structure](#-project-structure)
- [Requirements](#-requirements)
- [Installation](#-installation)
- [Usage](#-usage)
- [Methodology](#-methodology)
- [Results](#-results)
- [Project Pipeline](#-project-pipeline)


---

## 🧠 Overview

This project implements a complete pipeline for EEG-based classification of Alzheimer's Disease stages. It uses **Fast Fourier Transform (FFT)** features extracted from multi-channel EEG recordings and applies classical machine learning classifiers (SVM and Decision Tree) with **Leave-One-Out Cross-Validation (LOOCV)**.

The goal is to distinguish between three clinical groups:
- **AD** — Alzheimer's Disease
- **MCI** — Mild Cognitive Impairment
- **CT** — Normal Control

---

## 📊 Dataset

The project expects three MATLAB (`.mat`) files placed in the working directory:

| File | Description | Subjects |
|------|-------------|----------|
| `AD.mat` | Alzheimer's Disease subjects | 13 |
| `MCI.mat` | Mild Cognitive Impairment subjects | 7 |
| `normal.mat` | Healthy control subjects | 15 |

**Data structure per subject:**
- `epoch` — EEG signal array of shape `(4, 600, N_trials)`
  - 4 EEG channels: `Fp1`, `Fz`, `Cz`, `Pz`
  - 600 time samples (sampling rate = 200 Hz, from −1s to +2s)
  - N trials per subject
- `odor` — Odor stimulus information
- `noisy` — Indices of noisy trials to be removed

---

## 📁 Project Structure

```
.
├── alzehimerDisease.ipynb    # Main notebook with the full pipeline
├── AD.mat                    # Alzheimer's Disease data
├── MCI.mat                   # Mild Cognitive Impairment data
├── normal.mat                # Healthy control data
└── README.md                 # This file
```

---

## ⚙️ Requirements

- Python 3.8+
- Jupyter Notebook / Google Colab

### Python Libraries

```
numpy
scipy
matplotlib
seaborn
scikit-learn
pandas
```

---

## 🚀 Installation

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```

2. **Install dependencies:**
   ```bash
   pip install numpy scipy matplotlib seaborn scikit-learn pandas
   ```

3. **Place the `.mat` data files** (`AD.mat`, `MCI.mat`, `normal.mat`) in the project root directory.

4. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook alzehimerDisease.ipynb
   ```

---

## 🧪 Usage

Open the notebook and run the cells sequentially. The pipeline includes:

1. **Data Loading** — Load EEG data from `.mat` files.
2. **Feature Extraction** — Compute FFT-based features per channel per trial.
3. **Labeling & Concatenation** — Combine subjects into a unified feature matrix.
4. **Normalization** — Standardize features using `StandardScaler`.
5. **Visualization** — Signal plots, PSD, band-power distributions.
6. **Classification** — SVM and Decision Tree with LOOCV.
7. **Evaluation** — Accuracy, sensitivity, specificity, confusion matrices.
8. **Model Interpretation** — Decision tree visualization.

---

## 🔬 Methodology

### Feature Extraction

For each subject:
1. Remove **noisy trials** using the `noisy` index array.
2. For each of the 4 channels:
   - Compute FFT of each clean trial.
   - Keep the first **30 frequency bins** (low-frequency components).
   - Average across all clean trials.
3. Concatenate channel features → **120-dimensional feature vector** per subject.

### Classifiers

| Classifier | Configuration |
|------------|---------------|
| **SVM** | Polynomial kernel, degree = 3, C = 1 |
| **Decision Tree** | Entropy criterion (C4.5-like), unlimited depth (and depth=3 for visualization) |

### Validation

**Leave-One-Out Cross-Validation (LOOCV)** — ideal for small datasets. Each sample is used once as the test set while the rest serve as training data.

### Metrics

- Accuracy
- Sensitivity (True Positive Rate)
- Specificity (True Negative Rate)
- Confusion Matrix

---

## 📈 Results

### Pairwise Classification (LOOCV)

| Task | Model | Accuracy | Sensitivity | Specificity |
|------|-------|----------|-------------|-------------|
| AD vs CT | SVM (poly, deg=3) | 0.571 | 1.00 | 0.077 |
| AD vs CT | Decision Tree | 0.286 | 0.400 | 0.154 |
| MCI vs CT | SVM | 0.409 | 0.600 | 0.000 |
| MCI vs CT | Decision Tree | **0.909** | 0.867 | 1.000 |
| AD vs MCI | SVM | 0.650 | 0.000 | 1.000 |
| AD vs MCI | Decision Tree | **0.850** | 0.714 | 0.923 |

### Key Observations

- **Decision Tree** performs best on MCI vs CT and AD vs MCI.
- **SVM** performs poorly on specificity for AD vs CT.
- The AD vs CT classification remains the hardest task, likely due to high inter-subject variability and small sample size.

---

## 🔄 Project Pipeline

```
Raw EEG (.mat files)
        │
        ▼
  Remove Noisy Trials
        │
        ▼
   FFT Feature Extraction (30 bins × 4 channels = 120 features)
        │
        ▼
  Combine AD + MCI + CT  →  X (35 × 120), y (35,)
        │
        ▼
   StandardScaler Normalization
        │
        ▼
  ┌──────────────┬──────────────┐
  │  SVM (poly)  │   Decision   │
  │  LOOCV       │   Tree LOOCV │
  └──────────────┴──────────────┘
        │
        ▼
  Accuracy / Sensitivity / Specificity / Confusion Matrix
        │
        ▼
  Decision Tree Visualization (depth=3)
```

---

## ⚠️ Limitations & Future Work

### Limitations

- **Small sample size** (only 35 subjects) limits generalization.
- Simple FFT features may not capture non-stationary EEG dynamics.
- Only 4 channels used — spatial information is limited.
- No hyperparameter tuning reported.




## 📚 References

- Welch, P. (1967). *The use of fast Fourier transform for the estimation of power spectra.*
- Delorme, A., & Makeig, S. (2004). *EEGLAB: an open source toolbox for analysis of single-trial EEG dynamics.*
- Scikit-learn: [https://scikit-learn.org](https://scikit-learn.org)
- SciPy: [https://scipy.org](https://scipy.org)

---


---

*For questions or collaboration, please open an issue or contact the author.*
