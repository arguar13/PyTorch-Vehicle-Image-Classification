*Read this in other languages: [Español](README_es.md)*
# Deep Learning Project — Multiclass Vehicle Image Classification Benchmark

This project develops a comprehensive deep learning benchmark for multiclass vehicle image classification using modern computer vision architectures.

Rather than focusing on a single model, the project evaluates the trade-off between predictive performance and computational efficiency across multiple deep learning architectures, including convolutional neural networks (CNNs) and Vision Transformers (ViTs).

The objective is to classify vehicle images into multiple transportation categories while identifying the most suitable architecture for real-world deployment scenarios such as intelligent transportation systems, traffic monitoring, surveillance, logistics automation, and smart city applications.

---

## About the Dataset

* **Dataset:** Vehicle Image Classification Dataset
* **Task:** Multiclass Image Classification
* **Classes:** 7 (Auto Rickshaws, Bikes, Cars, Motorcycles, Planes, Ships, Trains)
* **Total Images:** Approximately 5,600

Images are organized using the PyTorch `ImageFolder` structure.

The dataset contains vehicle images with varying viewpoints, lighting conditions, scales, and backgrounds, making it suitable for evaluating model robustness and generalization capabilities.

---

## Project Objectives

The project establishes an end-to-end computer vision benchmark pipeline including:

* Dataset validation and quality control
* Exploratory Data Analysis (EDA)
* Class distribution analysis
* Image preprocessing and augmentation
* Stratified train-validation splitting
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

### Validation Pipeline

For deterministic evaluation:

* Resize (224×224)
* ImageNet Normalization

These transformations preserve semantic vehicle features while improving robustness to visual variability.

---

## Class Balancing Strategy

To mitigate class imbalance during training:

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

---

## Training Strategy

All models are trained under a unified experimental framework:

* Transfer Learning
* Cross-Entropy Loss
* Adam Optimizer
* Early Stopping
* Stratified Data Splits
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
