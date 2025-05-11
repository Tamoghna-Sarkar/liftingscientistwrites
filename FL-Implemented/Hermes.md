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


## 3.3 Personalization-Preserving Aggregation

In each communication round, Hermes enables participating devices to send their **personalized subnetworks** to the central server for aggregation. However, unlike traditional FL methods such as FedAvg, **Hermes cannot average all model parameters** — because each device may have a **different subnetwork structure** due to structured pruning.

---

### ❌ Problem with Traditional Aggregation

- In FedAvg, the server averages **all parameters**, assuming that each device trains the **same full model**.
- But in Hermes, different devices may **prune different layers, channels, or filters**.
- Directly averaging all parameters would **destroy the personalized structure** learned by each device.

---

### ✅ Hermes' Solution: Personalization-Preserving Aggregation

Hermes uses a **mask-aware aggregation strategy**:
- It only **averages the parameters that multiple devices share** — i.e., the **intersected parameters**.
- Any parameter that is **unique to a device’s subnetwork (i.e., non-intersected)** is left **unchanged**.

This helps:
- Share knowledge where overlap exists
- Retain device-specific adaptations in pruned regions

---

### 🔁 Workflow Summary

1. Each device sends:
   - Its subnetwork weights: `W_k`
   - Its binary mask: `M_k`
2. The server:
   - Identifies **intersected parameters** using the masks from all devices.
   - Averages only the intersected parameters.
   - Leaves all other parameters untouched.
3. Server sends back:
   - The merged parameter matrix (`W_merged`)
4. Each device applies its own mask again:

W_k^{T+1} = W_merged ⊙ M_k

This ensures:
- Intersected weights are updated from global aggregation
- Personalized, non-intersected weights are preserved

---

### 📊 Toy Example

Imagine a model with **5 parameters**:  

[W1, W2, W3, W4, W5]


#### Device A's Mask:

[1, 1, 0, 0, 1] → keeps W1, W2, W5


#### Device B's Mask:

[1, 0, 1, 0, 1] → keeps W1, W3, W5


#### Intersected Parameters (shared):
- W1 and W5 (kept by both A and B)

#### Non-Intersected:
- W2 (only on A)
- W3 (only on B)

#### At the Server:
- Average W1 and W5
- Leave W2 and W3 unchanged

#### When Device A receives `W_merged`:
- Applies its mask:

W_A = W_merged ⊙ [1, 1, 0, 0, 1]

- Gets:
- Updated W1 and W5 (from aggregation)
- Keeps its own W2
- Ignores W3 and W4 (pruned anyway)

---

### ✅ Benefits

| Feature                         | Benefit                                      |
|--------------------------------|----------------------------------------------|
| Intersection-only aggregation  | Prevents corruption of personalized structure |
| Lightweight binary mask        | Enables accurate parameter alignment          |
| Personalized + Collaborative   | Balances individual learning and global sharing |

---

By aggregating only shared parts and reusing binary masks to recover personalized structures, Hermes effectively enables both **collaborative learning** and **local adaptation** in a communication-efficient way.


## 🧠 Algorithm 1: Training Algorithm of Hermes (Compact Line-by-Line Explanation)

This algorithm outlines the training process in Hermes, where a global model `W` is trained collaboratively across multiple devices with local, private, and non-IID data `(D1, ..., DN)`. The server first initializes the full dense global model `W`. For each communication round `T`, it samples a subset `k = max(N × K, 1)` of the available devices, ensuring at least one device is selected. This subset `S_c` is randomly drawn from the device pool `{C1, ..., CN}`. Then, for each device `C_k` in `S_c` (executed in parallel), the device extracts its own subnetwork by applying its mask `M_k^T` to the global model: `W_k^T = W^T ⊙ M_k^T`. The device then performs local training on this subnetwork using the `ClientUpdate` function and returns the updated subnetwork parameters `W_k^{T+1}`. After all participating devices finish their local updates, the server aggregates the set `{W_k^{T+1}}` using the personalization-preserving aggregation strategy: only the intersected parameters (present across devices) are averaged, and the rest are left unchanged. 

