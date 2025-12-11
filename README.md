# Deep Monocular Parallax: Real-Time Headset-less VR
**CSC173 Intelligent Systems Final Project** *Mindanao State University - Iligan Institute of Technology* **Student:** [Your Full Name], [Student ID]  
**Semester:** AY 2025-2026 Sem 1  
[![Python](https://img.shields.io/badge/Python-3.10+-blue)](https://python.org) [![MediaPipe](https://img.shields.io/badge/MediaPipe-0.10-teal)](https://developers.google.com/mediapipe) [![TouchDesigner](https://img.shields.io/badge/TouchDesigner-2023-orange)](https://derivative.ca/)

## Abstract
Traditional Virtual Reality (VR) relies on bulky headsets that isolate users from their surroundings. This project implements **Deep Monocular Parallax**, a desktop-based system that creates a "holographic" 3D illusion on a standard 2D monitor. By using **Google MediaPipe Face Mesh** to track the user's head position in real-time, the system transmits coordinates via OSC (Open Sound Control) to **TouchDesigner**. The outcome is a reactive visual environment—featuring a retro-wave wireframe grid—that shifts responsively to the user's movement, simulating depth and immersion without wearable hardware.

## Table of Contents
- [Introduction](#introduction)
- [Related Work](#related-work)
- [Methodology](#methodology)
- [Ethical Considerations](#ethical-considerations)
- [Installation](#installation)
- [References](#references)

## Introduction
### Problem Statement
Standard computer interfaces are static and lack depth cues, creating a "flat" experience that fails to engage spatial perception. While VR solves this, it requires expensive hardware. There is a need for a lightweight solution that turns a standard laptop screen into a spatially aware "window" to enhance user engagement.

### Objectives
- **Objective 1:** Implement a robust face-tracking pipeline using **MediaPipe** to extract 3D head coordinates (x, y, z) from a webcam feed.
- **Objective 2:** Establish a low-latency **OSC (Open Sound Control)** bridge to transmit tracking data from Python to TouchDesigner.
- **Objective 3:** Create a generative 3D environment in **TouchDesigner** (wireframe grid/horizon) that dynamically adjusts its camera perspective based on the received coordinates.

![Problem Demo](images/problem_example.gif)

## Related Work
- **Paper 1:** *Real-time Facial Surface Geometry from Monocular Video on Mobile GPUs [1]* - The foundation for the mesh tracking used in this project.
- **Paper 2:** *The Use of Motion Parallax for Spatial Perception on 2D Displays [2]* - Validates the concept of using head-tracking to simulate depth on flat screens.
- **Gap:** Most existing parallax implementations are closed-source or require gaming engines; this project demonstrates a flexible **creative coding workflow** using TouchDesigner for rapid visual prototyping.

## Methodology
### Dataset
- **Primary Source:** Pre-trained **Google MediaPipe Face Mesh**, capable of detecting 468 facial landmarks.
- **Validation:** Real-time testing performed in a controlled study environment to tune sensitivity and smoothing parameters.
- **Preprocessing:** Webcam input is downscaled to 480p for faster inference before being processed by the neural network.

### Architecture
![Model Diagram](images/architecture.png)
- **Input:** Webcam Stream (OpenCV).
- **Inference:** MediaPipe Face Mesh (Python/CPU).
- **Bridge:** `python-osc` library sends `/head/pos` messages to localhost port 5000.
- **Rendering:** **TouchDesigner** receives OSC data and drives a Camera COMP to render the 3D geometry.

**Hyperparameters (Smoothing):**
| Parameter | Value |
|-----------|-------|
| Smoothing Algorithm | OneEuroFilter |
| Beta (Jitter Reduction) | 0.05 |
| OSC Update Rate | 60 Hz |

## Ethical Considerations
- **Bias:** The tracking model may exhibit varied performance based on lighting conditions and facial obstructions (e.g., masks or heavy glasses).
- **Privacy:** This application follows a "Privacy by Design" approach. All video processing is performed locally in volatile memory; no video data is saved to disk or transmitted to the internet.
- **Misuse:** This technology is intended for interface enhancement and creative expression, not for surveillance or behavioral monitoring.

## Installation
### Prerequisites
* Python 3.8+
* TouchDesigner (Non-Commercial or Commercial License)
* Webcam

### Steps
1.  **Clone repo:**
    ```bash
    git clone [https://github.com/yourusername/Deep-Monocular-Parallax](https://github.com/yourusername/Deep-Monocular-Parallax)
    ```
2.  **Install Python dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Launch the Visuals:**
    Open `visualization/parallax_grid.toe` in TouchDesigner. It is pre-configured to listen for OSC on port 5000.
4.  **Run the Tracker:**
    ```bash
    python main.py --ip 127.0.0.1 --port 5000
    ```

**requirements.txt:**
```text
mediapipe
opencv-python
python-osc
numpy
  ```
### References
[1] Kartynnik, Y., et al. "Real-time Facial Surface Geometry from Monocular Video on Mobile GPUs," arXiv preprint arXiv:1907.06724, 2019.

[2] Rogers, S., et al. "Motion Parallax as an Independent Cue for Depth Perception," Perception, 1979.
