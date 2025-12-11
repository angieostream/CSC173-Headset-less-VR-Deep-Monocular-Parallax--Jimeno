# [Project Title: e.g., FocusFlow - Intelligent Study Assistant]
**CSC173 Intelligent Systems Final Project** *Mindanao State University - Iligan Institute of Technology* **Student:** [Your Full Name], [Student ID]  
**Semester:** [e.g., AY 2025-2026 Sem 1]  
[![Python](https://img.shields.io/badge/Python-3.8+-blue)](https://python.org) [![PyTorch](https://img.shields.io/badge/PyTorch-2.0-orange)](https://pytorch.org)

## Abstract
This project develops an intelligent computer vision system designed to monitor and enhance study habits. Using deep learning techniques (YOLOv8 fine-tuned on custom datasets), the system detects real-time distractions such as phone usage or drowsiness. The primary goal is to provide a technological solution to the personal challenge of maintaining academic focus. **This project is deeply personal; it aims to help me study more effectively, maintain focus, and manage my workload by automating the detection of my own distraction habits.**

## Table of Contents
- [Introduction](#introduction)
- [Related Work](#related-work)
- [Methodology](#methodology)
- [Ethical Considerations](#ethical-considerations)
- [Installation](#installation)
- [References](#references)

## Introduction
### Problem Statement
Maintaining deep focus for academic study is increasingly difficult due to digital distractions and fragmented attention spans. Existing tools rely on manual input and lack the capability to passively monitor and correct behavior in real-time.

### Personal Motivation
This project is built to help me **study more effectively, focus, and manage my time**. By automating the detection of my own distraction habits, I am building a system that directly supports my academic goals and acts as an impartial accountability partner.

### Objectives
- **Objective 1:** Develop a system to detect "Phone Usage" and "Drowsiness" with >90% accuracy.
- **Objective 2:** Create a personal "Focus" aid that helps me study and stay on track.
- **Objective 3:** Deploy the model for real-time inference on a local machine.

![Problem Demo](images/problem_example.gif)

## Related Work
- **Paper 1:** *Real-time Gaze Tracking for E-Learning [1]* - Discusses attention metrics.
- **Paper 2:** *YOLOv8 for Object Detection [2]* - Baseline architecture.
- **Gap:** Most systems are for classroom surveillance; this project focuses on **self-regulated, personal study assistance**.

## Methodology
### Dataset
- **Source:** Custom dataset (Webcam captures) + Public Drowsiness Detection dataset.
- **Split:** 70/15/15 train/val/test.
- **Preprocessing:** Grayscale conversion, mosaic augmentation, resizing to 640x640.

### Architecture
![Model Diagram](images/architecture.png)
- **Backbone:** CSPDarknet53
- **Head:** YOLO detection layers (Custom classes: Focused, Phone, Drowsy)
- **Hyperparameters:** Table below

## Ethical Considerations
- **Bias:** The dataset is currently biased towards the author's specific appearance and room lighting conditions. The model may not generalize well to other users without retraining.
- **Privacy:** All video processing is performed locally on the machine. No image data is uploaded to the cloud, ensuring complete user privacy.
- **Misuse:** This tool is designed strictly for personal self-improvement and productivity. It should not be used for unauthorized surveillance or employee monitoring.

## Installation
1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/yourusername/CSC173-DeepCV-YourLastName](https://github.com/yourusername/CSC173-DeepCV-YourLastName)
    ```
2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Download weights:**
    Navigate to the `models/` directory or run the helper script:
    ```bash
    ./download_weights.sh
    ```


### requirements.txt
torch>=2.0
ultralytics
opencv-python
albumentations
pandas

### References
Jocher, G., et al. "YOLOv8," Ultralytics, 2023.

Smith, A. "Attention Mechanisms in Computer Vision," IEEE Transactions on Pattern Analysis and Machine Intelligence, 2022.
