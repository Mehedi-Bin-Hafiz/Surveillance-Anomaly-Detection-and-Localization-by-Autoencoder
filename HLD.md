
# High-Level Design (HLD): Surveillance Anomaly Detection and Localization

## 1. System Overview

The system is designed to automatically detect and localize anomalies in surveillance video footage. It operates in two distinct phases: a **Training Phase** where it learns what "normal" activity looks like, and an **Inference Phase** where it uses that knowledge to flag unusual events in new videos. The core of the system is a **Convolutional Autoencoder**, a type of neural network adept at learning efficient data representations.

## 2. Core Components

1.  **Data Preprocessing Module:** Responsible for ingesting raw video files and transforming them into a format suitable for the neural network.
2.  **Autoencoder Model:** The central engine of the system. It consists of an Encoder that compresses video frames into a low-dimensional representation (latent space) and a Decoder that reconstructs the frames from this representation.
3.  **Training Pipeline:** Orchestrates the process of teaching the Autoencoder model using a dataset of "normal" video frames.
4.  **Inference & Anomaly Detection Pipeline:** Uses the trained model to process new videos, calculate reconstruction errors, and identify anomalies.
5.  **Visualization Module:** Generates the final output, such as videos with anomalies highlighted and graphs of the reconstruction error over time.

## 3. Architectural Workflow

The system's architecture can be visualized as two primary workflows:

**A. Training Workflow (Learning Normality)**

```
[Raw "Normal" Videos] -> [1. Data Preprocessing Module] -> [Normalized Frames (.npy)] -> [3. Training Pipeline] -> [Trained Autoencoder Model (.h5)]
                                                                    ^
                                                                    |
                                                         [2. Autoencoder Model]
```

**B. Inference Workflow (Detecting Anomalies)**

```
[New Input Video] -> [1. Data Preprocessing Module] -> [Original Normalized Frames] --+
                                                                                    |
                                                                                    V
                                     [Trained Autoencoder Model (.h5)] -> [Reconstructed Frames]
                                                                                    |
                                                                                    V
                                                 +-> [4. Inference & Anomaly Detection Pipeline] -> [Anomaly Map]
                                                 |              (Calculate Reconstruction Error)
                                                 |
[Original Normalized Frames] --------------------+
                                                 |
                                                 V
                                     [5. Visualization Module] -> [Annotated Output Video/GIF]
                                     (Overlay Anomaly Map)
```

## 4. Component Descriptions

### 4.1. Data Preprocessing Module
*   **Input:** Directory of video files (e.g., `.mp4`).
*   **Process:**
    *   Uses OpenCV to read video files frame by frame.
    *   Samples frames at a consistent rate (e.g., 10 FPS) to ensure uniformity.
    *   Resizes each frame to the model's required input dimension (232x232).
    *   Converts frames from color (BGR) to grayscale.
    *   Normalizes pixel values to a standard range (e.g., [-1, 1]).
*   **Output:** A single NumPy array (`.npy` file) containing all the processed frames, ready for training.

### 4.2. Autoencoder Model
*   **Architecture:** A Convolutional Neural Network (CNN) with a symmetric encoder-decoder structure.
    *   **Encoder:** A series of convolutional and pooling layers that progressively reduce the spatial dimensions of the input frame while increasing the number of feature channels, compressing it into a compact latent vector.
    *   **Decoder:** A series of transposed convolutional layers that upsample the latent vector, attempting to reconstruct the original input frame.
*   **Principle:** The model is trained only on normal data. The compression forces it to learn the most essential features of normality. It will be very good at reconstructing normal frames but will struggle to reconstruct anomalous frames, as it has never seen those patterns before.

### 4.3. Training Pipeline
*   **Input:** The NumPy array of normalized frames from the preprocessing module.
*   **Process:**
    *   Feeds the frames into the Autoencoder model.
    *   The model's objective is to minimize the **reconstruction error** (e.g., Mean Squared Error - MSE) between the input frame and its own output (the reconstructed frame).
    *   This process is repeated for a set number of epochs until the model's loss converges.
*   **Output:** A trained model file (`.h5`) containing the learned weights of the network.

### 4.4. Inference & Anomaly Detection Pipeline
*   **Input:** A new video to be analyzed and the trained `.h5` model.
*   **Process:**
    *   The new video is passed through the same **Data Preprocessing Module**.
    *   Each preprocessed frame is fed into the trained Autoencoder, which generates a reconstructed frame.
    *   The pixel-wise difference (reconstruction error) between the original and reconstructed frame is calculated. This difference map is the **anomaly map**.
    *   A **threshold** is applied. If the average error for a frame is above this threshold, the frame is classified as anomalous. The high-error regions in the anomaly map indicate the *location* of the anomaly.
*   **Output:** A sequence of anomaly maps and a binary classification (normal/anomaly) for each frame.

### 4.5. Visualization Module
*   **Input:** The original video frames and their corresponding anomaly maps.
*   **Process:**
    *   Overlays the anomaly map (often as a colored heatmap) onto the original frame to visually highlight the anomalous regions.
    *   Compiles these annotated frames into an output video or GIF.
    *   Plots the reconstruction error for each frame over time to create a loss graph.
*   **Output:** The final, user-facing results (`output.gif`, `loss_line.gif`).

## 5. Key Technologies
*   **Programming Language:** Python
*   **Core Libraries:**
    *   **TensorFlow/Keras:** For building and training the autoencoder model.
    *   **OpenCV:** For all video and image manipulation tasks.
    *   **NumPy:** For efficient numerical operations and data handling.
    *   **Matplotlib/Seaborn:** For generating performance graphs.
