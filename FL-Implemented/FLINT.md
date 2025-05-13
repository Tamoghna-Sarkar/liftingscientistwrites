# FLINT: A Platform for Federated Learning Integration

**Authors:** Ewen Wang*, Ajay Kannan, Yuefeng Liang, Boyi Chen*, Mosharaf Chowdhury  
**Conference:** MLSys 2023  
**Institutions:** LinkedIn Corporation, University of Michigan  
**Summary by:** Tamoghna Sarkar

---

## Motivation

While Federated Learning (FL) has gained traction in academia and industry, transitioning centralized ML systems to cross-device FL at scale remains risky and complex. Large-scale deployment introduces:

- Device and OS heterogeneity  
- Unpredictable availability  
- Uncertain performance trade-offs  
- Incomplete visibility into resource consumption  
- High developer overhead and lack of mature tooling  

FLINT is proposed to bridge this gap by providing a production-grade, device-cloud collaborative FL platform integrated with LinkedIn’s centralized ML stack. The core motivation is to enable practical, responsible, and cost-effective deployment of FL, grounded in real device capabilities, system constraints, and measurable outcomes.

---

## Practical Evaluations

The FLINT platform was applied to three business-critical domains at LinkedIn:

### 1. Advertising

- Privacy-sensitive use case where moving training to the device helps comply with data regulations.
- Trained models with conservative participation criteria (WiFi, foreground app, battery ≥ 80%).
- Proxy datasets simulated real clients; performance reached 98.15% of centralized AUPR with FedBuff async FL.
- Training time: 4.2 days.
- Identified system challenges (e.g., embedding vocab size, memory usage).
- Used AWS Device Farm to benchmark models on real devices before rollout.

### 2. Messaging

- End-to-end encrypted data prohibits centralized collection.
- FL allows using raw message content locally for models like abuse detection and smart inbox.
- FL trained on synthetic messages matched centralized accuracy (only -0.18%).
- Handled embedding compression and evaluated adversarial robustness (e.g., evasion attacks, poisoning).

### 3. Search

- Low-latency use case with sub-100ms response needs (e.g., query auto-completion).
- FL enables more personalized suggestions and local caching without round-trips to the server.
- Achieved ~98.36% of centralized performance with lightweight LSTM model.
- Demonstrated value in model freshness and latency reduction.

---

## Key Contributions

### FLINT Platform Design

- A device-cloud collaborative platform extending centralized ML pipelines with FL-specific capabilities (training, monitoring, benchmarking).
- Seamlessly integrates with existing model store, schedulers, and visualization tools.

### Real-World Device Benchmarking

- On-device model profiling across 27 mobile devices using AWS Device Farm.
- Measured compute time, memory, CPU usage, and selected mobile-friendly models accordingly.

### Proxy Dataset Generation

- Converts centralized data into realistic FL-like partitions using member IDs or synthetic distributions.
- Captures label skew, data quantity skew, and client diversity.

### Experimental Framework

- Extends FedScale to support virtual time, hardware heterogeneity, and device availability in training simulations.
- Supports both FedAvg and FedBuff, synchronous and asynchronous modes.
- Reports model and system metrics (not just loss/accuracy).

### Decision Workflow

- Provides a structured process for evaluating model performance, client eligibility, system load, privacy risks, and infrastructure readiness.
- Helps de-risk FL deployment by forecasting training time, resource usage, and participation rates.

### Privacy and Security Modeling

- Supports integration of Secure Aggregation (SecAgg) with Trusted Execution Environments.
- Evaluates differential privacy, robust training, and attack scenarios (e.g., poisoning, model evasion).

---

## Results and Insights

- **Device Eligibility**: Strict criteria (WiFi, battery, OS) reduce eligible clients to ~22%.
- **Performance Trade-off**: Small but acceptable drop (~1–2%) in metrics vs centralized, in exchange for improved compliance and user privacy.
- **Resource Forecasting**: FLINT projects client compute load, bandwidth, and training timelines accurately, supporting scalable deployment planning.
- **Asynchronous Training**: FedBuff showed up to 6× faster convergence in high heterogeneity settings by tolerating stale updates.

---

## Takeaways

- Federated Learning is not plug-and-play: FLINT shows that deploying FL in production at scale requires deep integration, monitoring, and simulation tools.
- On-device benchmarking is critical: The feasibility of any FL job depends on how the model performs on real devices, not just in simulation.
- Collaboration between cloud and edge is key: FLINT introduces hybrid data pipelines (device + cloud features), enabling smarter trade-offs.
- FL can meet production-grade standards when systematically evaluated using frameworks like FLINT.

## 📎 Reference

Wang, Ewen, et al. *FLINT: A Platform for Federated Learning Integration*. Proceedings of the 6th MLSys Conference, Miami, FL, USA, 2023. [[Link to Paper](https://arxiv.org/abs/2302.12862)]
