# SkinNet

A convolutional deep learning pipeline for binary classification of dermoscopic skin lesions, combining transfer learning, selective fine-tuning, and Grad-CAM post-hoc interpretability on the HAM10000 benchmark.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat&logo=tensorflow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-D00000?style=flat&logo=keras&logoColor=white)
![Colab](https://img.shields.io/badge/Google_Colab-F9AB00?style=flat&logo=googlecolab&logoColor=white)

---

## Dataset

**HAM10000** — 10,015 dermoscopic images across 7 diagnostic categories, split into train (7,010), validation (1,502), and test (1,503) sets. All images resized to 224 × 224 px. Class imbalance addressed via weighted loss.

🔗 [Tschandl et al., Scientific Data, 2018](https://www.kaggle.com/datasets/kmader/skin-cancer-mnist-ham10000)

---

## Classification Protocol

Seven HAM10000 categories grouped into two classes:

| Label | Categories |
|-------|-----------|
| **Suspicious** | Melanoma `mel` · Basal Cell Carcinoma `bcc` · Actinic Keratoses `akiec` |
| **Non-suspicious** | Melanocytic Nevi `nv` · Benign Keratosis-like Lesions `bkl` · Vascular Lesions `vasc` · Dermatofibroma `df` |

---

## Results

![Models Comparison](results/models_comparison.png)

| Model | Accuracy | Precision | Recall | F1 |
|-------|----------|-----------|--------|----|
| CNN Baseline | 0.81 | — | — | — |
| ResNet50 Transfer Learning | 0.79 | 0.47 | 0.86 | 0.61 |
| **ResNet50 Fine-Tuned** | **0.86** | **0.60** | **0.77** | **0.68** |
| EfficientNetB0 | 0.77 | 0.45 | 0.79 | 0.58 |

> Metrics for the suspicious class (positive). CNN Baseline collapsed to the majority class.

#### ResNet50 Fine-Tuned — Per-class Report

| Class | Precision | Recall | F1 | Support |
|-------|-----------|--------|----|---------|
| Non-suspicious | 0.94 | 0.88 | 0.91 | 1,210 |
| Suspicious | 0.60 | 0.77 | 0.68 | 293 |
| Macro avg | 0.77 | 0.82 | 0.79 | 1,503 |

![Confusion Matrix](results/confusion_matrix_resnet50_finetuned.png)

> 225 of 293 suspicious lesions correctly identified (recall = 0.77). 68 false negatives — critical in clinical screening where missed malignancies carry high risk.

---

## Explainability

Grad-CAM was applied to generate class-discriminative saliency maps, localizing the spatial regions driving each prediction and supporting model auditability in a medical imaging context.

> *Grad-CAM visualizations coming soon.*

---

## References

- Tschandl, P. et al. *HAM10000 dataset.* Scientific Data, 2018.
- Selvaraju, R. R. et al. *Grad-CAM.* ICCV, 2017.
- He, K. et al. *Deep Residual Learning for Image Recognition.* CVPR, 2016.
- Tan, M. & Le, Q. V. *EfficientNet.* ICML, 2019.

---

## Author

**Lucas Marques**  
Software Engineer · Postgraduate in Database Administration · Postgraduate in Software Development

## License

[MIT](LICENSE)
