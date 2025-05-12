# On-device Federated Learning in Smartphones for Detecting Depression from Reddit Posts  
**Authors:** Mustofa Ahmed, Abdul Muntakim, Nawrin Tabassum, Mohammad Asifur Rahim, Faisal Muhammad Shah  
**arXiv:** [arXiv:2410.13709v2](https://arxiv.org/abs/2410.13709)  
**GitHub Summary by:** Tamoghna Sarkar

## 🌐 Motivation

Deep learning has proven effective in identifying mental health conditions such as depression from unstructured social media text. However, traditional centralized training raises serious privacy concerns, especially when dealing with sensitive user data from platforms like Reddit. Transmitting raw data to central servers risks data misuse and violates user trust.

This study is motivated by the need to perform **decentralized, privacy-preserving learning directly on smartphones** using Federated Learning (FL). It investigates whether real-world mobile devices—such as Android phones and tablets—can **efficiently train deep neural networks on-device** for depression detection, without exposing private user data.

Key motivations include:
- Ensuring **user privacy and GDPR compliance** by avoiding data centralization.
- Exploring **on-device FL performance** under real conditions (e.g., varied hardware, battery life, communication delays).
- Understanding how well lightweight models (RNN, GRU, LSTM) perform for text classification in an FL setup.
- Evaluating the trade-offs in **accuracy, communication cost, training time, and battery usage** in real smartphones.

The work advances the practical deployment of FL systems for mental health prediction, offering insight into system-level behavior, usability, and performance challenges on heterogeneous mobile devices.


### System Architecture

| Component       | Platform/Tool                   | Role                                                                 |
|----------------|----------------------------------|----------------------------------------------------------------------|
| Clients         | Android Phones/Tablets (Real)   | On-device model training using GRU/RNN/LSTM and local inference     |
| Server          | Python Tkinter App (Desktop)    | Federated Averaging, model evaluation, sync control                 |
| Storage         | Firebase Storage                | Model weights upload/download                                       |
| Coordination    | Firebase Realtime Database      | Status and sync messaging                                           |

---

### Client-Side Deployment (Android Devices)

- Built using Android Studio (2022.1.1), minimum Android 5.1 (Lollipop)
- Python integration via **Chaquopy** plugin (v14.0.2) to run native Python
- Python libraries used:  
  `TensorFlow 2.1.0`, `NumPy 1.19.5`, `Pandas 1.3.2`, `Pyrebase 4.7.1`
- On-device functions:
  - Tokenization using a shared pretrained tokenizer (not client-specific)
  - Padding, embedding using GloVe (100d), local training for 1 epoch
  - Upload trained weights to Firebase
  - Retrieve updated global model from server
  - Run inference on user-entered text
- Devices used: 5 Android devices (phones + tablet), specs detailed in Table II  
  (Range: Android 9–14, 3GB–8GB RAM, Snapdragon/MediaTek/Exynos CPUs)

---

### Server-Side Deployment

- Desktop application built using **Tkinter (v8.6)** in PyCharm
- Server tasks:
  - Initializes global model
  - Aggregates local models via **FedAvg**
  - Monitors sync signals from Firebase Realtime DB
  - Uploads updated global model for next round
- Server performs all model evaluations centrally (clients only train)

---

### Communication and Training Pipeline

Described in **Algorithm 1** and **Figure 3**:

1. Global model sent from server to clients via Firebase Storage
2. Clients load tokenizer, GloVe, and training data
3. Local model trained (1 epoch), weights uploaded to Firebase
4. Server aggregates weights once all clients complete
5. New global model uploaded, triggering next round

---

### Real Device Evaluations

#### Time and Resource Profiling (per round per device):
- **Training time**: LSTM > GRU > RNN (dependent on CPU/RAM)
- **Upload/download time**: No strict trend, varies by network type
- **Inference time**: GRU ~30-60ms on faster devices
- **Overhead time** (loading tokenizer, GloVe, etc.): ~0.5–4s
- **Battery usage**: Correlates with model complexity and battery health

#### Communication Costs (Table III):
| Model | Received (MB) | Transmitted (MB) |
|-------|----------------|------------------|
| RNN   | 5.50           | 4.10             |
| GRU   | 11.70          | 9.35             |
| LSTM  | 14.90          | 11.85            |

#### Training Settings Evaluated:
- IID with all clients
- IID with dropped clients
- Non-IID with all clients
- Non-IID with dropped clients

#### Key Observations:
- GRU performed best overall in FL setting.
- Performance dropped significantly in non-IID with client dropout.
- Client polling Firebase frequently increases "received bytes".

---

### Limitations
- **Synchronous FL** leads to straggler delays (slowest client bottlenecks round)
- Firebase occasionally stalls while downloading weights
- Large app size due to bundled Python libraries

---

## 📎 Reference

Ahmed, Mustofa, et al. *On-device Federated Learning in Smartphones for Detecting Depression from Reddit Posts*. arXiv:2410.13709v2 [cs.LG], 2025.  
[[Link to Paper](https://arxiv.org/abs/2410.13709)]

