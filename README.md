# Cell Image Classification with CNNs

A deep learning project for classifying cell types from images using Convolutional Neural Networks (CNNs), with potential applications in automated medical diagnostics.

---

## Table of Contents

- [Introduction](#introduction)
- [Model Architecture](#model-architecture)
- [Techniques & Experiments](#techniques--experiments)
  - [Data Augmentation](#data-augmentation)
  - [Batch Normalization](#batch-normalization)
  - [Additional Convolutional Layers](#additional-convolutional-layers)
  - [SENet, Residual, and Inception Blocks](#senet-residual-and-inception-blocks)
  - [Transfer Learning](#transfer-learning)
  - [Fine Tuning](#fine-tuning)
  - [Class Balancing](#class-balancing)
- [Results Summary](#results-summary)

---

## Introduction

This project focuses on image classification to identify different types of cells using CNNs. The dataset includes preprocessed images labeled with various cell categories. Advanced techniques such as regularization, data augmentation, and hyperparameter optimization were implemented to maximize model accuracy.

---

## Model Architecture

The base model is a CNN that accepts **96×96×3** RGB images and consists of:

- **Convolutional Layers** — extract spatial features (edges, textures, patterns)
- **MaxPooling Layers** — reduce spatial dimensions and retain dominant features
- **Flattening Layer** — converts feature maps into a 1D vector
- **Dense + Output Layers** — combines learned features; output uses softmax for multi-class classification

Standard techniques include ReLU activations, He weight initialization, and the Adam optimizer.

---

## Techniques & Experiments

### Data Augmentation

Random transformations applied to input images to improve generalization:
- Horizontal/vertical flips
- Rotations (up to 20%), zooming, translations
- Brightness and contrast adjustments

**Impact:** Validation accuracy improved from **84.1% → 84.5%** with augmentation on the original dataset. When combined with SMOTE-based upsampling, a slight drop was observed (85.98% → 84.81%), likely due to degraded synthetic images.

---

### Batch Normalization

Normalizes activations to zero mean and unit variance after selected convolutional layers, placed before the activation function.

**Impact:** Improved validation accuracy across configurations, accelerated training, and reduced overfitting risk. Slightly increased the number of epochs needed for convergence.

---

### Additional Convolutional Layers

Two extra convolutional layers were added to the base configuration to learn higher-level, more abstract features, with different filter count combinations tested.

**Impact:** No significant improvement observed — most trials led to a slight decrease in performance.

---

### SENet, Residual, and Inception Blocks

A composite architecture was designed nesting:
1. **Inception Block** — multi-scale feature extraction
2. **SENet Block** — channel-wise attention to refine the most relevant features
3. **Residual Block** — ensures efficient gradient flow in deep networks

This composed block was stacked up to three times.

**Impact:** +0.08 improvement on Codabench, but no significant gain in validation accuracy, likely due to dataset size limitations and overfitting risk.

---

### Transfer Learning

Used **InceptionV3** pre-trained on ImageNet as a feature extractor, replacing the final dense layer to match the target task's output classes.

**Impact:** Validation accuracy of **80.06%**. Reduced training time due to fewer trainable parameters.

---

### Fine Tuning

Built on top of the transfer learning model by unfreezing select layers and retraining with a smaller learning rate to extract task-specific features.

**Impact:** Final validation accuracy of **85.39%**. Improved flexibility and adaptability while reducing overfitting risk.

---

### Class Balancing

Dataset analysis revealed class imbalance, which can cause biased predictions and poor generalization for underrepresented classes.

**Approach:** Applied **SMOTE** (Synthetic Minority Over-sampling Technique), generating synthetic samples by interpolating between existing minority-class instances. Dataset size grew from **11,144 → 16,384** samples.

**Impact:** Final validation accuracy of **85.79%**.

---

## Results Summary

| Technique                        | Validation Accuracy |
|----------------------------------|---------------------|
| Basic Model                      | 84.10%              |
| + Data Augmentation              | 84.50%              |
| + Batch Normalization            | improvement noted   |
| + SENet/Residual/Inception       | ~84.18% (Codabench +0.08) |
| Transfer Learning (InceptionV3)  | 80.06%              |
| + Fine Tuning                    | 85.39%              |
| + Class Balancing (SMOTE)        | **85.79%**          |
