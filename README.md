# XAI-Lite-Efficient-Framework-for-Interpretable-Deep-Learning-in-Low-Resource-Healthcare-Imaging

Project Overview

XAI-Lite is a research project exploring a computationally efficient framework for interpretable deep learning in low-resource healthcare imaging. The paper focuses on skin-lesion classification from dermoscopic images and investigates ways to reduce the cost of both prediction and visual explanation.

The proposed framework combines a lightweight MobileNetV3-Large classifier with explanation approaches based on Grad-CAM and distilled attention. The study evaluates the framework using the HAM10000 dataset.

   Important: XAI-Lite is a research prototype, not a medical device. Its outputs must not be used as a standalone diagnosis or to make patient-care decisions.

Research Objective

- Develop an efficient deep-learning framework for healthcare imaging.
- Classify dermoscopic images into seven diagnostic categories.
- Investigate model-compression techniques for reducing computation and storage.
- Reduce the computational overhead of generating visual explanations.
- Evaluate classification performance alongside latency, memory, and model size.
- Explore explainable AI methods suitable for low-resource environments.
Business / Real-World Objective
The framework is motivated by settings where computing resources may be limited, including:
- Rural healthcare facilities
- Mobile screening units
- Low-power or edge-computing devices
- Healthcare environments with limited connectivity
- Medical-imaging research and education
The goal is to investigate whether efficient image classification and visual explanation can be made more practical on constrained hardware. Clinical usefulness and safety would require additional validation.

Project Workflow

1. Dataset Collection
The paper evaluates the framework using the HAM10000 dermoscopic image dataset, which contains images across seven diagnostic categories.

The paper reports a held-out test set of 1,503 images.

The dataset categories are:
Label	Category
akiec	Actinic keratoses and intraepithelial carcinoma
bcc	Basal cell carcinoma
bkl	Benign keratosis-like lesions
df	Dermatofibroma
mel	Melanoma
nv	Melanocytic nevi
vasc	Vascular lesions


Obtain the dataset from its official source and follow its terms of use. Do not upload restricted data or patient-related metadata to GitHub.

2. Image Preprocessing
The paper describes the following preprocessing pipeline:
- Resize input images to 224 × 224 pixels.
- Normalize images using ImageNet statistics.
- Apply data augmentation during training.
- Use class-balancing techniques during training.
- Evaluate on a held-out test set.
Training-only preprocessing and augmentation should not be applied in a way that leaks information from the test set.

3. Lightweight Feature Extraction and Classification
The framework uses MobileNetV3-Large as its lightweight image-classification backbone.

The classification pathway is intended to:
- Extract visual features from dermoscopic images.
- Predict one of seven diagnostic categories.
- Reduce computational cost compared with heavier architectures.
- Support evaluation on resource-constrained hardware.

4. Model Compression
The paper investigates multiple techniques to reduce the model's computational and storage requirements:
- Structured pruning: Remove less-important filters or channels.
- Knowledge distillation: Transfer information from a teacher model to a smaller student model.
- INT8 quantization: Represent model weights and/or operations with lower precision where supported.
These methods should be described as implemented only where the repository contains the corresponding code and reproducible checkpoints.

Explainability Framework

The project investigates explanation methods that visualize image regions influencing a model's prediction.
Grad-CAM
Gradient-weighted Class Activation Mapping (Grad-CAM) produces a heatmap from feature maps and gradients associated with a target class. The heatmap can be overlaid on the input image to inspect which regions influence the prediction.
Pruned Grad-CAM
The paper proposes reducing explanation cost by selecting a subset of important feature-map channels for explanation generation.
Distilled Attention
The paper also investigates a compact attention head trained to approximate the spatial pattern of a teacher explanation. The goal is to produce an explanation more efficiently at inference time.

Implementation note: A conventional Grad-CAM implementation alone does not demonstrate that pruned Grad-CAM or a distilled attention head has been implemented. Confirm these components in the source code before claiming them as available features.
System Architecture
The conceptual workflow described in the paper is:
             Dermoscopic Image
                     |
                     v
             Image Preprocessing
          Resize and Normalize
                     |
                     v
          MobileNetV3-Large Backbone
                     |
              +------+------+
              |             |
              v             v
       Classification    Explanation
            Head          Pathway
              |        Grad-CAM / Pruned
              |        Grad-CAM / Distilled
              |             Attention
              v             v
       Predicted Class   Visual Heatmap
              |             |
              +------+------+
                     |
                     v
          Prediction and Explanation
The diagram is a conceptual representation of the research framework. Update it if the final implementation uses a different architecture or execution path.

Evaluation Metrics

The paper evaluates classification performance and reports efficiency measurements.
Classification Metrics
- Accuracy: Proportion of test images classified correctly.
- Macro F1-score: F1-score calculated independently for each class and averaged equally across classes.
- Weighted F1-score: F1-score averaged using each class's support as its weight.
Per-class precision, recall, F1-score, and confusion matrices are also useful for assessing performance on imbalanced medical datasets.

Efficiency Metrics
The paper considers:
- Model parameter count
- Model storage size
- Computational complexity, including MACs/FLOPs
- Classification latency
- Explanation-generation latency
- Peak memory usage
- Resource use on edge hardware
Latency and memory figures should be accompanied by the device, software versions, input size, warm-up procedure, and measurement method.

Results & Analysis

The paper reports the following held-out test results:
Metric	Reported result
Test accuracy	83.63%
Macro F1-score	0.6100
Weighted F1-score	0.8278
Test-set size	1,503 images


Explanation Efficiency

