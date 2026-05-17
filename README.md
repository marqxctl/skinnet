# SkinNet

```
Deep learning pipeline for dermoscopic skin lesion classification
using CNNs, transfer learning, fine-tuning and Grad-CAM interpretability.
```

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat&logo=tensorflow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-D00000?style=flat&logo=keras&logoColor=white)
![Colab](https://img.shields.io/badge/Google_Colab-F9AB00?style=flat&logo=googlecolab&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat&logo=opencv&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat&logo=python&logoColor=white)

---

## Overview

This repository implements a convolutional deep learning pipeline for binary classification of dermoscopic skin lesions, targeting the automated detection of potentially malignant lesions. The pipeline encompasses data preprocessing, class imbalance handling, multi-architecture training with transfer learning and fine-tuning strategies, and post-hoc explainability via Gradient-weighted Class Activation Mapping (Grad-CAM).

Four architectures were systematically evaluated: a custom baseline CNN, ResNet50 with frozen feature extraction, ResNet50 with selective layer fine-tuning, and EfficientNetB0 — all trained on the HAM10000 benchmark dataset.

---

## Dataset

**HAM10000 — Human Against Machine with 10000 Training Images**

> Multi-source dermatoscopic image collection for pigmented skin lesion analysis.

🔗 [Kaggle — Skin Cancer MNIST: HAM10000](https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000)

| Split | Samples |
|-------|---------|
| Train | 7,010 |
| Validation | 1,502 |
| Test | 1,503 |
| **Total** | **10,015** |

> All images resized to **224 × 224 px** (RGB). Class imbalance addressed via weighted loss during training.

---

## Classification Protocol

Binary grouping of the seven original HAM10000 diagnostic categories:

| Label | Lesion Types |
|-------|-------------|
| 🔴 **Suspicious** | Melanoma `mel` · Basal Cell Carcinoma `bcc` · Actinic Keratoses `akiec` |
| 🟢 **Non-suspicious** | Melanocytic Nevi `nv` · Benign Keratosis-like Lesions `bkl` · Vascular Lesions `vasc` · Dermatofibroma `df` |

---

## Results

### Model Comparison

![Models Comparison](results/models_comparison.png)

| Model | Accuracy | Precision | Recall | F1-score |
|-------|----------|-----------|--------|----------|
| CNN Baseline | 0.81 | — | — | — |
| ResNet50 Transfer Learning | 0.79 | 0.47 | 0.86 | 0.61 |
| **ResNet50 Fine-Tuned** | **0.86** | **0.60** | **0.77** | **0.68** |
| EfficientNetB0 | 0.77 | 0.45 | 0.79 | 0.58 |

> All metrics reported for the **suspicious class** (positive class). The CNN Baseline collapsed to the majority class, yielding zero precision and recall for suspicious lesions.

---

### Best Model — ResNet50 Fine-Tuned

#### Per-class Classification Report

| Class | Precision | Recall | F1-score | Support |
|-------|-----------|--------|----------|---------|
| Non-suspicious | 0.94 | 0.88 | 0.91 | 1,210 |
| Suspicious | 0.60 | 0.77 | 0.68 | 293 |
| Macro avg | 0.77 | 0.82 | 0.79 | 1,503 |
| Weighted avg | 0.87 | 0.86 | 0.86 | 1,503 |

#### Confusion Matrix

![Confusion Matrix](results/confusion_matrix_resnet50_finetuned.png)

> The model correctly identified **225 of 293 suspicious lesions** (recall = 0.77), with 68 false negatives — a critical metric in clinical screening contexts where missed malignancies carry high risk.

---

## Explainability — Grad-CAM

Gradient-weighted Class Activation Mapping (Grad-CAM) was integrated as a post-hoc interpretability module, producing class-discriminative saliency maps that localize the spatial regions driving each prediction. This provides visual evidence of model behavior aligned with clinically relevant lesion features, supporting trust and auditability in a medical imaging context.

> *Grad-CAM visualizations coming soon.*

---

## References

- Tschandl, P. et al. *The HAM10000 dataset, a large collection of multi-source dermatoscopic images of common pigmented skin lesions.* Scientific Data, 2018.
- Selvaraju, R. R. et al. *Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization.* ICCV, 2017.
- He, K. et al. *Deep Residual Learning for Image Recognition.* CVPR, 2016.
- Tan, M. & Le, Q. V. *EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks.* ICML, 2019.

---

## Author

**Lucas Marques**

---

## License

This project is licensed under the [MIT License](LICENSE).
