# 📡 Automatic Modulation Classification (AMC)

> A deep learning–based system that will automatically identify the modulation type of raw radio signals (IQ samples) using CNN and CNN-LSTM architectures, with an interactive Streamlit GUI.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-ee4c2c?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-GUI-ff4b4b?logo=streamlit&logoColor=white)](https://streamlit.io/)
[![Status](https://img.shields.io/badge/Status-In%20Development-yellow)]()

---

## 🚧 Project Status: In Development

This project is currently under active development. Below is the planned scope and roadmap.

---

## 🎯 What Are We Building?

**Automatic Modulation Classification (AMC)** is a task in signal processing — the system takes a raw radio signal (IQ samples) and determines which modulation scheme was used, without any prior knowledge about the signal (blind classification).

### The Pipeline We're Building

```
User uploads signal file → Format conversion → Preprocessing → Model inference → Results + Visualizations
```

1. User selects file format and uploads a signal file through the GUI
2. The system converts any of the 4 supported formats into a unified IQ representation
3. The signal is preprocessed (normalized, reshaped) and fed to the trained model
4. The model outputs probabilities for each of the 11 modulation types
5. The GUI displays the predicted class, confidence score, and signal visualizations

### Why AMC Matters

- **Cognitive Radio** — Smart wireless networks that detect and use available frequencies autonomously
- **Spectrum Monitoring** — Regulatory agencies monitoring frequency usage
- **Military & Intelligence** — Analyzing unknown intercepted signals
- **Bridging Theory & Practice** — Connects Communications Theory with Deep Learning for real-world RF problems

---

## 📋 Planned Features

- [ ] 🎛️ **11 Modulation Types** — 8PSK, AM-DSB, AM-SSB, BPSK, CPFSK, GFSK, PAM4, QAM16, QAM64, QPSK, WBFM
- [ ] 📂 **4 Input Formats** — Raw Binary (`.iq`), MATLAB (`.mat`), CSV (`.csv`), NumPy (`.npy`)
- [ ] 🧠 **2 Model Architectures** — Baseline CNN vs. CNN+LSTM Hybrid
- [ ] 🖥️ **Interactive GUI** — Streamlit web app with model selection, file upload, and visualization
- [ ] 📊 **Visualizations** — Constellation diagrams, time-domain plots, spectrograms, confidence charts
- [ ] 📈 **Evaluation** — Confusion matrices, Accuracy vs. SNR curves, per-class metrics

---

## 🧠 Planned Model Architectures

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

Combines convolutional feature extraction with temporal sequence modeling:

| Layer | Details |
|-------|---------|
| Conv1D | 64 filters, kernel size 8, BatchNorm, MaxPool |
| Conv1D | 128 filters, kernel size 5, BatchNorm |
| LSTM | 128 hidden units |
| Dense | 128 units, ReLU, Dropout 50% |
| Output | 11 units, Softmax |

---

## 📦 Dataset

We will be using **RadioML2016.10a** — a widely-used benchmark dataset for AMC research.

| Property | Value |
|----------|-------|
| Modulation Types | 11 |
| SNR Range | -20 dB to +18 dB (20 levels, step 2 dB) |
| Samples per (mod, SNR) | 1,000 |
| Total Samples | 220,000 |
| Sample Shape | 2 × 128 (I row + Q row) |
| File Format | Python Pickle (.pkl) |

---

## 🏋️ Planned Training Configuration

| Parameter | Value |
|-----------|-------|
| Loss Function | Cross-Entropy Loss |
| Optimizer | Adam (lr = 0.001) |
| LR Scheduler | ReduceLROnPlateau |
| Batch Size | 512 – 1024 |
| Max Epochs | 100 |
| Early Stopping | Patience = 10 epochs |
| Data Split | 70% Train / 15% Val / 15% Test (Stratified) |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3.10+ | Core language |
| PyTorch 2.x | Deep learning framework |
| NumPy / Pandas / SciPy | Data handling |
| Matplotlib / Plotly | Visualization |
| Streamlit | GUI framework |
| Jupyter | Data exploration |
| Git / GitHub | Version control |

---

## 📁 Project Structure

```
Automatic-Modulation-Classification-AMC/
│
├── data/
│   ├── raw/                    # RadioML2016.10a dataset
│   ├── test_samples/           # Test files in all 4 formats
│   └── scripts/                # Data preparation scripts
│
├── utils/
│   ├── format_readers.py       # Read & convert all 4 input formats
│   ├── format_exporters.py     # Export samples to any format
│   └── preprocessing.py        # Normalization, reshaping, encoding
│
├── models/
│   ├── baseline_cnn.py         # Model 1: Baseline CNN
│   ├── cnn_lstm.py             # Model 2: CNN + LSTM Hybrid
│   └── checkpoints/            # Saved model weights
│
├── training/
│   ├── train.py                # Training loop
│   ├── evaluate.py             # Evaluation & metrics
│   └── dataset.py              # PyTorch Dataset class
│
├── gui/
│   └── app.py                  # Streamlit GUI
│
├── notebooks/                  # Exploration notebooks
├── docs/                       # Report & Presentation
├── tests/                      # Unit tests
│
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

## 🗺️ Roadmap

### Week 1 — Foundations & Study
- [x] Environment setup & repo structure
- [ ] Study modulation types theory
- [x] Download RadioML2016.10a dataset
- [ ] Dataset exploration & visualization
- [ ] Preprocessing pipeline

### Week 2 — Model Development
- [ ] Implement Baseline CNN (Model 1)
- [ ] Train & evaluate Model 1
- [ ] Implement CNN+LSTM Hybrid (Model 2)
- [ ] Train & evaluate Model 2
- [ ] Compare both models

### Week 3 — GUI & Multi-format Support
- [ ] Implement 4-format readers & exporters
- [ ] Generate test sample files
- [ ] Build Streamlit GUI
- [ ] Integrate models with GUI
- [ ] Testing & bug fixes

### Week 4 — Documentation & Polish
- [ ] Write project report
- [ ] Prepare presentation
- [ ] Code cleanup & refactoring
- [ ] Final testing
- [ ] Publish & share

---

## 👥 Authors

- **Ahmed Tarek Emam** — [GitHub](https://github.com/engatemam)
- **Mostafa Ayman** — [GitHub](https://github.com/77mostafaayman77)

---

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 📚 References

- T. J. O'Shea, J. Corgan, T. C. Clancy — *"Convolutional Radio Modulation Recognition Networks"* (2016)
- [RadioML2016.10a Dataset](https://www.deepsig.ai/datasets) — DeepSig Inc.
- [PyTorch Documentation](https://pytorch.org/docs/)
- [Streamlit Documentation](https://docs.streamlit.io/)
