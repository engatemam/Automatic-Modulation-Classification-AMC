# 📡 Automatic Modulation Classification (AMC)

> A deep learning–based system that automatically identifies the modulation type of raw radio signals (IQ samples) using Convolutional Neural Networks (CNN) and Recurrent Neural Networks (LSTM).

---

## 💡 Project Overview

**Automatic Modulation Classification (AMC)** is a project aimed at building an intelligent system capable of automatically identifying the modulation scheme (e.g., AM, FM, QPSK, 16QAM, etc.) used in any captured radio signal, without requiring any prior information about the signal itself.

### How It Works
1. **Signal Reception:** The system receives the signal as raw data (IQ Samples).
2. **Automated Recognition (Deep Learning):** The data is fed into deep learning models trained to extract patterns and distinctive features for each modulation type.
3. **Classification:** Finally, the system accurately determines the modulation type and presents the result to the user through a simple, interactive GUI.

### Why Is This Important?
- **Cognitive Radio:** Aids in building smart communication networks capable of sensing available frequencies in the air and utilizing them efficiently.
- **Spectrum Monitoring:** Makes it easier for regulatory authorities to monitor the frequency spectrum and know who is using which frequency and how.
- **Security & Military Applications:** Useful for analyzing and recognizing unknown or encrypted signals.
