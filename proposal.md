# Project Proposal: Headset-less VR (Deep Monocular Parallax)

**Student:** Angelyn Jimeno
**Date:** December 11, 2025

## 1. Project Title
**Deep Monocular Parallax: Real-Time Headset-less VR for Immersive Focus**

## 2. Problem Statement
**The Problem:** Traditional Virtual Reality (VR) requires expensive, bulky headsets that isolate users from their surroundings, making them impractical for daily work or study. Conversely, standard 2D screens are static and lack depth cues, leading to "flat" engagement and cognitive drift.
**Motivation:** This project aims to bridge the gap by developing a "Window Effect" interface. By leveraging Deep Learning to track head position in real-time, standard 2D monitors can behave like 3D holographic displays. This adds immersion to the desktop experience, potentially serving as a "focus tool" that visually locks the user's attention to their workspace during study sessions.

## 3. Objectives
* **Deep Learning Pipeline:** Implement a low-latency face tracking system using **MediaPipe Face Mesh** to infer 468 facial landmarks from a standard webcam feed.
* **Coordinate Mapping:** Develop a logic system to translate 2D facial landmarks into 3D camera movements (X, Y, Z position and rotation), converting the user's head movements into a virtual camera vector.
* **Python-Based Rendering:** Integrate the tracking loop with a Python 3D library (e.g., PyOpenGL or Ursina) to dynamically update the view matrix and render the parallax effect within a single application window.
* **Validation:** Evaluate the system's stability by measuring signal noise (jitter) across different lighting conditions and head angles to ensure a smooth user experience.


## 4. Dataset Plan
* **Primary Source:** I will utilize the pre-trained **Google MediaPipe Face Mesh model**, which was trained on over 30,000 "in-the-wild" images with annotated 3D mesh coordinates.
* **Validation Data:** I will record a custom validation dataset (approx. 50 video clips) of myself in a "study environment" under varying lighting (daylight vs. lamp) to test the robustness of the tracking and tune the smoothing filters.

## 5. Technical Approach
* **Architecture:**
    * **Input:** 720p Webcam Stream (RGB).
    * **Inference:** MediaPipe Face Mesh (running on CPU).
    * **Logic:** Python script converts face position to 3D coordinates.
    * **Rendering:** The same script updates the camera perspective in **PyOpenGL/Ursina** every frame.
* **Hardware:** Standard Consumer Laptop.
* **Frameworks:** Python, OpenCV, MediaPipe, PyOpenGL (or Ursina Engine).

## 6. Expected Challenges & Mitigations
* **Jitter (Noise):** Neural network predictions fluctuate frame-to-frame.
    * *Mitigation:* Implementation of a **OneEuroFilter** to smooth signal noise without adding significant latency.
* **Latency:** High lag breaks the 3D illusion and causes nausea.
    * *Mitigation:* Using the optimized TFLite backend and running the rendering loop asynchronously.
