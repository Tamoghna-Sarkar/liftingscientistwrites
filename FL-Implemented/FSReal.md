# FS-Real: Towards Real-World Cross-Device Federated Learning

**Authors**: Daoyuan Chen, Dawei Gao, Yuexiang Xie, Xuchen Pan, Zitao Li, Yaliang Li, Bolin Ding, Jingren Zhou  
**Conference**: [arXiv preprint arXiv:2303.13363, March 2023]  
**GitHub Summary by**: Tamoghna Sarkar

## 🌐 Motivation

Federated Learning (FL) research often assumes homogeneous devices and idealized simulations, but **real-world deployments involve highly heterogeneous mobile devices** (in compute, bandwidth, and availability) and operate at large scales. Most FL frameworks fail to capture this complexity, limiting their practical value.

FS-Real addresses this gap by providing a **scalable, deployable FL system** that supports:

- **Real Android devices** (e.g., RedMi K40 phones)
- **Simulated devices** with controllable heterogeneity in compute, memory, and bandwidth
- **Advanced FL techniques** such as personalization, communication compression, and asynchronous aggregation

It aims to enable more realistic evaluations of FL algorithms and bridge the gap between theory and deployment.


## What This Paper Does

- Proposes **FS-Real**, a practical system for cross-device Federated Learning (FL) that supports:
  - **Real-world heterogeneous devices** (Android phones with varying compute/network capabilities)
  - **Scalable FL experiments** (up to 100,000 simulated clients)
  - **Algorithmic extensions** including:
    - Personalization (e.g., fine-tuning, FedBABU)
    - Communication compression (e.g., Gzip, FP16, INT8)
    - Asynchronous aggregation (e.g., FedBuff)

- Bridges the gap between FL simulations and real-world deployments by enabling evaluations under **realistic device runtimes and network conditions**.

## Key Problems Solved

- Addresses the **mismatch** between most academic FL simulations (which use idealized, homogeneous devices) and real-world deployments (which involve diverse, unstable, and large-scale environments).
- Provides an **efficient runtime engine** for real Android phones and a **scalable simulator** to test FL performance under:
  - Device heterogeneity
  - Runtime variability
  - Different scale levels (hundreds to 100,000 clients)
- Outperforms prior systems like **FedScale** in both deployment feasibility and efficiency.

## FS-Real Usage: Real vs Simulated Devices

### Real Devices (e.g., RedMi K40)

- Used to **evaluate real-world deployability** and **runtime performance**.
- Measured round time (seconds) under:
  - **CPU load levels**: idle, light (video/music), moderate (gaming)
  - **Bandwidth conditions**: 100 Mbps, 25 Mbps, 5 Mbps
  - **Batch sizes**: 16, 32, 64
- Demonstrated:
  - FS-Real achieves **2×–4× faster** FL rounds compared to FedScale
  - FS-Real’s on-device runtime is **163 MB** (vs. ~9 GB for FedScale with Termux + Python)
  - Enables efficient, lightweight FL training on actual phones

### Simulated Devices (Android Emulator)

- Used to conduct **large-scale, controlled experiments**.
- Emulated configurations:
  - **CPU cores**: 1–4
  - **Memory**: 256–1024 MB
  - **Bandwidth**: 3G, 4G, WiFi (downlink/uplink speeds specified)
  - **Latency**: 35–400 ms
- Simulated multiple **device distributions**:
  - Homo-device (identical clients)
  - Uniform
  - Near-normal (most clients are medium-tier)
  - Strong-heavy (mostly strong clients)
  - Double-tails (many very weak and strong devices)
- Tested algorithmic performance on:
  - **Fairness** (per-client accuracy)
  - **Convergence time**
  - **Network traffic**
  - **Client utilization**

## FL Workloads and Models Used

### Datasets

- **FEMNIST**:
  - Handwritten digit/character recognition
  - 3,596 clients (partitioned by writer ID)
- **CelebA**:
  - Facial attribute classification
  - 9,343 clients (partitioned by celebrity identity)
- **Twitter Sentiment**:
  - Binary sentiment classification (positive/negative)
  - 13,203 clients (partitioned by Twitter user)

### Models

- **ConvNet** (2 conv + 2 linear layers) for FEMNIST and CelebA:
  - FEMNIST: [32, 64, 1024, 62]
  - CelebA: [32, 64, 256, 2] to avoid memory issues
- **Logistic Regression** for Twitter:
  - Uses 50-dim Glove embeddings per token

### FL Algorithms Tested

- **FedAvg** (baseline synchronous aggregation)
- **Personalized FL**:
  - Fine-tuning after global aggregation
  - FedBABU: freeze classifier during FL, fine-tune locally before evaluation
- **Compression**:
  - Gzip (lossless)
  - FP16 and INT8 (quantization)
- **Asynchronous Aggregation**:
  - FedBuff: tolerate message staleness and improve responsiveness
  - Adaptive timeout and over-selection of clients

## Summary of Contributions

- Delivers a **unified framework** that supports both **real deployment** and **scalable simulation** of FL under realistic runtime conditions.
- Quantifies the impact of:
  - Device heterogeneity
  - Communication variance
  - Large-scale deployments on FL training behavior
- Demonstrates that naive assumptions of homogeneity overestimate FL algorithm robustness.
- Provides an **open-source implementation** with high fidelity and extensibility for future FL research.

# Brief:
- The FS-Real paper presents a practical framework for studying real-world federated learning (FL) at scale across heterogeneous devices. Unlike most prior work that assumes homogeneous clients or simulated environments, FS-Real enables FL training on both real Android phones and high-fidelity emulated devices, capturing variations in compute, memory, and network conditions. It supports advanced FL techniques such as personalization, model compression, and asynchronous aggregation, and demonstrates performance across realistic workloads like FEMNIST, CelebA, and Twitter Sentiment datasets. By quantifying how device heterogeneity and scale impact convergence, fairness, and system efficiency, FS-Real offers a valuable bridge between academic FL research and deployable, production-ready systems.


## 📎 Reference

Chen, Daoyuan, et al. *FS-Real: Towards Real-World Cross-Device Federated Learning.* [[Link to Paper](https://arxiv.org/pdf/2303.13363)]




