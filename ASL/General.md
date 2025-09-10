<script type="text/javascript"
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

# Notes on Acoustic Localization: GCC-PHAT and SRP-PHAT  
*Date: 2025-09-08*

---

## 1. Acoustic Localization Basics
- Goal: Estimate **Time Difference of Arrival (TDOA)** between microphones to localize sound.
- If sound reaches Mic 1 before Mic 2, the delay gives direction/location.

---

## 2. Cross-Correlation
**Definition**: Measures similarity between two signals as one is shifted in time.  

$$
R_{12}(\tau) = \int x_1(t) \, x_2(t+\tau) \, dt
$$

- Peak of \(R_{12}(\tau)\) occurs at the true time delay.
- Intuition: When signals align → maximum overlap → maximum correlation.

**Relation to Convolution**:  
- Cross-correlation is like convolution **without flipping**.  

Convolution:  
$$
(x_1 * x_2)(t) = \int x_1(\tau) \, x_2(t-\tau) \, d\tau
$$

Cross-correlation:  
$$
R_{12}(\tau) = \int x_1(t) \, x_2(t+\tau) \, dt
$$

---

## 3. Frequency Domain Formulation
Correlation can be computed from the **cross-spectrum**:

$$
R_{12}(\tau) = \int_{-\infty}^{\infty} G_{12}(f) \, e^{j2\pi f \tau} \, df
$$

where  

$$
G_{12}(f) = X_1(f) \, X_2^*(f)
$$

- \(X_1(f), X_2(f)\): Fourier transforms of the signals.  
- \(X_2^*(f)\): complex conjugate.  
- Called the **cross-power spectral density**.

---

## 4. Generalized Cross-Correlation (GCC)
Add a **weighting function \(\Psi(f)\)** for robustness:

$$
R_{12}(\tau) = \int_{-\infty}^{\infty} \Psi(f) \, G_{12}(f) \, e^{j2\pi f \tau} \, df
$$

- Different choices of \(\Psi(f)\) → different GCC variants.

---

## 5. PHAT (Phase Transform)
**Weighting function**:

$$
\Psi(f) = \frac{1}{|G_{12}(f)|}
$$

So:

$$
R_{12}(\tau) = \int_{-\infty}^{\infty} \frac{G_{12}(f)}{|G_{12}(f)|} \, e^{j2\pi f \tau} \, df
$$

- Removes magnitude, keeps only phase.  
- Produces **sharper peaks** → more accurate delay estimation.  
- Works well in noisy/reverberant conditions.

**This is GCC-PHAT.**

---

## 6. SRP-PHAT (Steered Response Power with PHAT)
- Extension for **microphone arrays** (M > 2).
- Idea: “Steer” array to a candidate location \(\mathbf{r}\).
- Compute expected delays \(\tau_{ij}(\mathbf{r})\) for each mic pair.
- Sum GCC-PHAT values at those delays:

$$
P(\mathbf{r}) = \sum_{i<j} R_{ij}(\tau_{ij}(\mathbf{r}))
$$

- Location with maximum \(P(\mathbf{r})\) is chosen as source.  
- Robust method for **3D localization**.

---

## 7. Key Insights
- Cross-correlation = similarity measure.  
- Peak = estimated time delay.  
- GCC = generalized with weighting.  
- PHAT = normalize magnitude, keep phase → sharper peaks.  
- SRP-PHAT = array-based extension using spatial steering.

---
