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

#### 💡 Why Use VGG16 and Inception-v4 on CIFAR-10?

Although CIFAR-10 is a small image dataset (32×32 images, 10 classes), the Hermes paper uses **large models** like **VGG16** and **Inception-v4**. This may seem unusual at first, but there are good reasons:

---

**1. Stress-Testing Communication Overhead**

- VGG16 and Inception-v4 are **deep CNNs with tens of millions of parameters**.
- Using these models helps **expose the communication bottlenecks** of standard FL approaches like FedAvg.
- For instance, FedAvg incurs **10.59 TB** (VGG16) and **32.22 TB** (Inception-v4) over 500 rounds — a massive burden for edge devices.

---

**2. Real-World Model Complexity**

- Many mobile applications (e.g., camera-based AI, AR, security monitoring) require **large and expressive models**.
- Training such models in an FL setup mirrors the **real deployment constraints** faced in edge AI scenarios.
- Hermes is designed to make these deployments practical.

---

**3. Separation of Dataset vs. Model Role**

- **CIFAR-10** is used to simulate **non-IID data scenarios**: each device has 2 classes only.
- **VGG16/Inception** are used to simulate **heavy model communication and inference load**.
- The focus is not on beating benchmarks, but on measuring:
  - Communication cost
  - Inference latency
  - Personalization effectiveness

---

➡️ In summary, using large models on small datasets helps evaluate **system-level performance** of FL methods, which is central to Hermes' goals.


## 3.1 Overview of Hermes

Hermes is a personalized Federated Learning (FL) framework designed to simultaneously address:

- **Communication overhead**
- **Inference inefficiency**
- **Data heterogeneity**

Unlike traditional FL frameworks that train and share the full global model, Hermes takes a different approach:

---

### 🔁 Hermes Workflow (5 Steps)

1. **Local Subnetwork Learning (Structured Pruning)**  
   Each selected device starts from the same base model but uses **structured pruning** based on its **local data** to create a smaller, personalized subnetwork.

2. **Communication to Server**  
   Instead of sending the full model, the device **only communicates the pruned subnetwork parameters** to the server, reducing communication load.

3. **Aggregation on Intersected Parameters Only**  
   The server performs aggregation **only on parameters shared across devices' subnetworks**. Non-overlapping (personalized) parameters are left untouched to preserve local specialization.

4. **Return Updated Shared Parameters**  
   The server sends the updated overlapping parameters back to each device. Each device merges this into its own subnetwork.

5. **Repeat for T Rounds**  
   This process is repeated for a predefined number of communication rounds. Eventually, each device has a **personalized, structured-sparse model** suited for its data.

---

### 🧩 Structured Pruning Explained

**Structured pruning** is a compression technique where **entire components of a neural network** are removed (instead of individual weights). This includes:

- Filters or channels in CNNs
- Neurons in fully connected layers
- Blocks or layers in deeper architectures

This produces models that are:

- Smaller in size
- Faster to execute
- Easier to deploy on hardware (compared to unstructured sparsity)

---

### 🧠 Why Structured Pruning in Hermes?

- Each device has **non-IID data** (e.g., different users, tasks, or sensors).
- So each device **retains different parts** of the model that are most useful for its own local data.
- This leads to **heterogeneous subnetworks** across devices.

#### Mask Representation:
Each device learns a binary mask `M_k` such that:

- `M_k ∈ {0,1}^{|W_k|}` — same size as the base model
- `W_k ⊙ M_k` gives the subnetwork (only "kept" parameters are active)

Where:
- `W_k` is the full base model on device `k`
- `⊙` denotes element-wise multiplication

---

### ✅ Benefits of Structured Pruning in Hermes

| Benefit                     | Explanation                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| **Personalization**         | Devices keep only what matters for their local data                        |
| **Efficiency**              | Smaller models → faster inference and less energy use                      |
| **Communication Reduction** | Only active subnetwork parameters are sent to the server                   |
| **Preserves Heterogeneity** | Different devices naturally train on different parts of the base model     |

---

