# DEBRI-X: Autonomous Space Debris Removal Simulation

DEBRI-X is an autonomous satellite system designed to detect, capture, and safely remove space debris from Earth orbit. The increasing amount of orbital debris poses a serious threat to operational satellites, spacecraft, and future missions due to the possibility of cascading collisions known as the Kessler Syndrome.

To address this challenge, DEBRI-X integrates Artificial Intelligence, orbital mechanics simulation, robotics, and satellite swarm coordination into a unified debris mitigation platform.

## 1. Project Overview

The system architecture combines several key technologies:
*   **Deep learning-based debris detection** to identify debris objects in orbital imagery.
*   **Orbital mechanics simulation** for trajectory prediction and interception planning.
*   **Robotic capture mechanisms** capable of physically collecting debris in orbit.
*   **Multi-satellite swarm coordination** enabling collaborative debris removal missions.
*   **Dock-and-dump orbital disposal architecture** for safe atmospheric re-entry.

The system is designed to function as an autonomous orbital cleanup network, where multiple satellites cooperate to identify hazardous debris and remove it efficiently.

## 2. Deep Learning Debris Detection System

A core component of DEBRI-X is the AI-based debris detection system, which identifies debris objects from orbital imagery. The model was trained using the **Space Debris Detection Dataset**, a publicly available dataset designed for training object detection algorithms on orbital debris.

### Dataset Statistics
*   **Training Images:** 5,000
*   **Validation Images:** 800
*   **Labeled Debris Bounding Boxes:** 18,896
*   **Average Bounding Box Properties:**
    | Property | Value |
    | :--- | :--- |
    | Average width | 0.0562 |
    | Average height | 0.0550 |
    | Average area | 0.00312 |

These values indicate that the model must detect tiny objects under challenging visual conditions, similar to real orbital imagery.

## 3. Image Processing Pipeline

To improve detection accuracy, a multi-spectrum preprocessing pipeline was implemented. Instead of relying only on standard RGB images, the system generates multiple enhanced visual representations to help the neural network learn debris features under different lighting, contrast, and sensor conditions.

*   **Standard Visual Modes:** RGB image, Grayscale conversion.
*   **Simulated Sensor Modes:** Infrared simulation, Thermal imaging simulation.
*   **Contrast and Feature Enhancement:** CLAHE (Contrast Limited Adaptive Histogram Equalization), Histogram equalization, Image sharpening.
*   **Edge and Structure Enhancement:** Edge enhancement filters, Canny edge detection overlay.
*   **Alternative Representations:** False color mapping, Depth-enhanced visualization.

## 4. Model Architecture and Training

Several state-of-the-art object detection architectures were evaluated, including YOLOv8-Large, YOLOv8-XLarge, YOLOv9-E, YOLOv10-Large, and YOLO11-Large. All models were trained using PyTorch and the Ultralytics YOLO environment.

### Training Configuration
| Parameter | Value |
| :--- | :--- |
| Epochs | 30 |
| Batch size | 8 |
| Image resolution | 640 × 640 |
| Optimizer | AdamW |
| Model Parameters | ~43.6 million |

**Hardware Used:** NVIDIA Tesla T4 GPU (15.6 GB VRAM) with CUDA acceleration.

## 5. Training Performance

The final model achieved exceptional detection accuracy:

| Metric | Value |
| :--- | :--- |
| Precision | 0.9973 |
| Recall | 0.9958 |
| F1 Score | 0.9931 |
| mAP@50 | 0.995 |
| mAP@50-95 | 0.823 |
| Inference Speed | ≈ 38 ms/image |

The system performs near real-time debris detection, critical for autonomous satellite operations.

## 6. AI Detection Pipeline

The workflow operates as a multi-stage process:
1.  **Input Orbital Image:** Satellite cameras capture images of nearby orbital regions.
2.  **Multi-Spectrum Preprocessing:** Enhances the image using the transformations described above.
3.  **YOLO Detection Model:** Processes the enhanced images.
4.  **Bounding Box Prediction:** Identifies the position of debris.
5.  **Debris Localization and Classification:** Analyzes size, location, and risk.
6.  **Threat Prioritization:** Ranks objects based on collision risk and proximity.

## 7. Orbital Mission Simulation (GMAT)

The repository contains several General Mission Analysis Tool (GMAT) scripts to simulate the orbital dynamics and mission sequences of the DEBRI-X system. These scripts are optimized for **GMAT R2025a**.

### Simulation Scripts
*   **`debrix1.script`**: A fundamental simulation of the DEBRIX spacecraft approaching a single target (`Debris_Alpha`). It demonstrates autonomous decision logic for phasing burns and proximity operations.
*   **`debrix2.script`**: The "Junk Cloud" hunter edition. It simulates DEBRIX tracking and de-orbiting a primary target within a cloud of four debris objects, featuring multi-plot visualizations for range, fuel, and altitude.
*   **`debrix_full_simulation.script`**: A comprehensive research version featuring a swarm of 10 DEBRIX units and 10 debris objects. It includes complex "Dock-and-Dump" logic, multiple orbit views, and ground track plots.
*   **`debrix_normal.script`**: An optimized version of the full swarm simulation, reduced to essential visualizations to ensure stability in GMAT while maintaining full mission logic.

## 8. Physics-Based Robotic Simulation

A physics simulation environment was developed using **PyBullet** to validate capture mechanisms.
*   **Robotic Arm Capture:** Simulations of a Canadarm-inspired arm with joint torque limits and inverse kinematics.
*   **Microgravity Environment:** Replicates zero-gravity dynamics and momentum-based interactions.
*   **Debris Field Simulation:** Objects are categorized into High, Medium, and Low risk threat levels for testing target prioritization.

## 9. Dock-and-Dump Disposal Strategy

The DEBRI-X system uses a "dock-and-dump" strategy to maximize efficiency:
1.  **Detection:** AI identifies debris in orbit.
2.  **Capture:** Specialized capture satellites intercept and store debris.
3.  **Docking:** Capture satellites dock with a dedicated **Aggregation (Dump) Satellite**.
4.  **Transfer:** Debris is transferred to the dump satellite's storage.
5.  **Disposal:** Once at capacity, the dump satellite performs a controlled atmospheric re-entry, where the debris burns up safely.

This architecture allows a single disposal system to handle debris collected by multiple capture units, improving mission scalability.
