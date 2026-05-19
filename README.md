# SkinNet

Experimentos com redes neurais convolucionais para classificação binária de lesões cutâneas no HAM10000.  
Setup baseado em transfer learning + fine-tuning seletivo + interpretação via Grad-CAM.

---

## Dataset

HAM10000 — 10.015 imagens dermatoscópicas (7 classes)

Split fixo:
- train: 7.010  
- val: 1.502  
- test: 1.503  

Preprocess:
- resize 224×224  
- normalização ImageNet  
- imbalance tratado via class-weighted loss  

---

## Threat model (task collapse)

Redução do espaço de rótulos:

- **positive (risk signal)**: `mel`, `bcc`, `akiec`
- **negative (benign manifold)**: `nv`, `bkl`, `vasc`, `df`

Objetivo: maximizar recall na classe positiva (minimizar false negatives)

---

## Models (feature extractors)

- CNN from scratch (baseline, low prior)
- ResNet50 (pretrained backbone, frozen / partial unfreeze)
- ResNet50 (fine-tuned, deep layers unlocked)
- EfficientNetB0 (scaled conv net)

Loss:
- binary cross-entropy
- class-weighted gradient scaling (imbalance correction)

---

## Results (test set)

| model | acc | precision | recall | f1 |
|------|-----|-----------|--------|----|
| CNN baseline | 0.81 | 0.00 | 0.00 | 0.00 |
| ResNet50 TL | 0.79 | 0.47 | 0.86 | 0.61 |
| ResNet50 FT | **0.86** | **0.60** | **0.77** | **0.68** |
| EfficientNetB0 | 0.77 | 0.45 | 0.79 | 0.58 |

false negatives (positive class): 68  
→ main failure mode = missed detection of malignant signal

---

## Interpretability layer (post-hoc forensics)

Grad-CAM used as saliency probe over feature maps:

- TP: activation aligned with lesion manifold
- TN: near-zero activation (clean separation)
- FP: activation leakage into background noise
- FN: weak or diffused gradients → lost signal in feature space

---

## Observations (ML priors)

- baseline CNN → overfits majority class distribution (mode collapse behavior)
- pretrained backbones → better feature priors, improved separability
- fine-tuning → unlocks deeper representation layers → best trade-off
- FN remains critical failure channel (high-risk in medical triage setting)

---

## References

- Tschandl et al., 2018 — HAM10000 dataset
- He et al., 2016 — ResNet residual learning
- Tan & Le, 2019 — EfficientNet scaling laws
- Selvaraju et al., 2017 — Grad-CAM saliency maps
