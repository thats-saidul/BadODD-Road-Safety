# Class-Aware Data Augmentation for Long-Tailed Object Detection on the BadODD Dataset

This repository contains the code and experimental materials for the research paper:

**"Class-Aware Data Augmentation for Long-Tailed Object Detection on the BadODD Dataset"**

## Authors

- **MD Saidul Islam**
- **Mehedi Hasan Raj**
- **Tanvir Hassan Tonmoy**
- **Tanvir Ahmed**

Department of Computer Science and Engineering  
American International University-Bangladesh (AIUB)  
Dhaka, Bangladesh

---

## Abstract

Long-tailed class distributions are a major challenge in object detection for autonomous driving because frequent classes can dominate aggregate performance while rare but safety-relevant categories remain difficult to detect.

This study investigates the problem using the **BadODD dataset**, a Bangladeshi road-scene object detection dataset. The observed training distribution contains a severe class imbalance, with an imbalance ratio of **869.9** between the most and least frequent non-zero classes.

Two data-centric strategies were investigated using a fixed **YOLO11s** detector:

1. **Class-aware image-level oversampling**
2. **Targeted bounding-box-safe data augmentation**

Both strategies were applied to images containing under-represented categories. The experiments were conducted using the same training and evaluation protocol, with a fixed random seed and a 25-epoch training budget.

The combined class-aware strategy increased:

- **mAP@0.50:** 60.78 → 62.86
- **mAP@0.50:0.95:** 38.58 → 40.24

The minority-category group showed a **+6.83 percentage-point** change in mAP@0.50, while the majority group changed by only **+0.03 points**.

However, the study is preliminary. Only one random seed was used, and the individual effects of oversampling and augmentation were not separately isolated.

---

## Research Objective

The main objective of this study is to investigate whether simple class-aware training strategies can improve detection performance for under-represented categories in the BadODD dataset while keeping the detector architecture unchanged.

The study focuses on:

- Understanding the long-tailed class distribution of BadODD.
- Identifying minority object categories using a data-driven grouping method.
- Applying class-aware image-level oversampling.
- Applying targeted bounding-box-safe augmentation.
- Comparing the proposed training strategy with a YOLO11s baseline.
- Evaluating performance using overall and per-class detection metrics.

---

## Dataset

The experiments use the **BadODD (Bangladeshi Autonomous Driving Object Detection Dataset)**.

The dataset contains road scenes collected across nine districts of Bangladesh under different environments and conditions, including:

- Urban
- Rural
- Highway
- Expressway
- Daytime
- Night-time

The dataset contains **13 object categories**:

1. Auto-rickshaw
2. Bicycle
3. Bus
4. Car
5. Cart-vehicle
6. Construction-vehicle
7. Motorbike
8. Person
9. Priority-vehicle
10. Three-wheeler
11. Train
12. Truck
13. Wheelchair

The study used the publicly distributed Zenodo release:

**DOI:** `10.5281/zenodo.13823687`

The observed dataset contained:

- 10,032 total images
- 80,036 annotated objects
- 5,967 training images
- 2,032 validation images
- 2,033 test images
- 13 object classes

---

## Class Imbalance

The BadODD training distribution is highly imbalanced.

The three most frequent classes:

- Person
- Auto-rickshaw
- Three-wheeler

account for approximately **72.91%** of all training instances.

The three least frequent classes:

- Construction-vehicle
- Train
- Wheelchair

account for only **0.16%** of training instances.

The observed imbalance ratio between the largest and smallest non-zero class was:

**869.9 : 1**

---

## Method

The proposed training strategy consists of two components.

### 1. Class-Aware Image-Level Oversampling

Images containing minority-group categories were identified from the training dataset.

A repeat factor was calculated using square-root dampening:

```text
r_c = min(r_max, sqrt(n_med / n_c))
