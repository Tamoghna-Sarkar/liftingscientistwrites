<script type="text/javascript"
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

# Learn2pFed: Deep-Dive Summary for Systematic Explanation

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

Learn2pFed proposes a new solution: each client learns a **participation mask** \( \phi_i \) — a vector where each entry lies in \([0, 1]\), indicating the degree to which a model parameter should be shared during aggregation.

### Learnable Mask \( \phi_i \):
- \( \phi_i \approx 1 \) → Fully collaborative: parameter participates in global aggregation.
- \( \phi_i \approx 0 \) → Fully private: parameter is trained and used locally only.
- \( 0 < \phi_i < 1 \) → Partially collaborative.

These masks are **learned** (not predefined) using a technique called **algorithm unrolling**, which lets the system differentiate through the client’s own training process.

> This way, each client determines the optimal level of collaboration for each parameter.

---

## 3. Personalization via \( \phi_i \): Why and How It Works

### 🎯 You're Right:
Each client updates its participation gate \( \phi_i \) using **its own local data and training loss**.

So, naturally:
- Clients optimize \( \phi_i \) in a way that reflects their **own personalization needs**.
- The resulting models are **partially personalized**, leveraging collaborative benefits where it helps, and privatizing where necessary.

### ✅ Why That’s Exactly What We Want

In **personalized federated learning (pFL)**:
- The goal is **not** to make all clients match one model.
- Instead, each client should **learn its own personalized model**.
- Collaboration should occur **only when helpful**.

> Example:
- Client A may find sharing early layers useful (generic features), but keeping deeper layers private improves task performance.
- So \( \phi_A \) evolves such that:
  - \( \phi_i \) high for early layers → high collaboration.
  - \( \phi_i \) low for later layers → high personalization.

Client B, facing a different data distribution or task, might learn the opposite behavior.

### 🧠 Why This Works Despite Local Training

Each client:
1. **Unrolls its own training steps**, simulating multiple local updates.
2. **Computes the final loss** after those steps.
3. **Backpropagates through the entire unrolled computation graph** to adjust \( \phi_i \).

Thus, \( \phi_i \) is updated **not just based on immediate gradients**, but based on the **long-term effect** on personalized performance.

### 🔁 Server's Role in Aggregation

The central server:
- Does **not update or control \( \phi_i \)**.
- **Receives masked updates** from clients (weighted by \( \phi_i \)).
- Aggregates and broadcasts the updated global model.
- Shares **aggregated losses** to help clients refine their \( \phi_i \) masks.

### 🔐 Final Insight
> Learn2pFed doesn't force agreement — it learns how much to agree.

Each client pulls shared parts only as needed. The \( \phi_i \) gates balance this, resulting in a **continuum between full personalization and full collaboration**.

---

## 4. Algorithm Unrolling: Letting Clients Learn How to Train

### Gradient Descent Recap:
Standard gradient descent:

$$
w_{t+1} = w_t - \eta \nabla L(w_t)
$$

This is a fixed algorithm — it does not allow the system to learn *how* to optimize.

### Algorithm Unrolling:
We "unroll" T steps of optimization:

```text
Step 1: x₁ = f(x₀)
Step 2: x₂ = f(x₁)
...
Step T: x_T = f(x_{T-1})
```

Each step becomes a **layer** in a computation graph.

- Loss is computed at the final step.
- Gradients are backpropagated through the entire chain.

> This lets us **learn meta-parameters** — such as the \( \phi_i \) mask — by treating the optimization process itself as differentiable.

In Learn2pFed, this is how each client **learns which parameters to share**, over multiple simulated training steps.