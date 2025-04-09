# Surveillance Anomaly Detection by Autoencoder

![Surveillance Anomaly Detection](loss_graph.jpeg "Loss Graph")

## Project Overview

This project leverages a **Long Short-Term Memory (LSTM) Autoencoder** to detect anomalies in surveillance video data. By reconstructing normal sequences, the model flags anomalies based on reconstruction error thresholds. It's ideal for tasks like identifying unusual activities in video footage.

---

## Features

- **LSTM Autoencoder**: Harnesses temporal patterns for anomaly detection.
- **Preprocessing Pipeline**: Streamlines data normalization and sequence preparation.
- **Visualization**: Includes loss graphs for performance tracking.
- **Customizable Threshold**: Tweak anomaly sensitivity via reconstruction error thresholds.
- **Exportable Model**: Save trained models for future inference.

---

## File Descriptions

| File/Folder              | Description                                                    |
|--------------------------|----------------------------------------------------------------|
| `data_preprocessing.ipynb` | Data preprocessing pipeline: resizing, normalization, and sequence generation. |
| `model_train.ipynb`      | Builds, trains, and saves the LSTM Autoencoder model.         |
| `model_evaluation.ipynb` | Evaluates model performance and identifies anomalies.         |
| `loss_graph.jpeg`        | Training and validation loss graph.                          |
| `requirements.txt`       | Python dependencies list.                                    |
| `saved_model.h5`         | Pretrained model file.                                       |

---

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/Surveillance-Anomaly-Detection-by-Autoencoder.git

2. Navigate to the project directory:

```bash
cd Surveillance-Anomaly-Detection-by-Autoencoder

```

3. Install dependencies:

```bash
pip install -r requirements.txt

```