# Hybrid Brain Tumor MRI Analysis  
**Xception Classification + Class-Conditional Classical Segmentation**  
*Maxime Argenson*

## Overview
This project implements a hybrid pipeline for automated brain tumor analysis in MRI images. It combines deep learning–based multi-class classification with class-conditional classical image processing for tumor localization.

A pretrained **Xception** convolutional neural network is fine-tuned to classify MRI scans into four categories:

- Glioma  
- Meningioma  
- Pituitary tumor  
- No tumor  

Based on the predicted tumor class, a tailored classical segmentation/localization algorithm is then applied to highlight likely tumor regions.

This approach is designed for datasets that provide **image-level labels but no pixel-level segmentation masks**.

---

## Dataset
**Source:** Kaggle Brain Tumor MRI Dataset  
**Total images:** 7,153

Class distribution:
- Glioma — 1,621  
- Meningioma — 1,775  
- Pituitary — 1,757  
- No tumor — 2,000  

Data split (stratified):
- 80% train  
- 10% validation  
- 10% test  

---

## Methods

### Deep Learning Classification
- Transfer learning with **Xception (ImageNet pretrained)**
- Input size: 299×299
- Data augmentation on training set (rotation, zoom, brightness, translation)
- Custom classification head with softmax output (4 classes)
- Loss: categorical cross-entropy
- Optimizer: Adamax
- Fine-tuning backbone layers after head training

### Class-Conditional Segmentation
After classification, different classical image processing pipelines are applied per tumor type:

- **Pituitary:** ROI-based bright-region detection near anatomical center  
- **Meningioma:** Boundary-aware bright-region detection using brain masks  
- **Glioma:** Dark-core + ring-contrast heuristics  

Techniques used:
- CLAHE contrast enhancement
- Gaussian / median / bilateral filtering
- Morphological operations
- Connected component scoring

Segmentation results are evaluated **qualitatively** via overlay visualization (no ground-truth masks available).

---

## Results

### Classification Performance (Test Set)

- **Accuracy:** 96.65%  
- **Precision:** 96.65%  
- **Recall:** 96.65%  
- **Test Loss:** 0.1072  

Key observations:
- Strong separation between tumor vs no-tumor
- Most confusion occurs between visually similar tumor subtypes
- No-tumor class predictions are highly reliable

### Segmentation Performance
- Best results: Pituitary tumors (consistent location + brightness)
- Mixed results: Meningiomas (boundary leakage issues)
- Most challenging: Gliomas (high variability in appearance)

---

## References
- Kaggle Brain Tumor MRI Dataset  
- Xception: Deep Learning with Depthwise Separable Convolutions  
- Gonzalez & Woods — *Digital Image Processing*
