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
| **Experiment Scale(ALl had logical clients to show scale)** | ~20 devices per round (logical) | ~small # of real + synthetic clients | ~5 real smartphones, tablets | 2 Android, 1 Iphone, 2 AWS Farm Device(Iphone, Android) | ~30 devices via AWS Farm | 5 Rpi4 + 3 Rock64  |
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