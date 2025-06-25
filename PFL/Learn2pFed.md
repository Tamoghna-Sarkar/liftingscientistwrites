<script type="text/javascript"
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

# Learn2pFed: Learn What You Need in Personalized Federated Learning

---


## 1. Introduction: Motivation for Learn2pFed

### The Problem:
Traditional federated learning (FL) assumes that all clients contribute to and benefit from a shared global model. This assumption breaks down in **real-world non-IID settings** where client data distributions differ significantly — a setting common in edge devices, personalized services, and privacy-preserving analytics.

### Limitations of Existing Methods:
- **FedAvg / FedProx**: Share the full model — no personalization.
- **Fine-tuning or layer-splitting**: Manually specify what to share vs keep private.
- **Drawback**: These approaches are either too general (no personalization) or too rigid (not adaptive).

> **Key Challenge:** How can we let each client decide dynamically which parts of its model should be shared with others, and which should remain private?

---

## 2. Core Idea of Learn2pFed

Learn2pFed proposes a new solution: each client learns a **participation mask** \( φᵢ \) — a vector where each entry lies in \([0, 1]\), indicating the degree to which a model parameter should be shared during aggregation.

### Learnable Mask \( φᵢ \):
- \( φᵢ \approx 1 \) → Fully collaborative: parameter participates in global aggregation.
- \( φᵢ \approx 0 \) → Fully private: parameter is trained and used locally only.
- \( 0 < φᵢ < 1 \) → Partially collaborative.

These masks are **learned** (not predefined) using a technique called **algorithm unrolling**, which lets the system differentiate through the client’s own training process.

> This way, each client determines the optimal level of collaboration for each parameter.

---

## 3. Personalization via \( φᵢ \): Why and How It Works

###  You're Right:
Each client updates its participation gate \( φᵢ \) using **its own local data and training loss**.

So, naturally:
- Clients optimize \( φᵢ \) in a way that reflects their **own personalization needs**.
- The resulting models are **partially personalized**, leveraging collaborative benefits where it helps, and privatizing where necessary.

###  Why That’s Exactly What We Want

In **personalized federated learning (pFL)**:
- The goal is **not** to make all clients match one model.
- Instead, each client should **learn its own personalized model**.
- Collaboration should occur **only when helpful**.

> Example:
- Client A may find sharing early layers useful (generic features), but keeping deeper layers private improves task performance.
- So \( \phi_A \) evolves such that:
  - \( φᵢ \) high for early layers → high collaboration.
  - \( φᵢ \) low for later layers → high personalization.

Client B, facing a different data distribution or task, might learn the opposite behavior.

###  Why This Works Despite Local Training

Each client:
1. **Unrolls its own training steps**, simulating multiple local updates.
2. **Computes the final loss** after those steps.
3. **Backpropagates through the entire unrolled computation graph** to adjust \( φᵢ \).

Thus, \( φᵢ \) is updated **not just based on immediate gradients**, but based on the **long-term effect** on personalized performance.

###  Server's Role in Aggregation

The central server:
- Does **not update or control \( φᵢ \)**.
- **Receives masked updates** from clients (weighted by \( φᵢ \)).
- Aggregates and broadcasts the updated global model.
- Shares **aggregated losses** to help clients refine their \( φᵢ \) masks.

###  Final Insight
> Learn2pFed doesn't force agreement — it learns how much to agree.

Each client pulls shared parts only as needed. The \( φᵢ \) gates balance this, resulting in a **continuum between full personalization and full collaboration**.

---

## 4. Algorithm Unrolling: Letting Clients Learn How to Train

### Gradient Descent Recap:
Standard gradient descent:

$$
wₜ₊₁ = wₜ - η \nabla L(wₜ)
$$

This is a fixed algorithm — it does not allow the system to learn *how* to optimize.

### Algorithm Unrolling:
We "unroll" T steps of optimization:

```text
Step 1: x₁ = f(x₀)
Step 2: x₂ = f(x₁)
...
Step T: xₜ = f(xₜ₋₁)
```

Each step becomes a **layer** in a computation graph.

- Loss is computed at the final step.
- Gradients are backpropagated through the entire chain.

> This lets us **learn meta-parameters** — such as the \( φᵢ \) mask — by treating the optimization process itself as differentiable.

In Learn2pFed, this is how each client **learns which parameters to share**, over multiple simulated training steps.


---

## 5. Learn2pFed Algorithm Step-by-Step

### Initialization:
For each client i, initialize:
- Local model \( vᵢ \)
- Participation mask \( φᵢ \)
- Dual variable \( lpha_i \) (from ADMM-based formulation)

Server initializes global model \( u \)

### Iterative Training (Layer-wise):
For each layer \( l = 1 \) to \( L \), and for \( E \) epochs:

#### ▶ Client-Side:
- Run \( T \) unrolled steps of local training on \( vᵢ \)
- Update \( lpha_i \) (dual variable)
- Update \( φᵢ \) by backpropagating through the \( T \)-step computation
- Send \( \omega_i = vᵢ - lpha_i \) to server

####  Server-Side:
- Aggregate \( \omega_i \) from all clients → update \( u \)
- Broadcast updated global model \( u \) and loss feedback

####  Clients Again:
- Use broadcasted global loss to update \( φᵢ \)

After all layers are trained, return personalized models \( \{vᵢ^L\} \)

---

## 6. Why Unrolling Depth Matters

> More unrolled steps → better learning of \( φᵢ \)

### Why?
- Captures **longer-term training effects** of parameter sharing.
- Better models the **impact of collaboration vs personalization**.
- More depth = more accurate gradients for updating \( φᵢ \).

### Result:
Deeper unrolling improves convergence and final performance.
(Think of it like chess: looking 10 moves ahead is better than 2.)

---

## 7. Experimental Results

### Datasets:
- **Synthetic polynomial regression**
- **Electricity load forecasting** (real-world time series)
- **Image classification** (CIFAR-10, FMNIST)

### Key Findings:
- Learn2pFed consistently outperforms FedAvg, FedProx, Ditto, pFedMe, and others.
- Achieves:
  - **Lowest RMSE** in regression
  - **Best accuracy** in classification
  - **>90% reduction in communication cost**
  - **>9x reduction in FLOPs**

---

## 8. Ablation Studies & Insights

### What Matters Most:
- Learning \( φᵢ \) is critical.
- Learning global parameters like \( 
ho \), \( γ \) adds improvement, but less impactful.

### Layer Selection:
- Starting personalization from **deeper layers** significantly reduces communication without hurting accuracy.
- Fully personalized models require more communication and offer diminishing returns.

---

## 9. Privacy and Efficiency

- All data stays local; only masked model updates are shared.
- Gradient information is mixed and aggregated, making reverse engineering difficult.
- By learning only the **last layer** of CNNs in some settings, communication and FLOPs are greatly reduced.

---

## 10. Final Conclusion

Learn2pFed is a principled, efficient, and adaptive framework for personalized FL that learns **how much to collaborate per parameter, per client**.

- Offers the **flexibility of personalization** with the **power of collaboration**.
- Efficient in communication, computation, and convergence.
- Highly extensible for real-world edge FL deployments.

> It doesn't force uniformity. It learns collaboration boundaries adaptively — making FL truly personal.

### Notation Example:
Where \( 	heta = \{	heta_1, \dots, 	heta_n\} \)

---

Let me know if you'd like a LaTeX version of this document, or a PDF export.