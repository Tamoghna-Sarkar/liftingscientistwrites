# Hermes: An Efficient Federated Learning Framework for Heterogeneous Mobile Clients

**Authors**: Ang Li, Jingwei Sun, Pengcheng Li, Yu Pu, Hai Li, Yi  
**Conference**: [ACM MobiCom ’21, January 31-February 4, 2022, New Orleans, LA, USA ]  
**GitHub Summary by**: Tamoghna Sarkar

---

## 🌐 Motivation

Federated Learning (FL) enables distributed learning across multiple devices **without sharing raw data**, preserving privacy. However, FL faces two key challenges:

- **Non-IID Data**: Devices have data from **different distributions**, making it hard to train a single global model.
- **Resource Constraints**: Mobile devices have **limited bandwidth, compute power, and battery**, making large model updates costly.

---

## 🚧 Challenges

1. **Local Training Challenge**:  
   Train a **personalized subnetwork** on each client that:
   - Embeds local data patterns
   - Is **small** enough for fast inference and efficient communication

2. **Aggregation Challenge**:  
   Due to personalization, clients prune different parts of the model — resulting in **heterogeneous subnetworks**. Standard FedAvg fails here because:
   - It averages **all parameters**
   - This **destroys personalized updates** when subnetworks don't align

---

## 🚀 Hermes: Core Ideas

Hermes proposes a **personalization-preserving and communication-efficient FL framework**, with two key innovations:

### 1. Structured Subnetwork Training
- Clients perform **structured pruning** (e.g., filters, blocks) on a base model using **local data**.
- Train and communicate **only the pruned subnetwork**, not the entire model.
- Result: Models are **personalized, compact**, and require **less bandwidth**.

### 2. Overlap-Only Aggregation
- The server **only aggregates overlapping parameters** between subnetworks.
- **Non-overlapping (personalized) parts are untouched** — avoiding corruption of local knowledge.
- This allows **joint learning** of shared knowledge while retaining **client-specific structure**.

---

## 📉 Communication Optimization

Hermes significantly reduces communication by:
- Sending only **subnetwork parameters**
- Avoiding zero-padding for missing weights
- Exploiting **structured sparsity** for better compression and hardware execution

Communication cost is reduced by **1.92× to 3.48×** over state-of-the-art methods.

---

## ⚙️ System-Level Contributions

Hermes is implemented and evaluated on **real smartphones** across multiple applications and datasets:

- Compared against:  
  - **FedAvg**  
  - **Top-k sparsification**  
  - **Per-FedAvg** (fine-tuning)  
  - **LG-FedAvg** (local-global split)

- **Accuracy gains**: 0.53% – 32.17%  
- **Inference latency reduction**: up to 1.83×  
- **Memory footprint**: reduced by 70%  
- **Energy consumption**: reduced by 1.8×

This makes Hermes **deployable in realistic edge FL settings**.

---

## 📐 Theoretical Foundation

Hermes also includes a **theoretical convergence guarantee**, setting it apart from many heuristic personalization methods in FL.

---

## 🔑 Summary of Contributions

- 🎯 **Unified** personalization + communication + inference efficiency
- 🧠 **Overlap-aware aggregation** for heterogeneous subnetworks
- 📱 **Edge-deployable**, evaluated on real mobile devices
- 📐 Backed by **theoretical guarantees**

Hermes represents a significant step toward **practical, personalized FL** for resource-constrained, heterogeneous clients.

## 2. Background and Motivation

This section motivates the need for **personalized federated learning (FL)** and highlights the **communication bottlenecks** in current approaches. These limitations pave the way for the design of Hermes.

---

### 2.1 Background on Federated Learning

Federated Learning (FL) enables collaborative model training across multiple devices **without sharing local data**. A **central server** orchestrates the process, aiming to learn a global model \( \mathbf{W} \) by minimizing the **weighted sum of local losses**:

\[
\min_{\mathbf{W}} f(\mathbf{W}) = \sum_{k=1}^{N} \frac{n_k}{n} F_k(\mathbf{W}_k) = \mathbb{E}_k [F_k(\mathbf{W}_k)]
\]

Where:

- \( N \): total number of devices
- \( n_k \): number of data samples on device \( k \)
- \( n = \sum_{k=1}^N n_k \): total number of samples across all devices
- \( F_k(\mathbf{W}_k) \): local objective (empirical risk) for device \( k \)
- \( \mathbf{W}_k \): model parameters for device \( k \)
- \( D_k \): local data distribution on device \( k \)

---

#### ⚙️ FedAvg (Federated Averaging)

One of the most popular FL methods is **FedAvg** [McMahan et al. 2017]. Here's how it works:

1. A small random subset of clients (say \( K \ll N \)) is selected in each round.
2. Each selected device:
   - Trains the global model on its **local data** using **SGD**.
   - Uses the same number of local epochs, learning rate, and optimizer.
3. Devices send their **local updates \( \mathbf{W}_k \)** to the server.
4. The server performs a **weighted average** using mixing weights \( p_k \) to update the global model:

\[
\mathbf{W} = \sum_{k=1}^{K} p_k \mathbf{W}_k
\]

This method is simple and effective but assumes that:
- All devices use the **same full model**
- **Data is IID or not severely non-IID**
- Communication and inference cost of full model is acceptable

These assumptions **break down in heterogeneous and resource-limited settings**, motivating Hermes.















---

## 📎 Reference

Li, Ang, et al. *Hermes: An Efficient Federated Learning Framework for Heterogeneous Mobile Clients*. [Add full citation and link here]

