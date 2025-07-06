# NRCC251020: Automatic CLOUD and SHADOW mask generation from Resourcesat-2/2A Liss4 Satellite Images

Welcome to the repository for deep learning-based cloud and shadow segmentation on Resourcesat 2/2A LISS 4 satellite images. This project provides a complete pipeline for training, evaluation, and inference using a U-Net model, along with all supporting data, code, and documentation. The repository has been created as part of our submission for the **NRSC Offline Coding Challenge**.

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Data Organization](#data-organization)
- [Model Training](#model-training)
- [Inference Pipeline](#inference-pipeline)
- [Evaluation \& Results](#evaluation--results)
- [Requirements](#requirements)
- [References \& Documentation](#references--documentation)
- [Contact](#contact)


## Overview

This repository contains all resources for training and deploying a U-Net model to segment clouds, shadows, snow, and background in Resourcesat 2/2A LISS 4 imagery. The workflow followed includes:

- Data preparation and annotation
- Model training and evaluation
- Automated inference and geospatial output (TIFFs, shapefiles)
- Visualizations and performance reports


## Repository Structure

| File/Folder                             | Description                                                                               |
| --------------------------------------- | ----------------------------------------------------------------------------------------- |
| `Report.pdf`                            | Detailed project report, methodology, and results                                         |
| `NRCC251020_Training_Inference.csv`     | Training and evaluation metrics, hyperparameters, and results                             |
| `requirements.txt`                      | List of all required Python packages and versions                                         |
| `Training_Labeled_Data.zip`             | Labeled training data: JPG images and corresponding TIFF masks                            |
| `NRCC251020_Inference_Code.zip`         | Inference pipeline: code, inference results CSV, and help document                        |
| `Model.zip`                             | Model definition (`Model.py`), trained weights (`NRCC251020_model.h5`), and help file     |
| `Masks.zip`                             | Output masks: folders for each image with `mask.tiff`, `cloudshapes/`, and `shadowshapes/`|

> **Note:** While extracting files from any zip folder, choose the **Parent folder** as the destination location.

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/TheMuggleCode/NRCC251020.git
cd NRCC251020
```


### 2. Install Dependencies

Install all required packages using:

```bash
pip install -r requirements.txt
```

> **Note:**
> - `tkinter` is part of the Python standard library (no need to install via pip).
> - For geospatial libraries, ensure you have the necessary system dependencies (e.g., GDAL).

## Data Organization

### Training Data

- **Location:** `Training_Labeled_Data.zip`
- **Contents:**
    - JPG images (input)
    - TIFF masks (ground truth, 5 classes: Background, Cloud, NoCloud, Shadow, Snow)


### Inference Data

- **Location:** `Masks.zip`
- **Contents:**
    - For each image, with corresponding name folder:
        - `mask.tiff` (predicted mask)
        - `cloudshapes/` (shapefile for cloud polygons)
        - `shadowshapes/` (shapefile for shadow polygons)


#### Example Directory Structure

<pre>
Training_Labeled_Data/
├── image1.jpg
├── image1_mask.tiff
├── image2.jpg
├── image2_mask.tiff
...
Masks/
├── Image1/
│   ├── mask.tiff
│   ├── cloudshapes/
│   └── shadowshapes/
├── Image2/
│   ├── mask.tiff
│   ├── cloudshapes/
│   └── shadowshapes/
...
</pre>

## Model Training

- **Script:** See `Model.py` in `Model.zip` (Refer to the **HELP.md** file)
- **Architecture:** U-Net with 4 encoder/decoder blocks, ReLU activations, and softmax output
- **Input Size:** 128 × 128 × 3 (RGB)
- **Classes:** 5 (Background, Cloud, NoCloud, Shadow, Snow)
- **Loss Function:** Weighted categorical crossentropy + Dice loss
- **Optimizer:** Adam
- **Metrics:** Accuracy, IoU
- **Training/Validation/Test Split:** 70% / 20% / 10%
- **Early Stopping \& Checkpointing:** Enabled


#### Training Outputs

- `NRCC251020_Model.h5`: Trained model weights
- `NRCC251020_Training_Inference.csv`: Training metrics and hyperparameters, Results on provided Test data also included.


## Inference Pipeline

- **Script:** See inference code in `NRCC251020_Inference_Code.zip`; both .ipynb and .py files are included. Refer **HELP.md**.
- **Workflow:**

1. User selects a folder containing image subfolders via a GUI dialog
2. For each image folder:
        - Generates a false color composite (FCC) PNG
        - Predicts mask using the trained model
        - Saves georeferenced `mask.tiff` using the meta data file
        - Extracts and saves cloud/shadow polygons as shapefiles
        - Visualizes results and prints pixel statistics
- **Progress bars** are displayed for FCC generation and TIFF writing

> **Note:** NRCC251020_Inference.csv under the folder includes tabulated results for the 13 test data sets provided under the competition.


## Evaluation \& Results

- **Metrics on Test Set:**
    - Overall Accuracy, Macro Precision, Macro Recall, Macro F1-Score
    - Per-class and mean IoU
    - Confusion matrix and classification report
- **Visual Outputs:**
    - Sample predictions vs. ground truth
    - Training/validation curves
    - Heatmaps and mask overlays
- **See:**
    - `Report.pdf` for detailed analysis and discussion
    - `NRCC251020_Training_Inference.csv` for quantitative results


## Requirements

All dependencies are listed in `requirements.txt`. Key packages include:

- `tensorflow==2.19.0`
- `numpy==1.26.4`
- `opencv-python==4.5.5`
- `Pillow==10.4.0`
- `matplotlib==3.9.3`
- `seaborn==0.13.2`
- `scikit-image==0.25.2`
- `scikit-learn==1.6.1`
- `rasterio==1.4.3`
- `geopandas==0.14.0`
- `shapely==2.1.1`
- `Fiona==1.10.1`
- `pyproj==3.7.1`
- `tqdm==4.67.1`


## References \& Documentation

- **Help documents** are included in the respective zip folders for model and inference code.
- For further details, consult:
    - `Report.pdf` (project methodology and results)
    - In-code docstrings and comments
    - [TensorFlow documentation](https://www.tensorflow.org/)
    - [GeoPandas documentation](https://geopandas.org/)
    - [Rasterio documentation](https://rasterio.readthedocs.io/)


## Contact

For questions or support, please contact:

- nagadileelarao [at] gmail [dot] com
- rambabukushwaha04 [at] gmail [dot] com

**If you use this code or data, please cite this repository and acknowledge the authors.**
