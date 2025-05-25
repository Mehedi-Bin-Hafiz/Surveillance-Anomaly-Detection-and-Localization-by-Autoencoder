# Anomaly Detection and Localization

![Surveillance Anomaly Localization](results/output.gif "anomaly sample")

## Project Overview

This project leverages a **Long Short-Term Memory (LSTM) Autoencoder** to detect and localize anomalies in surveillance video data. The model flags anomalies based on reconstruction error thresholds by reconstructing normal sequences. It's ideal for tasks like identifying unusual activities in video footage.

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

## Dataset is taken from

```bash
Anomaly Detection in Crowded Scenes.
V. Mahadevan, W. Li, V. Bhalodia and N. Vasconcelos.
In Proc. IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 
San Francisco, CA, 2010
```