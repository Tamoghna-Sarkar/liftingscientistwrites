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

## 2.1 Background on Federated Learning

Federated Learning (FL) enables collaborative training of machine learning models across distributed devices **without requiring data to leave local devices**. A central server coordinates the global objective by aggregating model updates from each client.

The global optimization objective is formulated as a **weighted sum of local losses**:

min_W f(W) = sum_{k=1}^{N} (n_k / n) * F_k(W_k)


Where:
- `N`: total number of devices
- `n_k`: number of data samples on device `k`
- `n = sum_{k=1}^{N} n_k`: total number of samples across all devices
- `F_k(W_k)`: local objective (empirical risk) for device `k`
- `W_k`: model parameters for device `k`
- `D_k`: local data distribution on device `k`

---

### ✴️ FedAvg (Federated Averaging)

One of the most popular FL methods is **FedAvg** (McMahan et al., 2017). Here's how it works:

1. A small random subset of clients (say `K << N`) is selected in each communication round.
2. Each selected client:
   - Trains the global model on its **local data** using **SGD**.
   - Uses the same number of local epochs, learning rate, and optimizer.
3. Devices send their **local model updates** `W_k` to the server.
4. The server performs a **weighted average** using mixing weights `p_k` to produce the new global model:

W = sum_{k=1}^{K} p_k * W_k


This method is simple and effective but relies on key assumptions:
- All devices use the **same full model** architecture.
- Data is **IID** or only mildly non-IID.

### 2.2 Data Heterogeneity Enforces Model Personalization

One of the main motivations for using Federated Learning (FL) is to collaboratively learn a global model that performs **better than local-only training**. However, this advantage **breaks down under non-IID data**.

Several studies [18, 29, 36] show that when devices have **highly heterogeneous data distributions**, a locally trained model can actually **outperform the global model** trained via FL.

---

#### 🔬 Experimental Evidence

To demonstrate this, the authors conducted an experiment using **CIFAR-10** with the **FedAvg** algorithm.

- **Communication rounds**: 400  
- **Devices per round**: 20  
- **Local epochs per device**: 5  
- **Each device holds**:
  - Only **2 classes** out of 10  
  - **20 samples per class** → total 40 samples per device

---

#### 📊 Results

- **FedAvg Global Model Accuracy**: 47.67%
- **Locally Trained Model Accuracy**: 65.44%

➡️ The global model trained by FedAvg performs **~18% worse** than training locally, due to severe non-IID effects.

---

#### ⚠️ Why is this setup highly non-IID?

- Each device has **only 2 out of 10 classes** (extreme label skew).
- Devices **do not share coverage** of the full dataset.
- Local updates are **biased** and potentially harmful when aggregated globally.

---

#### ✅ Conclusion

This demonstrates a key challenge:  
> **"It is difficult to train a one-size-fits-all global model in the presence of non-IID data."**

Hence, **model personalization** becomes essential for effective FL. Hermes is introduced to address this exact problem — by enabling each client to learn a **personalized subnetwork**, while still benefiting from global coordination.










---

## 📎 Reference

Li, Ang, et al. *Hermes: An Efficient Federated Learning Framework for Heterogeneous Mobile Clients*. [Add full citation and link here]

