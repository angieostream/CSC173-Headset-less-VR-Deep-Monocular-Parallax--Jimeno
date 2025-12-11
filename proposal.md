# Project Proposal: Headset-less VR (Deep Monocular Parallax)

**Student:** Angelyn Jimeno
**Date:** December 11, 2025

## 1. Project Title
**Deep Monocular Parallax: Real-Time Headset-less VR for Immersive Focus**

## 2. Problem Statement
**The Problem:** Traditional Virtual Reality (VR) requires expensive, bulky headsets that isolate users from their surroundings, making them impractical for daily work or study. Conversely, standard 2D screens are static and lack depth cues, leading to "flat" engagement and cognitive drift.
**Motivation:** This project aims to bridge the gap by developing a "Window Effect" interface. By leveraging Deep Learning to track head position in real-time, standard 2D monitors can behave like 3D holographic displays. This adds immersion to the desktop experience, potentially serving as a "focus tool" that visually locks the user's attention to their workspace during study sessions.

## 3. Objectives
* **Deep Learning Pipeline:** Implement a low-latency face tracking system using **MediaPipe Face Mesh** to infer 468 facial landmarks from a monocular webcam feed.
* **Geometric Solving:** Develop a mapping algorithm (using Perspective-n-Point or relative centroid logic) to translate 2D facial coordinates into a 6-DoF (Degrees of Freedom) camera vector.
* **Immersive Rendering:** transmit these vectors via OSC (Open Sound Control) to a rendering engine (TouchDesigner/Godot) to update the camera frustum in real-time, creating motion parallax.
* **Validation:** Evaluate the system's stability by measuring jitter/noise across different lighting conditions and head angles.

## 4. Dataset Plan
* **Primary Source:** I will utilize the pre-trained **Google MediaPipe Face Mesh model**, which was trained on over 30,000 "in-the-wild" images with annotated 3D mesh coordinates.
* **Validation Data:** I will record a custom validation dataset (approx. 50 video clips) of myself in a "study environment" under varying lighting (daylight vs. lamp) to test the robustness of the tracking and tune the smoothing filters.

## 5. Technical Approach
* **Architecture:**
    * **Input:** 720p Webcam Stream (RGB).
    * **Inference:** MediaPipe Attention Mesh (MobileNetV2 backbone).
    * **Logic:** Python script calculates relative head position $(x, y, z)$.
    * **Output:** OSC messages (`/head/x`, `/head/y`) sent to the 3D Engine.
* **Hardware:** Standard Consumer Laptop (CPU inference).
* **Frameworks:** Python, OpenCV, MediaPipe, TouchDesigner (for visualization).

## 6. Expected Challenges & Mitigations
* **Jitter (Noise):** Neural network predictions fluctuate frame-to-frame.
    * *Mitigation:* Implementation of a **OneEuroFilter** to smooth signal noise without adding significant latency.
* **Latency:** High lag breaks the 3D illusion and causes nausea.
    * *Mitigation:* Using the optimized TFLite backend and running the rendering loop asynchronously.
