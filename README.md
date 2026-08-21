# To Remove or Not to Remove Clouds: Multi-View Overcast Flood Mapping

This repository contains the official implementation, experimental notebooks, and ablation evaluations for our paper on overcast water body segmentation using Synthetic Aperture Radar (SAR) and Synthetic Normalized Difference Water Index (NDWI) proxies.

---

## 📂 Repository Structure & Notebook Overview

The core experiments are organized across six primary Jupyter notebooks, categorized by cross-validation strategy, model capacity, and specific ablation studies:

| Notebook File | Description & Purpose |
| :--- | :--- |
| **`event-stratified.ipynb`** | Implements the primary baseline and multi-view evaluation under an **Event-Stratified Cross-Validation** split (testing generalization across completely unseen flood events). Evaluates S1 Only, Synthetic NDWI Only, and Combined frameworks. |
| **`mixed_events.ipynb`** | Evaluates performance under a **Mixed-Events Cross-Validation** framework (random/spatial tile splits across all flood regions) to establish comparative performance metrics against event-stratified setups. |
| **`mixed_events_testing_effect_of_R2_score.ipynb`** | Performs a quantitative analysis testing the relationship between intermediate regression translation quality ($R^2$ score of synthetic NDWI vs. cloud-free ground truth) and downstream segmentation accuracy ($IoU$ / $F1$). |
| **`other_configs_event_stratified.ipynb`** | Explores structural capacity variations (e.g., `Reg-B0/Seg-B0`, `Reg-B3/Seg-B0`, `Reg-B5/Seg-B0`) and input tile dimensions under the **Event-Stratified** evaluation framework. |
| **`other_configs_mixed_events.ipynb`** | Explores architectural capacity scaling and tile size comparisons ($128 \times 128$ vs. $256 \times 256$) under the **Mixed-Events** cross-validation scheme. |
| **`training_models_from_scratch.ipynb`** | Ablation study comparing custom/from-scratch regression backbones against transfer learning-based (ImageNet pretrained) models for the SAR-to-NDWI translation task. |

---

## 🧪 Key Experiments & Workflows

### 1. Comparative Evaluation (S1 vs. Synthetic NDWI vs. Combined)
Across the notebooks, three input modalities are evaluated to resolve the translation dilemma:
* **`S1 Only`**: Directly segments water features from raw, noisy Sentinel-1 radar backscatter.
* **`Syn S2 Only`**: Segments water features using the synthetic NDWI proxy translated from SAR.
* **`Combined`**: Our proposed multi-view framework that fuses the sharp structural boundaries of raw SAR with the high contrast of synthetic NDWI.

### 2. Intermediate $R^2$ vs. Downstream Accuracy Analysis
Located in `mixed_events_testing_effect_of_R2_score.ipynb`, this experiment establishes that higher SAR-to-NDWI reconstruction quality ($R^2$) strongly correlates with higher downstream segmentation performance, demonstrating that cross-modal translation acts as an effective structural noise filter.

### 3. Backbone Capacity & Tile Size Ablation
Explored in `other_configs_*.ipynb`, we systematically vary the regression (`Reg-X`) and segmentation (`Seg-Y`) backbones across $128 \times 128$ and $256 \times 256$ tile resolutions to identify optimal trade-offs between computational overhead and segmentation accuracy.

### 4. Transfer Learning vs. Scratch Architectures
Evaluated in `training_models_from_scratch.ipynb`, demonstrating that fine-tuning pretrained backbones significantly outperforms training models from scratch for SAR-to-optical mapping.

---

## ⚙️ Environment & Setup

* **Frameworks**: PyTorch, Segmentation Models Pytorch (`smp`), Weights & Biases (`wandb`).
* **Tracking**: Experiments are logged to **Weights & Biases**. Ensure your `WANDB_API_KEY` is configured via Kaggle User Secrets or environment variables prior to running the notebooks.
