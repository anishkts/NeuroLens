

# NeuroLens

**Accessible & Explainable Early Alzheimer’s Detection via Ensemble Deep Learning**

**Category:** Healthcare / AI
**Team:** Supreme

---

## Overview

NeuroLens is a deep learning–based system for early-stage Alzheimer’s disease classification from brain MRI scans. The project focuses on addressing two major weaknesses in medical AI systems: **class imbalance** and **lack of explainability**.
Instead of optimizing for headline accuracy, NeuroLens prioritizes **sensitivity to rare dementia cases** and provides **visual explanations** to support clinical trust.

---

## Problem Statement

### Clinical Challenge

Alzheimer’s disease affects over 50 million people worldwide, yet early diagnosis remains difficult. In early stages (e.g., *Very Mild Demented*), structural brain changes are subtle and easily missed.
Medical datasets are also highly imbalanced, causing many AI models to default to predicting “Normal,” resulting in misleadingly high accuracy but poor clinical usefulness.

### Accessibility & Trust Gap

Most medical AI systems require cloud-scale GPUs and operate as black boxes. This raises:

* Data privacy concerns
* Deployment barriers in low-resource settings
* Low clinician trust due to lack of interpretability

---

## Project Objective

NeuroLens aims to build an AI system that:

* **Prioritizes sensitivity** for early-stage dementia detection
* **Improves reliability** through ensemble learning
* **Builds trust** using explainable AI (Grad-CAM)

---

## Methodology

### Dataset

MRI scans categorized into four classes:

* Mild Demented
* Moderate Demented
* Non Demented
* Very Mild Demented

Images are stored as raw bytes in **Parquet format** and decoded during training.

---

### Data Preprocessing

* Image resizing to 224×224
* Grayscale conversion → 3 channels
* Normalization using ImageNet statistics
* Controlled augmentation:

  * Random rotation
  * Horizontal flipping

---

### Model Architecture

* **Backbone:** ResNet-18 (ImageNet pretrained)
* **Training Strategy:** Transfer learning with partial layer freezing
* **Loss Function:** Weighted Cross-Entropy to address class imbalance

**Class Weights:**

* Mild: 1.0
* Moderate: 2.0
* Normal: 1.2
* Very Mild: 0.8

---

### Ensemble Learning

To reduce variance and improve robustness:

* Three ResNet-18 models trained with different random seeds
* Predictions combined using **soft voting (averaged logits)**

---

### Explainable AI (XAI)

Grad-CAM is applied to the final convolutional layer to visualize:

* Brain regions influencing predictions
* Alignment with known Alzheimer’s biomarkers (e.g., ventricular enlargement)

---

## Evaluation & Results

### Quantitative Performance

| Model        | Accuracy | Moderate F1 | Observation                 |
| ------------ | -------- | ----------- | --------------------------- |
| Baseline     | ~70%     | 0.00        | Collapsed to “Normal”       |
| Single Model | 89%      | 0.93        | Improved recall             |
| **Ensemble** | **93%**  | **1.00**    | Strong rare-class detection |

**Key Insight:**
The ensemble achieved perfect precision and recall for the *Moderate Demented* class despite limited samples.

---

### ROC-AUC Analysis

* One-vs-rest ROC curves
* AUC > 0.95 across all classes
* Indicates strong class separation and high diagnostic confidence

---

### Qualitative Validation

Grad-CAM visualizations confirm attention on:

* Lateral ventricles
* Cortical gray matter

These regions are consistent with known Alzheimer’s pathology, indicating the model learns meaningful features rather than artifacts.

---



## Challenges Faced

* Severe class imbalance causing misleading accuracy
* Non-standard data format (Parquet-based MRI storage)
* Avoiding evaluation traps that hide rare-class failure
* Integrating explainability without breaking training stability

---

## Key Learnings

* Accuracy alone is insufficient for medical AI
* Loss design is as important as model architecture
* Explainability is essential for clinical relevance
* Ensembles can outperform larger single models when data is limited

---

## Future Work

* Multimodal fusion with clinical metadata (age, genetics)
* 3D volumetric CNNs for spatial brain analysis
* Further validation on larger and more diverse datasets

---

## Disclaimer

This project is a **research prototype** and is **not intended for clinical diagnosis**.


