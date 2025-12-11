# Project Proposal: Headset-less VR (Deep Monocular Parallax)

**Student:** [Your Name]
**Date:** December 11, 2025

## 1. Project Title
**Deep Monocular Parallax: Real-Time Headset-less VR for Immersive Focus**

## 2. Problem Statement
**The Problem:** Traditional Virtual Reality (VR) requires expensive, bulky headsets that isolate users from their surroundings, making them impractical for daily work or study. Conversely, standard 2D screens are static and lack depth cues, leading to "flat" engagement and cognitive drift.

**Motivation:** This project aims to bridge the gap by developing a "Window Effect" interface. By leveraging Deep Learning to track head position in real-time, standard 2D monitors can behave like 3D holographic displays. This adds immersion to the desktop experience, serving as a "focus tool" that visually locks the user's attention to their workspace.

## 3. Objectives
* **Deep Learning Pipeline:** Implement a low-latency face tracking system using **MediaPipe Face Mesh** to infer 468 facial landmarks from a standard webcam feed.
* **Coordinate Mapping:** Develop a logic system to translate 2D facial landmarks into 3D camera movements (X, Y, Z position), converting the user's head movements into a virtual camera vector.
* **Immersive Rendering:** Transmit these vectors via **OSC (Open Sound Control)** to **TouchDesigner** to update the 3D camera perspective in real-time, creating a reactive motion parallax effect.
* **Validation:** Evaluate the system's stability by measuring signal noise (jitter) across different lighting conditions and head angles.

## 4. Dataset Plan
* **Primary Source:** I will utilize the pre-trained **Google MediaPipe Face Mesh model**, which was trained on over 30,000 "in-the-wild" images with annotated 3D mesh coordinates.
* **Validation Data:** I will record a custom validation dataset (approx. 50 video clips) of myself in a "study environment" under varying lighting (daylight vs. lamp) to test the robustness of the tracking and tune the smoothing filters.

## 5. Technical Approach
* **Architecture:**
    * **Input:** 720p Webcam Stream (RGB).
    * **Inference:** MediaPipe Face Mesh (Python/CPU).
    * **Logic:** Python script calculates relative head position $(x, y, z)$.
    * **Bridge:** `python-osc` library sends OSC messages (`/head/pos`) to localhost port 5000.
    * **Output:** **TouchDesigner** receives the data and renders the 3D wireframe scene.
* **Hardware:** Standard Consumer Laptop.
* **Frameworks:** Python, OpenCV, MediaPipe, TouchDesigner (for visualization).

## 6. Expected Challenges & Mitigations
* **Jitter (Noise):** Neural network predictions fluctuate frame-to-frame.
    * *Mitigation:* Implementation of a **OneEuroFilter** to smooth signal noise without adding significant latency.
* **Latency:** High lag breaks the 3D illusion and causes nausea.
    * *Mitigation:* Using the lightweight `python-osc` library and running the tracking loop on a separate thread from the rendering.
