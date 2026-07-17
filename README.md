# 📡 Automatic Modulation Classification (AMC)

> A deep learning–based system that automatically identifies the modulation type of raw radio signals (IQ samples) — without any prior knowledge about the signal — using CNN and CNN-LSTM architectures, with an interactive Streamlit GUI.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-ee4c2c?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-GUI-ff4b4b?logo=streamlit&logoColor=white)](https://streamlit.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 🎯 Overview

**Automatic Modulation Classification (AMC)** is a key task in signal processing where the goal is to take a raw radio signal — represented as IQ (In-phase & Quadrature) samples — and determine which modulation scheme was used to transmit it, all without any prior information about the signal.

This project implements an end-to-end AMC pipeline:

1. **Upload** a signal file in any of the 4 supported formats  
2. **Preprocess** and normalize the signal into a unified representation  
3. **Classify** using a trained deep learning model (or compare two models side-by-side)  
4. **Visualize** the results with confidence scores, constellation diagrams, time-domain plots, and spectrograms  

### Why AMC Matters

- **Cognitive Radio** — Smart wireless networks that detect and use available frequencies autonomously  
- **Spectrum Monitoring** — Regulatory agencies monitoring frequency usage  
- **Military & Intelligence** — Analyzing unknown intercepted signals  
- **Bridging Theory & Practice** — Connects Communications Theory with Deep Learning for real-world RF problems  

---

## ✨ Features

- 🎛️ **11 Modulation Types** — Classifies: `8PSK`, `AM-DSB`, `AM-SSB`, `BPSK`, `CPFSK`, `GFSK`, `PAM4`, `QAM16`, `QAM64`, `QPSK`, `WBFM`
- 📂 **4 Input Formats** — Supports Raw Binary (`.iq`), MATLAB (`.mat`), CSV (`.csv`), and NumPy (`.npy`) files
- 🧠 **2 Model Architectures** — Baseline CNN vs. CNN+LSTM Hybrid for comparison
- 🖥️ **Interactive GUI** — Streamlit-based web interface with model selection, file upload, and result visualization
- 📊 **Rich Visualizations** — Constellation diagrams, time-domain plots, spectrograms, and confidence bar charts
- 📈 **Comprehensive Evaluation** — Confusion matrices, Accuracy vs. SNR curves, per-class precision/recall/F1

---

## 🖼️ Demo

<!-- TODO: Add a screenshot or GIF of the GUI in action -->
> *A demo screenshot/GIF will be added after the GUI is complete.*

---

## 🛠️ Installation

### Prerequisites

- [Anaconda](https://www.anaconda.com/download) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html)
- Git

### Step 1 — Clone the Repository

```bash
git clone https://github.com/engatemam/Automatic-Modulation-Classification-AMC.git
cd Automatic-Modulation-Classification-AMC
```

### Step 2 — Create a Conda Environment

```bash
conda create -n modclassifier python=3.10 -y
conda activate modclassifier
```

### Step 3 — Install PyTorch

**With NVIDIA GPU (CUDA):**
```bash
# Visit https://pytorch.org/get-started/locally/ for the exact command for your CUDA version
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

**CPU Only:**
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
```

### Step 4 — Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 5 — Download the Dataset

Download the **RadioML2016.10a** dataset and place it in the `data/raw/` directory.

> 📥 [RadioML2016.10a on DeepSig](https://www.deepsig.ai/datasets)

---

## 🚀 Usage

### Run the GUI

```bash
streamlit run gui/app.py
```

Then open your browser at `http://localhost:8501`.

### How to Use

1. **Select a model** from the sidebar (Model 1, Model 2, or Comparison Mode)  
2. **Choose the input file format** from the dropdown  
3. **Upload your signal file** (only the matching file extension will be accepted)  
4. **Click "Classify"** and view the results:
   - Predicted modulation type with confidence score
   - Top-3 predictions bar chart
   - Constellation Diagram, Time-Domain Plot, and Spectrogram tabs

### Train a Model

```bash
python training/train.py
```

### Evaluate a Model

```bash
python training/evaluate.py
```

---

## 📁 Project Structure

```
Automatic-Modulation-Classification-AMC/
│
├── data/
│   ├── raw/                    # RadioML2016.10a dataset (not tracked by git)
│   ├── test_samples/           # Test files in all 4 supported formats
│   └── scripts/                # Data preparation & generation scripts
│
├── utils/
│   ├── __init__.py
│   ├── format_readers.py       # Read & convert all 4 input formats to unified IQ
│   ├── format_exporters.py     # Export samples to any of the 4 formats
│   └── preprocessing.py        # Normalization, reshaping, one-hot encoding
│
├── models/
│   ├── __init__.py
│   ├── baseline_cnn.py         # Model 1: Baseline 2D-CNN architecture
│   ├── cnn_lstm.py             # Model 2: CNN + LSTM Hybrid architecture
│   └── checkpoints/            # Saved best model weights (.pt files)
│
├── training/
│   ├── __init__.py
│   ├── train.py                # Training loop, early stopping, checkpointing
│   ├── evaluate.py             # Evaluation: confusion matrix, accuracy vs SNR, metrics
│   └── dataset.py              # PyTorch Dataset class & data loading utilities
│
├── gui/
│   └── app.py                  # Streamlit GUI application
│
├── notebooks/                  # Jupyter notebooks for data exploration
├── docs/                       # Report, Presentation, and figures
├── tests/                      # Unit tests for core modules
│
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

## 🧠 Model Architectures

### Model 1 — Baseline CNN

A 2D Convolutional Neural Network that treats the IQ matrix (2×128) as a small "image":

| Layer | Details |
|-------|---------|
| Conv2D | 256 filters, kernel (1×3), ReLU, Dropout 50% |
| Conv2D | 80 filters, kernel (2×3), ReLU, Dropout 50% |
| Flatten | Reshape to 1D vector |
| Dense | 256 units, ReLU, Dropout 50% |
| Output | 11 units, Softmax |

### Model 2 — CNN + LSTM Hybrid

Combines convolutional feature extraction with temporal sequence modeling via LSTM:

| Layer | Details |
|-------|---------|
| Conv1D | 64 filters, kernel size 8, BatchNorm, MaxPool |
| Conv1D | 128 filters, kernel size 5, BatchNorm |
| LSTM | 128 hidden units |
| Dense | 128 units, ReLU, Dropout 50% |
| Output | 11 units, Softmax |

> **Why two models?** Comparing a simpler CNN with a more complex CNN+LSTM hybrid reveals when the added complexity of temporal modeling (LSTM) provides real accuracy gains — and when the simpler model is sufficient.

---

## 📊 Model Performance

<!-- TODO: Fill in after training is complete -->

| Metric | Baseline CNN | CNN + LSTM |
|--------|:------------:|:----------:|
| Overall Accuracy (SNR ≥ 10dB) | — | — |
| Overall Accuracy (All SNR) | — | — |
| Best Per-Class Accuracy | — | — |
| Training Time | — | — |

> *Results will be updated after model training is complete.*

---

## 📂 Supported Input Formats

| # | Format | Extension | Description |
|---|--------|-----------|-------------|
| 1 | Raw Binary | `.iq` | Interleaved I/Q float32 samples, no header |
| 2 | MATLAB | `.mat` | MATLAB file with complex IQ or separate I & Q variables |
| 3 | CSV | `.csv` | Two-column text file (I, Q) with optional header |
| 4 | NumPy | `.npy` | NumPy array — complex, (2×N), or (N×2) shape |

All formats are internally converted to a unified **2×128** matrix (I row + Q row) before classification.

---

## 📦 Dataset

**RadioML2016.10a** — A widely-used benchmark dataset for AMC research.

| Property | Value |
|----------|-------|
| Modulation Types | 11 (8PSK, AM-DSB, AM-SSB, BPSK, CPFSK, GFSK, PAM4, QAM16, QAM64, QPSK, WBFM) |
| SNR Range | -20 dB to +18 dB (step 2 dB → 20 levels) |
| Samples per (mod, SNR) | 1,000 |
| Total Samples | 220,000 |
| Sample Shape | 2 × 128 (I row + Q row) |
| File Format | Python Pickle (.pkl) |

---

## 🏋️ Training Details

| Parameter | Value |
|-----------|-------|
| Loss Function | Cross-Entropy Loss |
| Optimizer | Adam (lr = 0.001) |
| LR Scheduler | ReduceLROnPlateau |
| Batch Size | 512 – 1024 |
| Max Epochs | 100 |
| Early Stopping | Patience = 10 epochs |
| Data Split | 70% Train / 15% Validation / 15% Test (Stratified) |

---

## 👥 Authors

- **Ahmed Tarek Emam** — [GitHub](https://github.com/engatemam)
- **Mostafa Ayman** — [GitHub](https://github.com/77mostafaayman77)

---

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [RadioML2016.10a Dataset](https://www.deepsig.ai/datasets) by DeepSig Inc.
- T. J. O'Shea, J. Corgan, and T. C. Clancy — *"Convolutional Radio Modulation Recognition Networks"* (2016)
- [PyTorch Documentation](https://pytorch.org/docs/)
- [Streamlit Documentation](https://docs.streamlit.io/)
