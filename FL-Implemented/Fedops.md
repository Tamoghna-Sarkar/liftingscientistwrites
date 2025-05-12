#  Summary: FedOps Mobile – A Platform of Federated Learning Management for Enhanced Mobile Collaboration

**Citation**:  
Yusubov, F.; Lee, K. *A Platform of Federated Learning Management for Enhanced Mobile Collaboration*. Electronics 2024, 13, 4104. https://doi.org/10.3390/electronics13204104  
**GitHub Summary by**: Tamoghna Sarkar

---

## 🌐 Motivation

Federated Learning (FL) offers privacy-preserving distributed training across devices but faces major challenges in **mobile and heterogeneous environments**:
- **Device heterogeneity**: Mobile devices differ vastly in hardware, OS (iOS/Android), compute power, and energy capacity.
- **System scalability**: Large, diverse device pools require dynamic orchestration.
- **Energy efficiency**: Sustaining long-term participation demands battery-aware strategies.
- **Client dropout and unstable connectivity**: Affect consistency and training quality.
- **Lack of cross-platform mobile frameworks**: Most FL platforms only support Android or a single OS.

---

##  Solution (FedOps Mobile)

**FedOps Mobile** is introduced as a **cross-platform FL framework** specifically tailored for real-world mobile devices, built on:

- **On-device training** using:
  - **TensorFlow Lite** (Android)
  - **CoreML** (iOS)
- **Flutter** for unified front-end development.
- **Firebase** for:
  - Real-time client selection (Cloud Functions + Realtime DB)
  - Background task management (Cloud Messaging)

###  Deployment Details

- **Architecture**:
  - Mobile clients run training via native ML frameworks.
  - The FL server is hosted in a **Microk8s** environment (cloud/Kubernetes-based).
  - **Server-side FedOps** manages client orchestration, monitoring, and aggregation.

- **Client Selection**:
  - Devices are ranked by:
    - Battery level
    - Last-round accuracy
    - Network readiness
  - Top `m` devices are selected via Firebase Cloud Functions.

- **Client Training Loop**:
  - Notifications sent → device status collected → best clients selected → FL round initiated.
  - Training metrics + energy stats recorded on Firebase for real-time feedback.

---

##  Devices Used (Practical Setup)

Experiments used **25 devices** across three categories:

- **Lab Devices**:
  - Samsung Galaxy Tab S7 (Snapdragon 865)
  - Samsung Galaxy S10 (Exynos 9820)
  - iPhone 15 Pro (A17 Bionic)
  - Two Android emulators (Snapdragon 730, Android 13)

- **AWS Device Farm (20 devices)**:
  - High-end: Galaxy S21 Ultra, iPhone 12 Pro
  - Mid-range: Pixel 4a, Galaxy A52
  - Low-end: Moto E6

Each device participated in FL tasks with **real-time energy tracking, model evaluation, and resource reporting**.

---

##  Results (Three Experiments)

###  Experiment 1 – **CIFAR-10**, IID & Non-IID
- **Global accuracy**:  
  - IID: 80%  
  - Non-IID: 78%
- **Energy consumption**: 22% per training round
- **Training time**: 2h per round
- **Observation**: Rapid convergence (75% in 15 rounds), efficient training across device pool

###  Experiment 2 – **FEMNIST**, Personalized Modeling
- **Global accuracy**:
  - IID: 77%
  - Non-IID: 74%
- **Local model accuracy**:
  - IID: 85%
  - Non-IID: 83%
- **Energy use**: 18%/round
- **Training time**: 1.5h/round
- **Observation**: Strong personalized modeling and efficient resource allocation

###  Experiment 3 – **SHL dataset**, Multi-device Per User
- **Setup**: 10 users, 25 devices (1–3 per user)
- **Accuracy**: Global – 67% (Non-IID, realistic)
- **Energy**: ~20% battery/round
- **Training time**: 2.5h/round
- **Observation**: Handles realistic, heterogeneous, personalized mobile FL well

---

## 🆚 FLAME Comparison

| Metric                | FedOps Mobile | FLAME                     |
|----------------------|---------------|----------------------------|
| Global Accuracy       | 80%           | 77%                        |
| Personalization       | Moderate      | Strong (biased selection) |
| Energy Optimization   | Built-in      | Less focused               |
| Training Strategy     | Device-aware  | User-device personalization via ADMM |
| Ideal Use Case        | Uniform device pools | Per-user multi-device scenarios |

---

## 📎 Reference

Yusubov, F.; Lee, K. *A Platform of Federated Learning Management for Enhanced Mobile Collaboration*. [Electronics 2024, 13, 4104](https://doi.org/10.3390/electronics13204104)

---
