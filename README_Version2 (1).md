# Classification of Tumor vs Fibrosis Using Deep Learning

This repository contains the code and resources for a final year research project that applies deep learning to classify PET/CT images as either Tumor or Fibrosis. Our hybrid deep learning model leverages both spatial and contextual features from medical images, combined with quantitative metabolic information (SUVmax), to achieve highly accurate classification—assisting radiologists and researchers in diagnosis and workflow.

---

## Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Data Preprocessing](#data-preprocessing)
- [Model Architecture](#model-architecture)
- [Training & Evaluation](#training--evaluation)
- [Results](#results)
- [Usage](#usage)
- [License](#license)
- [Acknowledgements](#acknowledgements)

---

## Overview

The project implements a hybrid neural network that combines convolutional and transformer-based approaches, augmented with radiomic features, for robust binary classification of medical images. The model is designed to differentiate between tumor tissue and fibrosis, facilitating better decision making in clinical settings.

---

## Dataset

- **Source:** PET/CT DICOM image slices.
- **Size:** Approximately 581 image slices.
- **Labeling Criteria:**
  - Each slice has a computed SUVmax value (Standardized Uptake Value, Maximum).
  - **0:** Fibrosis (SUVmax ≤ 2.5)
  - **1:** Tumor (SUVmax > 2.5)

---

## Data Preprocessing

- **Intensity Standardization:**
  - PET intensities are clipped to [0, 20] SUV units.
  - Images are normalized to [0, 1].
- **Image Formatting:**
  - All slices are resized to 224×224 pixels.
  - Images are converted to 3-channel RGB format.
- **Radiomic Feature Extraction:**
  - SUVmax is calculated per slice using DICOM metadata.
- **Augmentation:**
  - Random rotation
  - Horizontal flip
  - Stratified splitting: 80% training / 20% validation

---

## Model Architecture

- **Local Spatial Features:**  
  ResNet-18 (removing the final FC layer) extracts 512-dimensional features per image.
- **Global Context Features:**  
  Vision Transformer (3 encoder layers, 2 attention heads, 96-dim hidden) extracts 96-dimensional features.
- **Radiomic Feature:**  
  Scalar SUVmax value appended.
- **Fusion and Classification:**  
  The model concatenates all features (total 609-dim) before passing them through a multi-layer perceptron (MLP) for binary classification.

---

## Training & Evaluation

- **Optimizer:** (Specify: e.g., Adam, SGD)
- **Loss Function:** Binary Cross-Entropy
- **Hardware:** (Specify: e.g., Nvidia GPU)
- **Metrics:** Accuracy, Precision, Recall, F1, ROC-AUC

---

## Results

- **Overall Accuracy:** 94.02%
- **AUC:** 0.9954 (ROC curve indicates near-perfect discrimination)
- **Classification Metrics:**
    - _Fibrosis_: Precision = 0.98, Recall = 0.91, F1-score = 0.95
    - _Tumor_: Precision = 0.88, Recall = 0.98, F1-score = 0.93
- **Confusion Matrix Overview:**
    - Fibrosis: 64 correct, 6 misclassified
    - Tumor: 46 correct, 1 misclassified
- The model demonstrates high sensitivity for tumor detection, aligning with clinical priorities.

---

## Usage

1. **Installation**
    ```bash
    git clone https://github.com/Adithibpnad/Classification-of-Tumor-vs-Fibrosis-using-deep-learning.git
    cd Classification-of-Tumor-vs-Fibrosis-using-deep-learning
    pip install -r requirements.txt
    ```
2. **Prepare Dataset**
    - Place PET/CT DICOM slices in the `data/` directory.
    - Ensure correct directory structure as per `data_loader.py` (if applicable).
3. **Train Model**
    ```bash
    python train.py
    ```
4. **Evaluate on Test Set**
    ```bash
    python evaluate.py
    ```
5. **View Output**
    - Evaluation metrics and logs will appear in the console or output files, as implemented.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

---


- Thanks to all contributors and open-source resources.

---

_For questions or feedback, please open an issue or contact [Adithibpnad](https://github.com/Adithibpnad)._
