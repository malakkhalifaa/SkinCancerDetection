# SkinCancerDetection Project (APS360)

APS360 Project: 7-class skin lesion classification on the HAM10000 dataset. Includes reproducible data preprocessing, stratified train/val/test splits, augmentation, baseline Random Forest model, and a VGG-16 transfer learning model with a custom classifier head.

This repository presents a deep learning pipeline for **automatic classification of skin cancer lesions** using dermoscopic images. The model supports seven target classes: **akiec, bcc, bkl, df, mel, nv, and vasc**. It applies **transfer learning** with the **VGG16** architecture for robust feature extraction and demonstrates significant improvements over a baseline Random Forest classifier.  

---

## Table of Contents
- [Project Overview](#project-overview)  
- [Dataset Description](#dataset-description)  
- [Preprocessing](#preprocessing)  
- [Model Architecture](#model-architecture)  
- [Baseline Model](#baseline-model)  
- [Training Details](#training-details)  
- [Evaluation Metrics](#evaluation-metrics)  
- [Qualitative Results](#qualitative-results)  
- [External Evaluation](#external-evaluation)  
- [Ethical Considerations](#ethical-considerations)  

---

## Project Overview

Skin cancer is one of the most common cancers worldwide, and early detection is critical for improving patient outcomes. Manual diagnosis requires dermatological expertise and can be time-consuming. This project applies **Convolutional Neural Networks (CNNs)** for automated lesion classification.  

The solution includes:  
- Multi-class classification into 7 skin lesion types  
- Transfer learning with VGG16  
- Data augmentation and oversampling of minority classes  
- Baseline comparison with Random Forest  
- Evaluation on new external ISIC test data  

<p align="center">
  <img src="https://github.com/user-attachments/assets/d9ad09a7-77a2-4d22-9c0b-d0c15d763226" width="80%" />
</p>

---

## Dataset Description

- Source: HAM10000 Skin Cancer Dataset (Kaggle / ISIC 2018)  
- Total Images: 10,015 dermoscopic images  
- Classes: akiec, bcc, bkl, df, mel, nv, vasc  
- Split: 70% training (7,010), 15% validation (1,502), 15% testing (1,503)  

Class distribution is highly imbalanced, with **nv (67%)** dominant, while **vasc (1.4%)** and **df (1.1%)** are rare.  

<p align="center">
  <img src="https://github.com/user-attachments/assets/30f1aff2-e556-4da4-ab23-8f44095cd14a" width="45%" />
  <img src="https://github.com/user-attachments/assets/6f9eedaf-6b98-459d-8984-8312b3eae71c" width="50%" />
</p>

---

## Preprocessing

- Cropped and resized all images to 224×224 pixels  
- Normalized using ImageNet mean and standard deviation  
- Data augmentation: random horizontal/vertical flips, rotations, color jitter, random greyscale  
- Oversampling of minority classes to reduce bias  

<p align="center">
  <img src="https://github.com/user-attachments/assets/97302d83-b3cf-4a31-a274-334ac0c02c3b" width="75%" />
</p>

---

## Model Architecture

- Base: VGG16 (ImageNet weights, convolutional layers frozen)  
- Custom Classifier Head:  
  - Dense(1024) + BN + ReLU + Dropout(0.3)  
  - Dense(512) + BN + ReLU + Dropout(0.2)  
  - Dense(128) + BN + ReLU + Dropout(0.2)  
  - Dense(7, activation=softmax)  
- Parameters: ~26.3M trainable  

<p align="center">
  <img src="https://github.com/user-attachments/assets/61215261-f047-4064-8b99-44183b177978" width="85%" />
</p>

---

## Baseline Model

- Model: Random Forest classifier  
- Validation Accuracy: 72.2%  
- Test Accuracy: 71.5%  
- F1-scores: strong on dominant class (nv, 0.85) but **0.00 on rare classes (df, vasc)**  

<p align="center">
  <img src="https://github.com/user-attachments/assets/d3478b89-3464-49a1-9063-8cecc09407a6" width="75%" />
</p>

---

## Training Details

- Platform: Google Colab (GPU-enabled)  
- Primary Model (VGG16):  
  - Validation Accuracy: **74.5%**  
  - Test Accuracy: **74.7%**  
  - Macro F1-score: **0.60** (vs. 0.30 baseline)  
  - Weighted F1-score: **0.76** (vs. 0.66 baseline)  

---

## Evaluation Metrics

### Test Classification Report (Primary Model)

- Overall Accuracy: 74.7%  
- Error Rate: 25.3%  
- Class 0 (akiec): F1-score = 0.53  
- Class 3 (df): F1-score = 0.47 (baseline 0.00)  
- Class 6 (vasc): F1-score = 0.62 (baseline 0.00)  
- Majority class (nv): F1-score = 0.86  

<p align="center">
  <img src="https://github.com/user-attachments/assets/a463269e-f245-461b-962f-3e7dcfb71ff7" width="75%" />
</p>

Improvements across minority classes show balanced performance compared to baseline.  

<p align="center">
  <img src="https://github.com/user-attachments/assets/f99bcd5f-79ad-460d-9a95-6863063a9270" width="75%" />
</p>

---

## Qualitative Results

- **Baseline Confusion Matrix**: strong on majority class (965 correct for nv), no correct predictions for df and vasc.  
- **Primary Confusion Matrix**: improved minority class predictions while maintaining strong performance on nv (830 correct, 82% hit rate).  

<p align="center">
  <img src="https://github.com/user-attachments/assets/d9899736-9730-4dee-b941-d298700d6401" width="48%" />
  <img src="https://github.com/user-attachments/assets/ff0463eb-64de-4f50-ac9f-7f3003474c52" width="48%" />
</p>

---

## External Evaluation

- Dataset: ISIC 2018 external test set (1,511 images)  
- Accuracy: **67.3%** (7% lower than original test set)  
- Minority class improvements maintained:  
  - Class 3 F1-score = 0.39 (vs. 0.00 baseline)  
  - Class 6 F1-score = 0.51 (vs. 0.00 baseline)  

<p align="center">
  <img src="https://github.com/user-attachments/assets/c259453c-0e8f-4c90-95ef-250392bc35c0" width="75%" />
</p>

---

## Ethical Considerations 

- Dataset underrepresents darker skin tones, introducing potential bias  
- Oversampling used to partially mitigate imbalance  
- Dataset consent limitations raise privacy concerns  
- Model should not be over-relied on clinically; serves as a research prototype  
