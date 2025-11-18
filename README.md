📡 WiFi Sensing Using Synthetic CSI
(DSA + Machine Learning Project — Ubuntu • Python • Jupyter Notebook)




🧭 Overview


This project demonstrates a complete WiFi sensing pipeline using synthetic Channel State Information (CSI).
Because real CSI hardware was unavailable, we simulate CSI with a controlled multipath + Doppler model.

The system:

- Generates synthetic CSI streams

- Labels data (static vs motion)

- Extracts features using DS&A concepts

- Builds a train/test dataset

- Trains a Random Forest classifier

- Performs real-time streaming inference

- Detects anomalies (motion onset)

- Visualizes system behavior

All code is contained in one notebook: notebooks/synthetic_csi_demo.ipynb




🧱 Project Structure



wifi_sensing/
├── notebooks/
│   ├── synthetic_csi_demo.ipynb
│   └── data/
│       ├── csi_windows_small.npz      # dataset
│       └── rf_csi_model.joblib        # trained model
├── venv/
├── README.md
└── requirements.txt




📡 Synthetic CSI Model



We model CSI as:

- A complex-valued frequency-domain vector (subcarrier amplitudes + phases)

- Multiple multipath reflections

- Doppler shifts (for motion)

- Gaussian noise


Two classes:
| Label | Meaning            |
| ----- | ------------------ |
|   0   | Static environment |
|   1   | Motion present     |





🧠 DS&A Concepts Used



This project integrates Data Structures & Algorithms directly into the sensing pipeline:

✔ Circular Buffer (Deque)

Efficient sliding window storage for CSI samples (O(1) push / pop).

✔ Sliding Window Aggregation

Extract real-time statistics across the latest window.

✔ Moving Median Filter

Noise smoothing per subcarrier using median over window.

✔ Top-K Subcarrier Energy Extraction

Find most energetic subcarriers using argsort.

✔ Streaming Algorithm

Real-time CSI → buffer → features → ML → plot.




🧩 Feature Engineering

For each window:

-Amplitude Mean

-Amplitude Std

-Total Energy

-Subcarrier Std Mean

-Phase Difference Rate (Doppler proxy)

-Amplitude Delta (trend indicator)

These features cleanly separate static vs motion.




🤖 Machine Learning


A Random Forest Classifier is trained using the extracted features.

Performance:
| Mode                 | Accuracy |
| -------------------- | -------- |
|   Offline test       |   100%   |
|   Online streaming   |   92%    |

The model is saved as: notebooks/data/rf_csi_model.joblib




🔄 Streaming System

A StreamProcessor class performs:

-Window buffering

-Feature extraction

-Random Forest online prediction

-Z-score anomaly detection

-Moving median

-Top-K energy tracking


A mixed sequence (static → motion → static) is fed through the streaming system.

Results show:

- Motion correctly detected

- Anomalies spike precisely in motion segment

- Classifier matches ground truth with high accuracy



▶️ How to Run the Project

1. Activate virtual environment: source venv/bin/activate

2. Open VS Code → open synthetic_csi_demo.ipynb

3. Select kernel: wifi_sensing (venv)

4. Run all cells


📊 Results Summary

Synthetic CSI simulation working

-Clean dataset generation

-Random Forest achieves perfect offline accuracy

-Online streaming accuracy ~92%

-Anomaly detector identifies movement transitions

-DS&A techniques implemented successfully