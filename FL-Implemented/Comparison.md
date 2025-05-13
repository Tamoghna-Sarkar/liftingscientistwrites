## 🧪 Real Device Implementation Focus – Paper-by-Paper Summary

### 1. **Hermes: An Efficient Federated Learning Framework for Heterogeneous Mobile Clients**
- **Venue**: ACM MobiCom '21
- **Device Setup**: Android phones running models using **PyTorch Mobile** and **TorchScript**.
- **What They Measured**: End-to-end FL training time, model convergence, resource constraints.
- **Unique Angle**: Devices run **logical clients** emulating FL behavior; focus is on **heterogeneity-aware scheduling** using *Hermes Runtime*.
- **Deployment Details**: FL training executed directly on-device using USB for control and profiling.

---

### 2. **FS-Real: Federated Learning at Scale with Real-World Clients**
- **Venue**: Alibaba, arXiv '23
- **Device Setup**: Alibaba’s internal testbed; combination of **real edge devices** and simulated faults.
- **What They Measured**: Update success, convergence time, client failure, communication bottlenecks.
- **Unique Angle**: Focuses on **robustness testing and recovery**; realistic behavior under **network dropout, device fault, CPU overload**.
- **Deployment Details**: Real devices connected to a production-like FL server to mimic federated orchestration in Alibaba-scale environment.

---

### 3. **Smartphone FL for Depression Detection**
- **Venue**: arXiv '25
- **Device Setup**: Real Android smartphones with a **custom mobile app** for FL.
- **What They Measured**: Accuracy of mental health prediction, battery usage, dropout rates.
- **Unique Angle**: Real-world **non-synthetic users** using their phones; health-centric FL.
- **Deployment Details**: Users install an app that collects behavior/passive sensor data and runs **on-device PyTorch inference and training** using fine-tuned small models.

---

### 4. **FedOps: A Platform of Federated Learning Management for Enhanced Mobile Collaboration**
- **Venue**: Electronics 2024
- **Device Setup**: Android smartphones in a mobile FL testbed.
- **What They Measured**: Coordination success, task execution latency, training lifecycle completion.
- **Unique Angle**: Focuses on **managing and orchestrating FL** in real smartphones, not the model performance itself.
- **Deployment Details**: Mobile clients are controllable via a central UI; runtime profiling is tracked during training sessions to manage client availability and heterogeneity.

---

### 5. **FLINT: A Platform for Federated Learning Integration**
- **Venue**: MLSys '23
- **Device Setup**: Uses **AWS Device Farm** to deploy models to a range of Android/iOS devices.
- **What They Measured**: **Model runtime, crash rate, OS-level compatibility, CPU/memory usage**.
- **Unique Angle**: Focuses on **safe model selection** by pre-testing on real devices before FL.
- **Deployment Details**: Candidate FL models are pushed to phones using AWS pipeline; performance logs used to inform client eligibility and avoid runtime failures.

---

### 6. **Fedstellar: A Platform for Decentralized Federated Learning**
- **Venue**: arXiv '24
- **Device Setup**: Real devices (e.g., Raspberry Pi, Android) and Docker-based emulation.
- **What They Measured**: Message propagation latency, update delay, protocol throughput.
- **Unique Angle**: Supports **decentralized FL (DFL, SDFL, CFL)**; not cloud-centric.
- **Deployment Details**: Combines real hardware and virtual nodes to simulate various FL topologies (mesh, star, hierarchical) under real bandwidth constraints.



##  Common Characteristics in FL Real-Device Papers

| Feature | Hermes (MobiCom '21) | FS-Real (Alibaba '23) | Smartphone FL for Depression ('25) | FedOps (Electronics '24) | FLINT (MLSys '23) | Fedstellar (Arxiv '24) |
|--------|-----------------------|------------------------|-------------------------------------|----------------------------|---------------------|--------------------------|
| **Real Device Use** | Yes (Android phones) | Yes (Alibaba devices, numbers not mentioned) | Yes (Android phones, varied demographics) | Yes (phones in FL testbed) | Yes (AWS Device Farm) | Yes (Raspberry Pi + Android) |
| **Platform/OS** | Android via PyTorch Mobile / TorchScript | Android + Linux edge nodes | Android (custom health app) | Android app-based control | Android/iOS (via AWS) | Android, Raspberry Pi (edge) |
| **Deployment Setup** | Logical clients via TorchScript, USB | Clustered on-device clients + simulated faults | Real smartphone users with health tracking app | UI-based FL lifecycle control | App deployment via AWS Farm | Virtual + Physical deployments |
| **Heterogeneity Handling** | Yes (Device-aware scheduling) | Yes (hardware stats + energy profiling) | Yes (personalized training + dropout handling) | Yes (profiled stats, cloud-offload options) | Yes (diverse devices, system fallback) | Yes (hardware heterogeneity captured) |
| **Network Conditions** | Emulated throttling + real wireless | Real-world latency, packet drops modeled | Tested under WiFi/mobile data variabilities | Assumes constrained mobile networks | Simulated drop/rate-limiting | Includes WAN/LAN settings |
| **Evaluation Metrics** | Accuracy, system time, model fairness | Accuracy, latency, convergence, fault recovery | Mental health score prediction, resource stats | Lifecycle success rates, coordination overhead | Resource usage, model accuracy | Throughput, update delay, success rate |
| **Privacy / Data Locality** | Yes (no raw data sharing) | Yes (production data localized) | Yes (mental health data remains local) | Yes | Yes | Yes |
| **Training Framework** | PyTorch Mobile, custom Hermes runtime | Alibaba PAI-FL | Custom Android client + PyTorch | Custom orchestrator + FL backend | PyTorch variants + AWS pipeline | Lightweight orchestration layer |
| **Experiment Scale(ALl had logical clients to show scale)** | ~20 devices per round (logical) | ~small # of real + synthetic clients | ~5 real smartphones, tablets | 2 Android, 1 Iphone, 20 AWS Farm Device(Iphone, Android) | ~30 devices via AWS Farm | 5 Rpi4 + 3 Rock64  |
| **Emphasis** | **Heterogeneity-aware FL training** | **Cross-device fault recovery, realism** | **Health FL with real population** | **Manageability, fault resilience** | **FL porting, risk mitigation** | **Decentralized orchestration + hybrid testbeds** |


 ## Cross-Paper Takeaways
 
 Core Similarities:

    Emphasis on Device Heterogeneity: All papers focus on how FL must adapt to differences in CPU, memory, battery, or OS-level performance.

    Realistic FL Conditions: Include packet loss, intermittent connectivity, system-level crashes, etc.

    Privacy-Preserving Approach: All respect local data boundaries—no raw data leaves the device.

    Hybrid Evaluation: Combine synthetic simulation (for scale) and real-device deployment (for realism).

    Lightweight Clients: Use PyTorch Mobile, TensorFlow Lite, or customized lean runtimes to run on phones or Raspberry Pis.
 
 Key Implementation Strategies:

    Hermes/FLINT/Fedstellar use modular runtime (TorchScript or Dockerized services) to enable portability.

    FS-Real and FLINT use both virtual emulation and large-scale internal infrastructure (like Alibaba or AWS testbeds).

    FedOps focuses on making FL manageable via an app-level interface.

    Depression-FL prioritizes real data collection under mobile health scenarios, balancing inference with battery and privacy.