The paper reports these explanation-pathway measurements on a Jetson Nano:
Explanation configuration	Latency	Peak memory
Standard Grad-CAM	18.4 ms/image	145 MB
Pruned Grad-CAM	6.2 ms/image	52 MB
Distilled attention	2.1 ms/image	31 MB


These values are reported by the paper and should not be treated as independently reproduced until the benchmark setup, logs, model checkpoints, and hardware configuration are available.
Important Interpretation
- The paper reports a macro F1-score of 0.61 and a weighted F1-score of 0.8278.
- The reported F1-score for the df class is 0.00, indicating a serious class-specific performance limitation.
- Class imbalance can make weighted metrics appear stronger than minority-class performance.
- The paper describes qualitative inspection of explanations; quantitative validation against lesion masks or expert annotations remains necessary.
- These results do not establish clinical safety or diagnostic suitability.

Key Findings

Lightweight Classification
MobileNetV3-Large is used as the lightweight classification backbone to investigate efficient dermoscopic image classification.
Explanation Efficiency
The paper reports lower latency and peak memory for pruned Grad-CAM and distilled attention compared with standard Grad-CAM.
Resource-Constrained Deployment
The research explores model compression and explanation methods with low-resource and edge-computing settings in mind.
Need for Further Validation
The reported class-level results and the lack of quantitative explanation-fidelity evaluation highlight the need for additional experiments before clinical use.

Tools & Technologies

The paper and associated benchmark workflow reference:
- Python: Main programming language
- PyTorch: Deep-learning framework
- torchvision: Image transforms and model utilities
- MobileNetV3-Large: Lightweight classification backbone
- Grad-CAM: Gradient-based visual explanation
- Knowledge distillation: Student-model training approach
- Structured pruning: Model-compression technique
- INT8 quantization: Lower-precision model representation
- HAM10000: Dermoscopic image dataset
- scikit-learn: Classification metrics
- NumPy / pandas: Numerical processing and result tables
- THOP: Optional MACs/FLOPs estimation
- Pillow: Image handling
Confirm package versions and the exact implemented methods in the repository before publication.

Key Deliverables

A complete repository for this research may include:
- Dataset preparation instructions
- Image preprocessing pipeline
- MobileNetV3-Large classification model
- Training and evaluation scripts
- Structured-pruning implementation
- Knowledge-distillation implementation
- Quantization workflow
- Grad-CAM explanation module
- Pruned Grad-CAM implementation
- Distilled attention explanation module
- Benchmarking script
- Classification and efficiency metrics
- Example heatmaps and visualizations
- Reproducibility instructions
- Research paper and documentation
Only list a deliverable as implemented when its source code or artifact is actually included.

Project Structure

A suggested repository layout is:
xai-lite/
├── README.md
├── requirements.txt
├── configs/
│   └── default.yaml
├── data/
│   └── README.md
├── src/
│   ├── models/
│   ├── data/
│   ├── explainability/
│   ├── compression/
│   └── evaluation/
├── scripts/
│   ├── train.py
│   ├── evaluate.py
│   └── benchmark.py
├── tests/
├── results/
│   ├── metrics/
│   └── visualizations/
├── research-paper/
└── .gitignore
This is a suggested structure, not a claim that these files already exist. Adjust it to match the actual repository. Avoid committing the full dataset, large checkpoints, or sensitive information.
Installation & Usage


Limitations

- Class imbalance: The reported F1-score for the df class is 0.00.
- Explanation fidelity: Qualitative heatmap inspection does not establish that highlighted regions are faithful or clinically meaningful.
- External validation: Evaluation on independent datasets and patient cohorts is needed.
- Hardware reproducibility: Efficiency measurements need documented device and software configurations.
- Clinical generalization: Performance on a single dataset may not generalize to other populations, devices, or clinical settings.
- Implementation verification: The actual repository should be checked to confirm which compression and explanation methods are implemented.

Future Scope
Potential next steps include:
- Improve minority-class performance and report per-class sensitivity and specificity.
- Validate explanations against expert annotations or lesion segmentation masks.
- Evaluate on independent dermoscopic datasets.
- Compare compressed models with uncompressed baselines under identical conditions.
- Measure latency, memory, and power on specified edge devices.
- Document model checkpoints, random seeds, preprocessing, and software versions.
- Assess robustness across imaging devices and patient populations.
- Conduct appropriate clinical and regulatory validation before any real-world clinical use.
  
Research Paper
Title: XAI-Lite: A Computationally Efficient Framework for Interpretable Deep Learning in Low-Resource Healthcare Imaging
Research Area: Explainable AI, Deep Learning, Medical Image Analysis, Healthcare Imaging, Model Compression, Edge AI
Add the paper's publication venue, DOI, and public paper link here when available.

Authors
Use the author list and affiliations exactly as they appear in the final paper. They were not included in the information available for this README draft.

Keywords
XAI-Lite, Explainable AI, XAI, Medical Imaging, Skin Lesion Classification, HAM10000, MobileNetV3, Grad-CAM, Knowledge Distillation, Structured Pruning, INT8 Quantization, Edge AI, Healthcare AI, Deep Learning

Responsible Use
This repository is intended for research and educational use. XAI-Lite must not be used as a standalone diagnostic system, to rule out disease, or to make patient-care decisions. Model predictions can be incorrect, and visual heatmaps do not prove that a prediction is correct. Clinical deployment requires appropriate independent validation, expert oversight, and applicable regulatory review.

Citation
If you use this project in academic work, cite the XAI-Lite paper. Add the complete citation here once the publication details are confirmed. Clearly distinguish the paper's reported results from results reproduced using your own code, data splits, checkpoints, and hardware.
