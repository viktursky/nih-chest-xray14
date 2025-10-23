# NIH Chest X-ray14: Multi-Label Pathology Detection

This project trains a **Convolutional Neural Network (CNN)** to perform **multi-label classification** on chest X-ray images, identifying diseases such as Pneumonia, Emphysema, and others using the **NIH Chest X-ray14 dataset**.

---

## Table of Contents
1. [Project Objective](#project-objective)
2. [Dataset](#dataset)
3. [Data Preprocessing](#data-preprocessing)
4. [Model Architecture](#model-architecture)
5. [Training](#training)
6. [Evaluation](#evaluation)
7. [Usage](#usage)
8. [Results](#results)
9. [Observations & Conclusion](#observations--conclusion)
10. [References](#references)

---

## Project Objective
To train a CNN model to detect multiple diseases in chest X-rays using **supervised multi-label learning**.

---

## Dataset
- **Source:** NIH Chest X-ray14  
- **Total images (after filtering):** ~112,000  
- **Format:** PNG  
- **Labels:** Multiclass (each image can contain multiple pathologies)  
- **Selected pathologies:** `['Atelectasis', 'Cardiomegaly', 'Effusion', 'Emphysema', 'Infiltration', 'Mass', 'Nodule', 'Pneumonia', 'Pneumothorax']`  

---

## Data Preprocessing
- CSV files containing labels were linked to image files.  
- Multi-label text annotations converted to **binarized format** using `MultiLabelBinarizer()`.  
- Images without a corresponding file were discarded.  
- **Stratified split** applied:  
  - Training: 72%  
  - Validation: 8%  
  - Test: 20%  

### Image Preprocessing
- Input size: `(224, 224, 3)`  
- Normalization: Rescaled to `[0,1]`  
- Data augmentation (training only): rotation, zoom, horizontal/vertical shifts, horizontal flips  

---

## Model Architecture
Sequential CNN model:

| Layer       | Details |
|------------|---------|
| Conv2D     | 32 filters, 3x3 kernel, ReLU |
| MaxPooling2D | 2x2 pool |
| Conv2D     | 64 filters, 3x3 kernel, ReLU |
| MaxPooling2D | 2x2 pool |
| Conv2D     | 128 filters, 3x3 kernel, ReLU |
| MaxPooling2D | 2x2 pool |
| Flatten    | Flatten volume |
| Dense      | 128 units, ReLU, Dropout 0.5 |
| Dense      | `n_classes` units, Sigmoid activation (multi-label output) |

---

## Training
- Optimizer: **Adam**, learning rate = 0.0001  
- Loss function: `binary_crossentropy`  
- Metric: **AUC (Area Under the ROC Curve)**  
- Epochs: 10  
- Batch size: 64  
- Callbacks: `ModelCheckpoint` to save the best model  

---

## Evaluation
- **Validation AUC:** ~0.70  
- Visual inspection of test images with true vs. predicted labels using a threshold of 0.5.  
- Training vs validation curves show no strong overfitting.

---

## Usage

1. Clone the repository:

```bash
git clone https://github.com/your-username/nih-chest-xray14.git
cd nih-chest-xray14