On the client side, `ClientUpdate(C_k, W_k^T)` begins by evaluating the subnetwork `W_k^T` on the local validation data `D_k^{val}` to obtain an accuracy score `acc`. If this accuracy exceeds a predefined threshold `acc_threshold` and the current pruning rate `r_k^T` is still less than the target pruning rate `r_target`, the client prunes `W_k^T` further using a fixed rate `r_p`, generating a new binary mask `M_k^{T+1}`. The local training data `D_k^{train}` is then split into batches `B`, and for `E` local epochs, the device performs stochastic gradient descent only over active (unpruned) weights: for each batch `b`, the update rule is `W_k^T ← W_k^T ⊙ M_k^T − η ∇F_k(W_k^T ⊙ M_k^T, b)`, where `η` is the learning rate and `F_k` is the loss function. Finally, the updated subnetwork weights `W_k^{T+1}` and the new mask `M_k^{T+1}` are returned to the server for aggregation in the next communication round.


## 4. Theoritical Convergence Analysis is SKIPPED

## 5. Evaluations: Explanation and Setup

This section evaluates Hermes under real-world FL settings and compares it against several popular baselines. The experiments focus on verifying Hermes’ ability to achieve high accuracy while reducing memory, energy, and communication costs — especially in non-IID environments.

---

### 5.1 Applications, Datasets, and Models

To test Hermes’ generality, two types of mobile AI applications are used:

#### 🧠 Application #1: Image Classification

- **Why**: A fundamental deep learning task, increasingly performed on smartphones.
- **Models Used**: VGG16 for EMNIST and CIFAR-10.
- **Datasets**:
  - **EMNIST**: A handwriting dataset where each writer’s data is assigned to a different device → non-IID by writer.
  - **CIFAR-10**: Each device is assigned 2 classes out of 10, with an imbalanced distribution across devices → non-IID by label.

This simulates realistic mobile data heterogeneity, such as each user primarily capturing a small subset of image types.

#### 🏃 Application #2: Human Activity Recognition (HAR)

- **Why**: Smartphones often track human activity using motion sensors (e.g., for fitness or health apps).
- **Dataset**: HAR dataset with 6 activity classes collected from 30 individuals.
- **Model**: A 3-layer fully connected neural network.
- **Data Split**: Each user’s data is assigned to one device, making it naturally non-IID.

---

### 📊 Dataset Summary (Table 1)

| Dataset  | # Devices | # Classes | Non-IID |
|----------|-----------|-----------|---------|
| EMNIST   | 2414      | 64        | ✅      |
| CIFAR-10 | 400       | 10        | ✅      |
| HAR      | 30        | 6         | ✅      |

---

### 5.2 System Implementation

Hermes is deployed in a real FL setup:

- **Client Devices**: Google Pixel 3 smartphones (Android 9.0, 3-core CPU).
- **Server Machine**: Intel Xeon E5-2630 @ 2.6GHz, 128GB RAM.
- **Framework**: PyTorch 1.5.
- **Power Measurement**: Monsoon Power Monitor [38] used to track energy consumption during runtime.

#### ⚙️ FL Protocol Configuration

- 20 clients are selected randomly per round.
- Each client trains for 5 local epochs per round.
- **Pruning and training parameters**:
  - `r_p = 0.2`: prune 20% of weights per round.
  - `B = 16`: batch size.
  - `acc_threshold = 0.5`: stop pruning if accuracy drops below this.
  - `r_target = 0.3`: target pruning ratio (i.e., 30% of model pruned at most).

---

### 5.3 Experimental Setup

#### 🔁 Compared Baselines

1. **Standalone**: Trains only on local data with no collaboration. Represents pure personalization without communication.
2. **FedAvg [36]**: Classical FL baseline with full model synchronization.
3. **Top-k [1]**: Compresses updates by sending only the k-largest gradient elements to reduce bandwidth.
4. **Per-FedAvg [12]**: Adds MAML-based meta-learning to FedAvg. Allows per-device fine-tuning from a shared initialization.
5. **LG-FedAvg [32]**: State-of-the-art FL method that jointly trains global and local representations for both personalization and compression.

All methods use the same model architectures and data splits. Hermes and all baselines are trained for the same number of communication rounds and local epochs, except Standalone (which trains longer to compensate for lack of collaboration).

---

### 📏 Evaluation Metrics

Two major metric categories are used:

