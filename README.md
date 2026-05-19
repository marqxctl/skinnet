# SkinNet

Estudo experimental de modelos CNN para classificação binária de lesões cutâneas no HAM10000, com transfer learning, fine-tuning seletivo e interpretabilidade via Grad-CAM.

---

## Dataset

HAM10000 — 10.015 imagens dermatoscópicas (7 classes)

Split:
- Train: 7.010
- Val: 1.502
- Test: 1.503

Pré-processamento:
- 224×224
- normalização ImageNet
- class-weighted loss

Ref: Tschandl et al., Scientific Data (2018)

---

## Problema

Binário:

- Suspeita: `mel`, `bcc`, `akiec`
- Não suspeita: `nv`, `bkl`, `vasc`, `df`

---

## Modelos

- CNN baseline
- ResNet50 (transfer learning)
- ResNet50 (fine-tuned)
- EfficientNetB0

Loss: binary cross-entropy (weighted)

---

## Resultados (test)

| Modelo | Acc | Prec | Rec | F1 |
|-------|-----|------|-----|----|
| CNN | 0.81 | 0.00 | 0.00 | 0.00 |
| ResNet50 TL | 0.79 | 0.47 | 0.86 | 0.61 |
| ResNet50 FT | **0.86** | **0.60** | **0.77** | **0.68** |
| EfficientNetB0 | 0.77 | 0.45 | 0.79 | 0.58 |

FN (suspeita): 68

---

## Interpretabilidade

Grad-CAM:

- TP: foco na lesão
- TN: ausência de ativação
- FP: ativação espúria
- FN: sinal difuso ou ausente

---

## Observações

- baseline colapsa na classe majoritária
- transfer learning melhora recall
- fine-tuning melhora trade-off global
- FN é o principal risco clínico

---

## Referências

- Tschandl et al., 2018
- He et al., 2016
- Tan & Le, 2019
- Selvaraju et al., 2017
