# Deep Monocular Parallax: Real-Time Headset-less VR for Immersive Focus
**CSC173 Intelligent Systems Final Project** *Mindanao State University - Iligan Institute of Technology* **Student:** [Your Full Name], [Student ID]  
**Semester:** AY 2025-2026 Sem 1  
[![Python](https://img.shields.io/badge/Python-3.8+-blue)](https://python.org) [![MediaPipe](https://img.shields.io/badge/MediaPipe-0.10-teal)](https://developers.google.com/mediapipe)

## Abstract
Traditional Virtual Reality (VR) relies on expensive, bulky headsets that isolate users from their surroundings, making them impractical for daily study or work. This project introduces **Deep Monocular Parallax**, a "Window Effect" interface that transforms standard 2D monitors into immersive holographic displays. By utilizing the **Google MediaPipe Face Mesh** architecture to infer 468 facial landmarks from a standard webcam feed, the system calculates real-time 6-DoF head vectors. These vectors are transmitted via OSC (Open Sound Control) to a rendering engine, dynamically updating the camera frustum to create motion parallax depth cues. This system aims to reduce cognitive drift and enhance focus without the physical burden of wearable hardware.

## Table of Contents
- [Introduction](#introduction)
- [Related Work](#related-work)
- [Methodology](#methodology)
- [Ethical Considerations](#ethical-considerations)
- [Installation](#installation)
- [References](#references)

## Introduction
### Problem Statement
Standard desktop study environments suffer from "flat" engagement; 2D screens lack the depth cues natural to human perception, contributing to cognitive drift. While VR offers immersion, headsets are too isolating and cumbersome for long study sessions. There is a need for a "middle ground"—a headset-less, immersive interface that locks user attention to the workspace using existing hardware.

### Objectives
- **Objective 1:** Implement a low-latency face tracking pipeline using **MediaPipe Face Mesh**.
- **Objective 2:** Develop a geometric solving algorithm to translate 2D facial landmarks into a 3D camera vector $(x, y, z)$.
- **Objective 3:** Transmit tracking data via OSC to a 3D engine (Godot/TouchDesigner) to render real-time off-axis projection.

![Problem Demo](images/problem_example.gif)

## Related Work
- **Paper 1:** *Real-time Facial Surface Geometry from Monocular Video on Mobile GPUs [1]* - Describes the underlying mesh topology used for tracking.
- **Paper 2:** *1 € Filter: A Simple Speed-based Low-pass Filter [2]* - Methodology used to stabilize jitter in the neural network output.
- **Gap:** Most head-tracking solutions require specialized infrared hardware (e.g., TrackIR); this project achieves similar results using **deep learning on standard webcams**.

## Methodology
### Dataset
- **Primary Source:** Pre-trained **Google MediaPipe Face Mesh model**, trained on ~30,000 "in-the-wild" images with 3D coordinate annotations.
- **Validation:** A custom dataset of 50 video clips recorded in the author's study environment to tune smoothing parameters against variable lighting conditions.
- **Preprocessing:** Frame resizing to 256x256, RGB normalization.

### Architecture
![Model Diagram](images/architecture.png)
- **Backbone:** MobileNetV2 (optimized for CPU inference).
- **Head:** Multi-task regression head predicting 468 3D landmarks.
- **Logic:** Perspective-n-Point (PnP) solver converts landmarks to head pose vectors.
- **Hyperparameters:** (Smoothing Filter)

| Parameter | Value |
|-----------|-------|
| Min Cutoff Frequency | 1.0 Hz |
| Beta (Speed Coeff) | 0.05 |
| FPS Target | 30/60 |
| d_cutoff | 1.0 |

## Ethical Considerations
- **Bias:** The MediaPipe model is generally robust, but performance may degrade under extreme lighting or for users with heavy facial occlusion (e.g., thick glasses). The validation set is limited to the author's environment.
- **Privacy:** This system is "Privacy by Design." All video processing occurs locally on the CPU; no video feed or biometric data is ever transmitted to a cloud server or stored permanently.
- **Misuse:** While designed for focus assistance, head-tracking technology can potentially be repurposed for non-consensual attention monitoring in workplaces. This project opposes such use cases.

## Installation
1.  **Clone repo:**
    ```bash
    git clone [https://github.com/yourusername/Deep-Monocular-Parallax](https://github.com/yourusername/Deep-Monocular-Parallax)
    ```
2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run the Tracker:**
    ```bash
    python main.py --camera 0 --ip 127.0.0.1 --port 5005
    ```
4.  **Launch Visualization:** Open the included Godot project or TouchDesigner file to see the parallax effect.

**requirements.txt:**
```text
mediapipe>=0.10.0
opencv-python
python-osc
numpy
scipy
  ```

## References
[1] Kartynnik, Y., Ablavatski, A., Grishchenko, I., & Grundmann, M. "Real-time Facial Surface Geometry from Monocular Video on Mobile GPUs," arXiv preprint arXiv:1907.06724, 2019.

[2] Casiez, G., Roussel, N., & Vogel, D. "1 € Filter: A Simple Speed-based Low-pass Filter for Noisy Input in Interactive Systems," CHI Conference, 2012.