#### 1. Training Quality
- **Inference Accuracy**: Accuracy on each device’s test set.
- **Communication Cost**: Time cost for uploading/downloading updates in each round.

#### 2. Runtime Efficiency
- **Memory Footprint**: RAM required by each model during inference.
- **Inference Latency**: Average time taken per prediction.
- **Energy Consumption**: Battery power used per inference (measured on real devices).

These metrics comprehensively capture both model quality and resource usage — essential for real-world FL deployment.


## 5.4 Convergence Speed

To validate Hermes' training efficiency, the authors compare its convergence behavior with that of two popular baselines: **FedAvg** and **Top-k**. Figure 6 presents the training loss curves across 100 communication rounds for three datasets: CIFAR-10, EMNIST, and HAR.

### 📊 Figure 6 Observations

- **Hermes** (red line) achieves the **fastest convergence** and the **lowest final training loss** in all three tasks.
- **FedAvg** (green) and **Top-k** (blue) both converge slower and tend to plateau at higher loss values.
- This confirms Hermes' ability to learn effectively despite non-IID data and partial model sharing.

➡️ **Conclusion**: Hermes is significantly more communication-efficient and converges faster than conventional FL baselines by training compact, personalized subnetworks.

---

## 5.5 Inference Accuracy vs. Communication Cost

The authors also evaluate the tradeoff between **model performance** (accuracy) and **communication cost** using Figure 7. This plot compares Hermes to five baselines:

- **Standalone**
- **FedAvg**
- **Top-k**
- **Per-FedAvg**
- **LG-FedAvg**

Each point in the figure represents one method’s position in the **accuracy vs. communication cost space** for each dataset.

### 📊 Figure 7 Observations

- Hermes (⭐) consistently appears in the **lower-right** region — meaning **low communication cost** and **high accuracy**.
- While Top-k offers the **lowest communication cost**, it suffers from poor accuracy.
- Per-FedAvg and LG-FedAvg provide good accuracy but at much higher communication costs.
- Hermes strikes the **best balance** across all three datasets.

---

### 🔢 Quantitative Results

#### 📌 Hermes vs. LG-FedAvg
- **Accuracy Improvement**:
  - +8.93% (CIFAR-10)
  - +3.23% (EMNIST)
  - +0.53% (HAR)
- **Communication Reduction**:
  - 1.96× (average)

#### 📌 Hermes vs. Per-FedAvg
- **Communication Cost Reduced**:
  - 3.05× (CIFAR-10)
  - 3.25× (EMNIST)
  - 3.48× (HAR)
- **Accuracy Gain**:
  - +12.55%, +3.46%, and +0.6% respectively

#### 📌 Hermes vs. Top-k
- **Higher Accuracy** by:
  - +32.17% (CIFAR-10)
  - +8.71% (EMNIST)
  - +2.39% (HAR)

➡️ **Conclusion**: Hermes may not always be the single best in either accuracy or communication, but it offers the **most balanced performance**, making it ideal for real-world FL deployments where both factors matter.

## 5.6 Hyper-Parameter Evaluation

The authors conduct a series of experiments to analyze how different **hyperparameters** affect Hermes' performance, focusing on:

### 📌 Number of Participating Devices

- **Setup**: Devices per round = {20, 40, 80}
- **Datasets**: IC-EMNIST, IC-CIFAR10
- **Observation** (Figure 8): 
  - Increasing the number of participating devices per round **slightly improves accuracy**.
  - On IC-CIFAR10, accuracy improves by 1.75% when increasing from 20 to 80 devices.
  - However, this also causes **4× higher bandwidth usage**, limiting practical benefit.

➡️ **Conclusion**: More devices can help, but the communication overhead grows significantly.

---

### 📌 Data Volume and Balance Rate

Hermes is evaluated under **challenging conditions** where devices have:
- Very **limited data**, and
- **Unbalanced class distributions**

- **Setup**:
  - Number of samples/class = {5, 10, 20}
  - Balance rate = {0.25, 0.5, 0.75, 1.0}
- **Balance Rate Definition**:
  - Ratio of the minor class to major class per device
  - Lower = more unbalanced

