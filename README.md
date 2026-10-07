# Mobile-Robots-Project

[![ROS](https://img.shields.io/badge/ROS-Noetic%20%2F%20Melodic-orange.svg)](https://www.ros.org/)
[![Python](https://img.shields.io/badge/Python-3.12-blue.svg)](https://www.python.org/)
[![Hardware](https://img.shields.io/badge/Hardware-Raspberry%20Pi%203B%20%7C%20Arduino-red.svg)](https://www.raspberrypi.com/)
[![Model](https://img.shields.io/badge/VLM-Moondream%200.5B-green.svg)](https://github.com/vikhyat/moondream)

> Mobile Robot with Vision-Language Model (VLM) code for edge visual scene understanding and object captioning.

---

## 📌 Overview
Implementing Vision-Language Model (Moondream VLM) into a Mobile Robot. This project investigates the feasibility of running lightweight multi-modal models on resource-constrained embedded systems and compares edge inference with base-station offloading via ROS.

* **Collaborators**: [Frank Liu](https://github.com/Frank-Liu-0118), [@jychpr](https://github.com/jychpr)

---

## 🎬 Demo & Results

[![Watch the Demo](https://img.youtube.com/vi/TWx1p8HYpFw/hqdefault.jpg)](https://youtu.be/TWx1p8HYpFw)

*(Click the image above to watch the full demonstration on YouTube)*

* **Test Scenario**: Real-time object recognition on a desktop setting using an onboard camera.
* **Prompt**: `"what is the objects in the image"`
* **Terminal Output**:
  ```text
  Answer: The objects in the image are a can of tomato juice, a bottle of water, and a bottle of soda.
  ```

---

## ⚙️ Specs - Mobile Robot

| Component | Specification | Description |
| :--- | :--- | :--- |
| **Microcontroller** | Arduino Uno | Motor driving and low-level chassis control |
| **SBC (Edge Compute)** | Raspberry Pi 3 Model B | Edge computing node (ARM Cortex-A53, 1GB RAM) |
| **Camera** | Raspberry Pi Camera Module V2 | Real-time image capture (`camimage.jpg`) |
| **VLM Model** | Moondream 0.5B | INT8 quantized model (`moondream-0_5b-int8.mf`) |
| **Middleware** | ROS (Robot Operating System) | Inter-device topic communication |

---

## 🏗️ Setup & Architecture

### Scenario 1: VLM Running inside Raspberry Pi 3 Model B
The VLM runs locally inside the Raspberry Pi 3 Model B Python virtual environment (`tpvlm`). The generated caption text is published to the base station via ROS.

![Scenario 1](assets/TPFINAL-Scenario-1.png)

---

### Scenario 2: Mobile Robot send image to base station and VLM on base station
The mobile robot captures images and publishes them over the ROS network. The heavier VLM inference is handled externally by the laptop/PC base station.

![Scenario 2](assets/TPFINAL-Scenario-2.png)

---

## 📂 Pre-requisite & Directory Structure

Download the VLM model from [vikhyat/moondream](https://github.com/vikhyat/moondream) and place it inside the `models` folder.

```text
├── assets/
│   ├── Scenario 1.png
│   └── Scenario 2.png
├── base_station/
│   ├── Image_talker_listener/   # ROS package for base station in laptop or PC
│   └── MR-TP-FIX/               # Python environment setup and requirements
├── mobile_robot/
│   ├── Image_talker_listener/   # ROS package for mobile robot
│   └── tpvlm/                   # VLM experimentation within Raspberry Pi 3 Model B
└── README.md
```

---

## 🚀 Quick Start (Demo Workflow)

Based on the demonstrated on-device execution on the Raspberry Pi 3 Model B:

### 1. Environment & Directory Setup
Open a terminal on the Raspberry Pi Desktop and activate the Python virtual environment:
```bash
# Activate virtual environment
source activate tpvlm

# Navigate to the VLM experimentation directory
cd tpvlm

# (Optional) Install dependencies if running for the first time
pip install -r requirements.txt
```

### 2. Verify Captured Frame
Ensure the camera snapshot (`camimage.jpg`) has been captured and placed inside the working directory.

### 3. Run Inference
Execute the Moondream VLM script to query the captured image:
```bash
python vlm.py
```

### 4. Expected Output
The script loads the INT8 quantized model (`moondream-0_5b-int8.mf`) and prints the recognized scene description:
```text
Answer: The objects in the image are a can of tomato juice, a bottle of water, and a bottle of soda.
```

---

## 📊 Results & Benchmark Findings

* **Feasibility**: Moondream VLM 0.5B can run on Raspberry Pi 3 Model B without memory crashes using INT8 quantization.
* **Processing Time**: ~8-10 minutes on Raspberry Pi 3 Model B CPU.
* **Architecture Comparison**:
  * **Scenario 1 (Edge Computing)**: Completely self-contained onboard without relying on high-bandwidth wireless links, but limited by embedded CPU compute latency.
  * **Scenario 2 (Base Station Offload)**: Significantly faster inference time by utilizing PC compute resources, requiring reliable image data transmission over the ROS network.
