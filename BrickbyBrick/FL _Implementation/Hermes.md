# Hermes: An Efficient Federated Learning Framework for Heterogeneous Mobile Clients

**Authors**: Ang Li, Jingwei Sun, Pengcheng Li, Yu Pu, Hai Li, Yi  
**Conference**: [Add the conference here if known]  
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

---

## 📎 Reference

Li, Ang, et al. *Hermes: An Efficient Federated Learning Framework for Heterogeneous Mobile Clients*. [Add full citation and link here]