- **Observation** (Figure 9):
  - **More data** = better performance
  - **More balance** = better performance
  - For instance:
    - At 20 samples/class:
      - Accuracy drops from 84.35% → 83.67% when balance rate drops from 1.0 → 0.5
    - At balance rate 0.75:
      - Accuracy drops by only 0.92% when reducing from 10 to 5 samples/class

➡️ **Conclusion**: Hermes remains **robust** even under extreme data imbalance and scarcity.

---

### 📌 Target Pruning Rate (r_target)

- **r_target** defines the **final sparsity goal** for each device.
- Higher `r_target` = more pruning = smaller subnetwork

- **Setup**:
  - r_target = {0.3, 0.5, 0.8}
- **Observation** (Table 2 summary):
  - On IC-CIFAR10:
    - Accuracy drops slightly from 86.35% → 85.72% when increasing pruning from 0.3 → 0.8
    - Communication cost drops **46%**

➡️ **Conclusion**: Hermes can trade off **minimal accuracy loss** for **significant communication reduction** by increasing model sparsity.

---

## 5.7 Runtime Performance

Hermes offers major advantages in runtime resource usage due to its **structured sparse subnetworks**:

### 🧠 Memory Footprint

- Each device only needs to store and run its **own pruned model**.
- Authors measure model size to quantify memory reduction.
- Hermes’ personalized models are **much smaller** than the full dense model used in FedAvg or other baselines.

➡️ **Conclusion**: Hermes is more suitable for mobile and embedded settings due to **smaller memory and compute requirements**.


## 5.7 Runtime Performance

Hermes offers significant runtime advantages due to its **structured sparsity**, which allows each device to store and run a smaller, personalized subnetwork.

---

### 📦 Memory Footprint (Table 3)

Hermes significantly reduces the model size compared to full baselines.

| Application | Hermes Model Size (MB) | Baseline Model Size (MB) | Reduction |
|-------------|-------------------------|---------------------------|-----------|
| IC-CIFAR10  | 161.16                  | 537.21                    | ~70%      |
| IC-EMNIST   | 161.43                  | 538.09                    | ~70%      |
| HAR         | 1.32                    | 4.41                      | ~70%      |
| **All Apps**| **323.91**              | **1081.24**               | **757 MB**|

➡️ These savings demonstrate Hermes’ deployability across multiple applications on smartphones.

---

### ⚡ Inference Speedup (Figure 10)

Hermes achieves faster inference due to the reduced model size.

| Application | Inference Speedup |
|-------------|-------------------|
| IC-CIFAR10  | 1.83×             |
| IC-EMNIST   | 1.79×             |
| HAR         | 1.82×             |

➡️ This is especially useful in real-time mobile settings where low-latency responses are critical.

---

### 🔋 Energy Consumption (Figure 11)

Hermes consumes less energy during inference.

| Application | Energy Saving |
|-------------|----------------|
| IC-CIFAR10  | 1.80×          |
| IC-EMNIST   | 1.76×          |
| HAR         | 1.78×          |

➡️ Hermes is well-suited for battery-constrained environments like smartphones and IoT.

---

## 6. Discussion

---

### 🔁 Generality of Hermes

Although tested on two applications (image classification and activity recognition), Hermes can generalize to many mobile AI tasks such as:

- Next-character prediction (e.g., using Shakespeare or Sentiment140 datasets)
- Mobile keyboard prediction
- Sensor-based behavior modeling

➡️ Its design of **structured, personalized sparse models** makes it broadly applicable.

---

### 🔐 Privacy Leakage Mitigation

Recent work shows that **FL can leak private info** via gradients or model updates. Hermes may inherently reduce this risk:

- It sends **only part of the model** (subnetworks), not the entire model.
- **Pruned gradients** are never uploaded.
- This limits how much data a malicious server could use in a **model inversion or property inference attack**.

➡️ Hermes is not a complete privacy framework, but it provides a **structural advantage** in privacy-aware FL systems.

---

📌 **Conclusion**: Hermes is an efficient, generalizable, and privacy-conscious FL framework that performs well under non-IID, resource-constrained settings typical of mobile and edge devices.






---

## 📎 Reference

Li, Ang, et al. *Hermes: An Efficient Federated Learning Framework for Heterogeneous Mobile Clients*. [Add full citation and link here]

