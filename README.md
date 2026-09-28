*Read this in other languages: [Español](README_es.md)*
# Deep Learning Project — Multiclass Vehicle Image Classification Benchmark

This project develops a comprehensive deep learning benchmark for multiclass vehicle image classification using modern computer vision architectures.

Rather than focusing on a single model, the project evaluates the trade-off between predictive performance and computational efficiency across multiple deep learning architectures, including convolutional neural networks (CNNs) and Vision Transformers (ViTs).

The objective is to classify vehicle images into multiple transportation categories while identifying the most suitable architecture for real-world deployment scenarios such as intelligent transportation systems, traffic monitoring, surveillance, logistics automation, and smart city applications.

---

## About the Dataset

* **Dataset:** [Vehicle Image Classification](https://www.kaggle.com/datasets/mohamedmaher5/vehicle-classification) (Kaggle, Mohamed Maher, CC0)
* **Task:** Multiclass Image Classification
* **Classes:** 7 (Auto Rickshaws, Bikes, Cars, Motorcycles, Planes, Ships, Trains)
* **Total Images:** 5,590 (800 per class, 790 for Cars). 5,589 are loaded: `ImageFolder` skips the single `.gif` file.

Images are organized using the PyTorch `ImageFolder` structure.

The dataset contains vehicle images with varying viewpoints, lighting conditions, scales, and backgrounds, making it suitable for evaluating model robustness and generalization capabilities.

---

## Project Objectives

The project establishes an end-to-end computer vision benchmark pipeline including:

* Dataset validation and quality control
* Exploratory Data Analysis (EDA)
* Class distribution analysis
* Image preprocessing and augmentation
* Stratified train / validation / test splitting (70% / 15% / 15%)
* Class balancing using `WeightedRandomSampler`
* Transfer learning with ImageNet-pretrained models
* Comparative benchmarking of multiple architectures
* Early stopping implementation
* Performance and efficiency evaluation
* Visualization of benchmark results
* Reproducible experimentation framework

---

## Exploratory Data Analysis (EDA)

Before training, the dataset is analyzed to ensure data quality and understand class characteristics.

The analysis includes:

* Dataset profiling
* Class distribution visualization
* Sample image inspection
* Class imbalance assessment
* Raw image structure analysis

This step helps identify potential biases and informs the design of preprocessing and balancing strategies.

---

## Data Preprocessing and Augmentation

### Training Pipeline

The training dataset undergoes several augmentation techniques to improve model generalization:

* Resize (256×256)
* Random Crop (224×224)
* Random Horizontal Flip
* Random Rotation
* Color Jitter
* ImageNet Normalization

### Validation / Test Pipeline

For deterministic evaluation (same object scale as the training crops):

* Resize (256×256)
* Center Crop (224×224)
* ImageNet Normalization

These transformations preserve semantic vehicle features while improving robustness to visual variability.

---

## Class Balancing Strategy

The dataset is nearly balanced (~800 images per class), so the sampler acts as a safeguard that keeps the pipeline correct if the class distribution changes:

* `WeightedRandomSampler` is used for dynamic resampling
* Minority classes receive higher sampling probability
* Validation data remains untouched to preserve realistic evaluation

This approach promotes balanced learning across all vehicle categories.

---

## Evaluated Architectures

The benchmark compares six transfer learning architectures pretrained on ImageNet:

### CNN-Based Models

* ResNet18
* EfficientNet-B0
* MobileNetV3-Large
* DenseNet121
* ConvNeXt-Tiny

### Transformer-Based Models

* Vision Transformer (ViT-B/16)

Each architecture is adapted to the seven-class vehicle classification task by replacing the original classification head.

All six architectures are implemented in the model factory. To keep training time manageable, the executed benchmark runs **ResNet18** and **EfficientNet-B0**; the others can be enabled by uncommenting them in Section 7 of the notebook.

---

## Training Strategy

All models are trained under a unified experimental framework:

* Transfer Learning
* Cross-Entropy Loss
* Adam Optimizer
* Early Stopping on validation loss (patience = 2, best weights restored)
* Stratified Data Splits
* Model selection on the validation set; the test set is used only for the final report
* GPU Acceleration (when available)
* Fixed Random Seeds for Reproducibility

This ensures a fair comparison between architectures.

---

## Evaluation Metrics

The benchmark evaluates both predictive performance and computational efficiency.

### Predictive Metrics

* Accuracy
* Macro F1-Score
* Precision
* Recall
* Classification Report

### Efficiency Metrics

* Total Training Time

This evaluation allows assessment of both model quality and deployment feasibility.

---

## Benchmark Analysis

The project compares architectures according to:

* Classification Accuracy
* Macro F1-Score
* Training Execution Time

Visualization dashboards provide direct comparisons between architectures and highlight the trade-offs between speed and predictive performance.

---

## Results

Results of the executed benchmark (stratified test set of 839 images, best architecture selected by validation macro F1):

| Architecture | Val Macro F1 | Test Accuracy | Test Macro F1 | Training Time (s) |
|---|---|---|---|---|
| **EfficientNet-B0** | **0.976** | **0.968** | **0.968** | 573 |
| ResNet18 | 0.912 | 0.913 | 0.914 | 812 |

* **EfficientNet-B0** is the best model: ~97% accuracy and macro F1 on unseen data, with every class at an F1 of 0.95 or higher. The lowest recall is for Cars and Motorcycles (0.94).
* ResNet18 reaches ~91%, and its validation loss oscillates between epochs, which suggests the learning rate (1e-3) is high for fine-tuning this backbone.
* Training times were measured on an NVIDIA GTX 1650 (4 GB) while other workloads were running on the same machine, so they are indicative only (in isolation ResNet18 is usually faster than EfficientNet-B0).

---

## Project Structure

```
├── data/Vehicles/            # Dataset (not versioned, download from Kaggle)
├── notebooks/
│   └── vehicle_classification_benchmark.ipynb
├── requirements.txt
├── README.md
└── README_es.md
```

---

## How to Run

1. Download the [dataset](https://www.kaggle.com/datasets/mohamedmaher5/vehicle-classification) and extract it so the class folders are under `data/Vehicles/`.
2. Create an environment and install the dependencies (CUDA 12.1 build of PyTorch by default):

   ```bash
   python -m venv .venv
   .venv\Scripts\activate        # Linux/macOS: source .venv/bin/activate
   pip install -r requirements.txt
   ```

3. Open `notebooks/vehicle_classification_benchmark.ipynb` and run all cells. A GPU is recommended.

---

## Applications

Potential real-world applications include:

* Traffic Monitoring Systems
* Intelligent Transportation Systems
* Vehicle Recognition Platforms
* Smart City Infrastructure
* Automated Surveillance
* Logistics and Fleet Management
* Transportation Analytics

---

## License

Project for educational purposes. The dataset is used exclusively for research, experimentation, and educational purposes.

---

## Author
**Armando Guarnera**

Data Scientist 

Argentina
