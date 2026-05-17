# SkinNet
A convolutional deep learning pipeline for binary classification of dermoscopic skin lesions, combining transfer learning, selective fine-tuning, and Grad-CAM post-hoc interpretability on the HAM10000 benchmark.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat&logo=tensorflow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-D00000?style=flat&logo=keras&logoColor=white)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1pOvXFliaCLQAsyMYchAc5Mpw3nCgjMx7?usp=sharing)

---

## Dataset

**HAM10000** — 10,015 dermoscopic images spanning 7 diagnostic categories, partitioned into training (7,010), validation (1,502), and test (1,503) subsets. All images were resized to 224 × 224 pixels. Class imbalance was addressed via cost-sensitive learning through weighted loss functions.

🔗 [Tschandl et al., Scientific Data, 2018](https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000)

---

## Classification Protocol

The seven HAM10000 diagnostic categories were aggregated into two clinically motivated classes:

| Label | Categories |
|-------|-----------|
| **Suspicious** | Melanoma `mel` · Basal Cell Carcinoma `bcc` · Actinic Keratoses `akiec` |
| **Non-suspicious** | Melanocytic Nevi `nv` · Benign Keratosis-like Lesions `bkl` · Vascular Lesions `vasc` · Dermatofibroma `df` |

---

## Results

![Models Comparison](results/models_comparison.png)

| Model | Accuracy | Precision | Recall | F1 |
|-------|----------|-----------|--------|----|
| CNN Baseline | 0.81 | 0.00 | 0.00 | 0.00 |
| ResNet50 Transfer Learning | 0.79 | 0.47 | 0.86 | 0.61 |
| **ResNet50 Fine-Tuned** | **0.86** | **0.60** | **0.77** | **0.68** |
| EfficientNetB0 | 0.77 | 0.45 | 0.79 | 0.58 |

> Metrics reported for the suspicious (positive) class. The CNN Baseline collapsed to the majority class, yielding zero discriminative capacity for the minority class.

#### ResNet50 Fine-Tuned — Per-class Classification Report

| Class | Precision | Recall | F1 | Support |
|-------|-----------|--------|----|---------|
| Non-suspicious | 0.94 | 0.88 | 0.91 | 1,210 |
| Suspicious | 0.60 | 0.77 | 0.68 | 293 |
| Macro avg | 0.77 | 0.82 | 0.79 | 1,503 |

![Confusion Matrix](results/confusion_matrix_resnet50_finetuned.png)

> 225 of 293 suspicious lesions were correctly identified (recall = 0.77), yielding 68 false negatives. In clinical screening contexts, false negatives carry significant risk due to missed malignancy detection.

---

## Explainability

Gradient-weighted Class Activation Mapping (Grad-CAM) was applied to generate class-discriminative saliency maps, localizing the spatial regions most influential to each prediction. This technique supports post-hoc model auditability and interpretability in medical imaging contexts, providing a mechanism to assess whether model attention aligns with clinically relevant morphological features.

| | |
|---|---|
| ![True Positive](results/gradcam_examples/true_positive.png) | ![True Negative](results/gradcam_examples/true_negative.png) |
| **True Positive** — activation concentrated over the lesion body, consistent with clinically relevant morphological features | **True Negative** — low activation across the lesion region, correctly attributed to a non-suspicious pattern |
| ![False Positive](results/gradcam_examples/false_positive.png) | ![False Negative](results/gradcam_examples/false_negative.png) |
| **False Positive** — saliency displaced outside the lesion boundary, indicating spurious feature activation driving misclassification | **False Negative** — diffuse and spatially unfocused activation, reflecting insufficient discriminative signal for malignancy detection |

---

## Disclaimer

This project is intended for academic and research purposes only. It is not a medical device and must not be used as a substitute for professional clinical diagnosis.

---

## References

- Tschandl, P. et al. *HAM10000 dataset.* Scientific Data, 2018.
- Selvaraju, R. R. et al. *Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization.* ICCV, 2017.
- He, K. et al. *Deep Residual Learning for Image Recognition.* CVPR, 2016.
- Tan, M. & Le, Q. V. *EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks.* ICML, 2019.

---

## Author

**Lucas Marques**
Software Engineer · Software Development · Database Administration

## License

[MIT](LICENSE)
