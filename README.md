# Trustworthy Pneumonia Detection from Chest X-rays with Grad-CAM Validation

**Project P03 · Team 3 (section-a)**

We train a pneumonia classifier on paediatric chest X-rays and check whether it looks at the **lungs** or relies on **shortcuts** such as text markers, corners and image borders.

> **Status:** Project setup. We're agreeing on the design decisions in [issue #1](https://github.com/Abhishek-Singh-2008/Pneumonia-Detection/issues/1) before Stage 1 code starts. The setup, run steps and results below get filled in as each stage is finished.

---

## Research question

Measure **shortcut learning**:
1. What percentage of Grad-CAM attention falls **inside the lung region**?
2. How does training on **lung-masked** images (everything outside the lungs set to black) change accuracy and attention?

## Approach

| Step | Method |
|---|---|
| Baseline | Custom CNN trained from scratch |
| Stronger model | DenseNet-121 transfer learning, CheXNet style (ImageNet-pretrained, head training, then fine-tuning) |
| Explainability | Grad-CAM heatmaps on both models |
| Attention metric | Sum of Grad-CAM heatmap inside the lung mask ÷ sum over the whole image |
| Masking experiment | Retrain on lung-masked images; cross-test the unmasked model on masked test images |

## Dataset

**Chest X-Ray Images (Pneumonia)**, Kaggle: [`paultimothymooney/chest-xray-pneumonia`](https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia)

- Two classes: `NORMAL` and `PNEUMONIA` (paediatric patients).
- The dataset is **not stored in this repo**. Download steps will be added in Stage 1.1.
- The official `val/` folder has only 16 images, so we build a new validation split from the training images. The split files will live in `data/splits/`.

## Team and stages

The project runs as four stages in a fixed order. Each member starts when the previous one posts a handover note on their parent issue.

| Stage | Member | Work | Branch | Hands over |
|---|---|---|---|---|
| 1 | Abhishek Singh | Data pipeline, splits, preprocessing, baseline CNN | `stage1-data-baseline` | Splits, seed, baseline results |
| 2 | Ajit Yadav | DenseNet-121 transfer learning, comparison with baseline | `stage2-densenet` | Best model, last conv layer name |
| 3 | Aman Singh | Grad-CAM, lung masks, attention metric, shortcut analysis | `stage3-gradcam` | Lung masks, metric function |
| 4 | Himanshu Kumar | Lung-masked training, final comparison, README, report | `stage4-masking-report` | Final table, README, report |

## Repository structure (planned)

```
Pneumonia-Detection/
├── README.md           # project summary, how to run, results
├── requirements.txt    # all libraries with versions
├── data/               # NOT pushed to GitHub; see Dataset section
├── src/
│   ├── data/           # Stage 1: loading, splits, augmentation
│   ├── models/         # Stage 1 & 2: custom CNN, DenseNet-121
│   ├── explain/        # Stage 3: Grad-CAM, lung-attention metric
│   └── masking/        # Stage 4: lung masks, masked training
├── notebooks/          # one notebook per stage: 01_, 02_, 03_, 04_
├── results/            # metrics tables, plots, heatmaps
└── report/             # figures and notes for the Overleaf report
```

## Setup and how to run

*Added in Stage 1.*

## Results

*Filled in as each stage is completed. The final comparison table is added in Stage 4.*

| Model | Training data | Accuracy | Recall | F1 | AUC | Lung attention % |
|---|---|---|---|---|---|---|
| Custom CNN | Unmasked | – | – | – | – | – |
| DenseNet-121 | Unmasked | – | – | – | – | – |
| DenseNet-121 | Lung-masked | – | – | – | – | – |

## Working rules

- `main` is kept working at all times. Each member works on their own branch and merges through a pull request.
- PR reviews: Ajit reviews Stage 1, Aman reviews Stage 2, Himanshu reviews Stage 3, Abhishek reviews Stage 4.
- Never commit the dataset or large model weights. They're shared through Google Drive (link will be added here).
- One fixed random seed (42), kept in a shared config file and used everywhere.
- Every stage has one parent issue split into child issues, labelled `stage-N` + `parent` / `child`.
- Before submission, the code must run from a clean clone using only this README.

## References

1. P. Rajpurkar et al., *CheXNet: Radiologist-Level Pneumonia Detection on Chest X-Rays with Deep Learning*, 2017. [arXiv:1711.05225](https://arxiv.org/abs/1711.05225)
2. R. R. Selvaraju et al., *Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization*, ICCV 2017. [arXiv:1610.02391](https://arxiv.org/abs/1610.02391)