Hermes capitalizes on structured pruning to make FL more practical and personalized, especially under heterogeneous, resource-constrained environments.

## 3.2 Learn Subnetwork for Joint Efficiency and Personalization

Hermes departs from traditional FL frameworks by having each device learn a **sparse subnetwork** — a smaller, personalized model — using its local data. The subnetwork is then communicated to the central server instead of the full model.

The key idea:  
> Learn a subnetwork that balances **communication efficiency**, **inference efficiency**, and **personalization**.

This is achieved using **structured pruning** — a hardware-friendly technique that removes **entire filters, channels, or neurons** from the model.

---

### 🔍 Why Structured Pruning?

There are two types of pruning:
- **Unstructured Pruning**:
  - Removes individual weights randomly.
  - Offers high compression but results in irregular models that are inefficient on real hardware.
- **Structured Pruning**:
  - Removes full filters, channels, rows, or columns.
  - Produces regular, clean subnetworks that run efficiently on mobile/edge devices.

Hermes uses **structured pruning** to:
- Compress models in a predictable way
- Enable devices to adapt model size based on local data
- Reduce bandwidth and inference latency

---

### 📐 Structured Sparsity Regularization

To prune during training, Hermes adds a **structured regularization term** to the loss function:

F(W) = F_D(W) + λ * R(W)


Where:
- `F_D(W)` is the standard training loss on local data
- `R(W)` is the structured sparsity regularizer
- `λ` is a hyperparameter controlling the strength of pruning

---

### 🔧 Breakdown of the Regularizer

Hermes splits the regularization into two parts:

R(W) = R_conv(W) + R_fc(W)


#### 🔹 R_conv(W): for Convolutional Layers

Encourages filter-wise and channel-wise sparsity:


R_conv(W) = ∑ over conv layers l [
∑ over filters f_l: ||W_f_l,:,:,:||g +
∑ over input channels ch_l: ||W:ch_l,:,:||_g
]


- `|| · ||_g` is the **group Lasso norm**, which promotes sparsity at the filter/channel level.
- Filters and channels with small norm are pruned entirely.

#### 🔹 R_fc(W): for Fully Connected Layers

Encourages row-wise and column-wise sparsity:

R_fc(W) = ∑ over fc layers l [
∑ over rows: ||W_row_l,:||g +
∑ over columns: ||W:col_l||_g
]


- Promotes dropping full neurons in input/output layers.

---

### 🧠 How the Device Knows What to Prune

During training:
1. Each device tracks which filters, channels, rows, or columns have **low group norm**.
2. These are considered **unimportant** to its task.
3. A binary mask `M_k` is generated to **retain only the important parts**.
4. The subnetwork `W_k = W ⊙ M_k` is used for further training and communication.

---

### 🔁 Subnetwork Evaluation and Update

At each communication round:
- The device evaluates the current subnetwork on its **local validation set**.
- If performance is good and the **pruning rate** hasn’t reached the target:
  - Continue pruning (make the model even smaller)
- If performance drops:
  - Retain the current subnetwork structure

---

### 📊 Figure 4 Explained

- **(a)**: Parameter matrix layout by channel for each device.
- **(b)**: Structured pruning example — entire channels (orange/white) are removed.
- **(c)**: Unstructured pruning — scattered weights removed, resulting in irregular patterns.

---

### ✅ Benefits of Structured Pruning in Hermes

| Feature                  | Benefit                                  |
|--------------------------|-------------------------------------------|
| Personalized Subnetworks | Adapted to device-specific data           |
| Structured Compression   | Efficient to run on real hardware         |
| Lower Communication Cost | Fewer parameters are transmitted          |
| Faster Inference         | Sparse models execute faster on devices   |

---

Hermes uses structured pruning to learn compact, personalized models that are better suited for non-IID data and low-resource devices, enabling scalable and efficient Federated Learning.










---

## 📎 Reference

Li, Ang, et al. *Hermes: An Efficient Federated Learning Framework for Heterogeneous Mobile Clients*. [Add full citation and link here]

