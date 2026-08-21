# To Remove or Not to Remove Clouds: Multi-View Overcast Flood Mapping

This repository contains the official code, experimental workflows, and evaluation notebooks for our study on overcast water body segmentation using Synthetic Aperture Radar (SAR) and Synthetic Normalized Difference Water Index (NDWI) proxies.

---

## 📂 Repository Structure & Notebook Overview

The experiments are organized across six primary Jupyter notebooks, categorized by cross-validation scheme, input feature configurations (including Principal Component Analysis), and specific ablation studies:

| Notebook File | Description & Purpose |
| :--- | :--- |
| **`event-stratified.ipynb`** | Evaluates core baseline modalities (**S1 Only**, **Syn S2 Only**, and **Combined**) under an **Event-Stratified Cross-Validation** split to test model generalization on unseen geographic flood events. |
| **`mixed_events.ipynb`** | Evaluates the core baseline modalities (**S1 Only**, **Syn S2 Only**, and **Combined**) under a **Mixed-Events Cross-Validation** framework (random/spatial tile splits across all flood regions). |
| **`mixed_events_testing_effect_of_R2_score.ipynb`** | Quantitative analysis measuring the correlation between intermediate regression quality ($R^2$ score of synthetic NDWI vs. optical ground truth) and downstream water segmentation performance ($IoU$ / $F1$). |
| **`other_configs_event_stratified.ipynb`** | Explores alternative input feature configurations—including **PCA-transformed S1 features**, **Synthetic S2**, and **PCA + Raw S1 feature combinations**—under the **Event-Stratified** CV setup. |
| **`other_configs_mixed_events.ipynb`** | Evaluates alternative input feature configurations (**PCA of S1**, **Synthetic S2**, and **PCA + Raw S1 combinations**) under the **Mixed-Events** CV setup. |
| **`training_models_from_scratch.ipynb`** | Ablation study comparing custom architectures trained from scratch against pretrained (transfer learning) backbones for the SAR-to-NDWI translation phase. |

---

## 🧪 Key Experiments & Workflows

### 1. Comparative Evaluation (S1 vs. Synthetic NDWI vs. Combined)
Evaluates three primary input strategies for resolving overcast segmentation:
* **`S1 Only`**: Directly segments water from raw, noisy Sentinel-1 radar backscatter.
* **`Syn S2 Only`**: Segments water using the synthetic NDWI proxy translated from SAR.
* **`Combined`**: Fuses the structural boundaries of raw SAR with the high contrast of synthetic NDWI.

### 2. Dimensionality Reduction & Feature Engineering (`other_configs_*.ipynb`)
Investigates advanced input feature combinations across both cross-validation schemes:
* **PCA of S1**: Applies Principal Component Analysis on SAR channels to compress noise and extract dominant backscatter components.
* **PCA + Raw S1**: Fuses decomposed principal components with raw SAR signals to test whether explicit feature decorrelation improves downstream segmentation.
* **Synthetic S2 Formulations**: Compares single-index proxies against multi-channel optical representations.

### 3. Intermediate $R^2$ vs. Downstream Accuracy Analysis
Located in `mixed_events_testing_effect_of_R2_score.ipynb`, this experiment proves that higher SAR-to-NDWI translation quality ($R^2$) directly improves downstream segmentation accuracy ($IoU$).

### 4. Pretrained Backbones vs. From-Scratch Models
Located in `training_models_from_scratch.ipynb`, demonstrating that fine-tuning ImageNet-pretrained backbones yields superior translation quality compared to training custom architectures from scratch.

---

## ⚙️ Environment & Setup

* **Frameworks**: PyTorch, Segmentation Models PyTorch (`smp`), Weights & Biases (`wandb`).
* **Tracking**: All experiments log directly to **Weights & Biases**. Configure your `WANDB_API_KEY` in environment variables or Kaggle User Secrets before running the notebooks.
