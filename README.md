# Drone Image Semantic Segmentation — Baseline Pipeline

Mostly prep work for a : **"Deep Learning-Based Semantic Segmentation of Peatland Microforms from High-Resolution Drone Imagery."**

## What this is

Before applying deep learning segmentation to real peatland drone imagery (once field data and labels are ready), I built and validated a full segmentation pipeline on a public benchmark dataset, to learn the tools and confirm the pipeline works end-to-end.

## Dataset

[TU Graz Semantic Drone Dataset](https://ivc.tugraz.at/research-project/semantic-drone-dataset/) — 400 labeled nadir drone images, 24 semantic classes (e.g. grass, paved-area, roof, vegetation, water). Used here purely as a sandbox; not related to the actual thesis study site.

## Approach

- **Model:** U-Net with a ResNet34 encoder, pretrained on ImageNet (transfer learning)
- **Framework:** PyTorch + `segmentation-models-pytorch`
- **Training:** 10 epochs, 80/20 train/val split, images resized to 256×256
- **Evaluation:** Per-class and mean IoU (Intersection over Union), not just loss

## Results

- Train loss: 1.07 → 0.70, Val loss: 1.11 → 0.85 over 10 epochs (steady improvement, no overfitting yet)
- Mean IoU (macro-averaged across all 24 classes): 0.215
- Strong performance on common/large classes (grass: 0.87, paved-area: 0.79, roof: 0.70), near-zero on rare/small classes (dog, car, window, fence)

## Key takeaway

This surfaced real **class imbalance**, an expected challenge with per-pixel loss functions and small datasets. This is directly relevant for the actual thesis, where peatland microform classes may also be imbalanced — useful to have seen and understood this failure mode ahead of time, along with the standard fixes (class-weighted loss, Dice/Focal loss, augmentation).

## Status

This is a learning/setup exercise, not thesis results. The actual project will use real peatland drone imagery and field-collected labels, currently being prepared.
