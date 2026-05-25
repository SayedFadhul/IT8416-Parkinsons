# Parkinson's Disease Detection

Parkinson's Disease Detection is a data mining project built in AI Studio 2026 for the IT8416 Data Mining. The goal is to classify whether a patient has Parkinson's Disease based on biomedical voice measurements using the UCI Oxford Parkinsons dataset.

Three classification models were built and evaluated:

- Decision Tree
- K-Nearest Neighbour (K-NN)
- Ensemble (Vote: Decision Tree + k-NN)

---

## Programs Used

Install these on your machine:

- **Altair AI Studio 2026** 
- A web browser such as **Chrome**, **Edge**, or **Brave**

---

## Dataset

- **Source**: UCI Machine Learning Repository — Oxford Parkinson's Disease Detection Dataset
- **File**: `parkinsons.rmhdf5table` (loaded in AI Studio as `//Local Repository/parkinsons`)
- **Instances**: 195 voice recordings from 31 people (23 with Parkinson's Disease)
- **Attributes**: 24 (name + 22 biomedical voice features + status)
- **Target**: `status` — 0 = Healthy, 1 = Parkinson's Disease
- **Missing values**: None

---

## Setup and Run Guide

### 1. Get the Project

- Download the repository as a ZIP from GitHub and extract it, **or**
- Clone it:

```bash
git clone <REPO_URL>
```

---

### 2. Open the Project in Altair AI Studio 2026

1. Open **Altair AI Studio 2026**.
2. In the Repository panel on the left, click **Add Repository**.
3. Select **Local Repository** and point it to the extracted project folder.
4. The repository will appear as `//Local Repository/` in the panel.
5. All process files will be visible under `processes/`.

---

### 3. Load the Dataset

The dataset is pre-loaded as `parkinsons.rmhdf5table` in the root of the repository.

It will be accessible inside any process as:

```text
//Local Repository/parkinsons
```

No additional import steps are needed.

---

### 4. Run a Process

1. In the Repository panel, expand `processes/`.
2. Double-click any `.rmp` file to open it in the Design canvas.
3. Click the **Run** button (blue play triangle) at the top.
4. Results will appear in the **Results** view.

---

## Process Files

All processes follow the same preprocessing pipeline before modelling:

```
Retrieve parkinsons
  → Select Attributes (exclude name)
  → Numerical to Polynominal (status)
  → Set Role (status = label)
  → Replace Missing Values (average/mode)
  → Detect Outlier (Distances, k=10, top 10)
  → Filter Examples (outlier = false)
  → Normalize (Z-transformation)
```

| File | Purpose |
|---|---|
| `parkinsons_initial_data_viz.rmp` | Correlation Matrix on raw data |
| `parkinsons_feature_importance.rmp` | Feature importance using Weight by Information Gain Ratio |
| `parkinsons_DT_split60.rmp` | Decision Tree with 60/40 split |
| `parkinsons_DecisionTree.rmp` | Decision Tree with 70/30 stratified split |
| `parkinsons_DT_split80.rmp` | Decision Tree with 80/20 split |
| `parkinsons_KNN.rmp` | k-NN (k=5, weighted) with 70/30 stratified split |
| `parkinsons_Ensemble.rmp` | Vote (Decision Tree + k-NN) with PCA, 70/30 stratified split |

---

## Model Parameters

**Decision Tree**
- Criterion: Gain Ratio
- Maximal depth: 10
- Pruning confidence: 0.1
- Minimal leaf size: 2
- Minimal size for split: 4

**k-NN**
- k: 5
- Weighted vote: true
- Distance measure: MixedEuclideanDistance

**Ensemble**
- Operator: Vote (Decision Tree + k-NN)
- PCA applied before validation (variance threshold: 0.95 → 8 components)
- Split: 70/30 stratified

---

## Results

All models were evaluated on a 30% stratified holdout set (56 instances).

| Metric | Decision Tree | k-NN | Ensemble |
|---|---|---|---|
| Accuracy | 83.93% | 92.86% | 85.71% |
| Precision (PD) | 83.67% | 91.30% | 84.00% |
| Recall (PD) | 97.62% | 100.00% | 100.00% |
| F1 (PD) | 90.07% | 95.45% | 91.30% |
| AUC | 0.873 | 1.000 | 0.850 |

**Best model: k-NN** — highest accuracy, perfect recall (zero missed PD cases), and perfect AUC.

---
