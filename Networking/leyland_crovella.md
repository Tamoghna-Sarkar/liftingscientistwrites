
# Exam Prep: **TIMESCALES, how different it is with self-simalr and Poisson traffic**


# self similar: parts look like each other at different timescales

Why network traffic is self-similar?
# Crovella point: the network, protocol, system is not responsible for why the network traffic is self-similar(pareto heavy tailed), the main reason is the USERs who are all doing things similarly at some part of the world over the web and it all overlaps(the behavior).
## superposition of heavy-tailed sizes results in self-similarity
### Leyland abstractly says this but Crovella solidifies this with evidence



## a bit about heavy-tail:
# Heavy-Tailed Distributions

## Overview
A **heavy-tailed distribution** is a probability distribution with a tail that decays slower than exponential distributions. This means it has a higher probability of producing extreme values.

### Key Features:
1. **Slow Decay**: Probability of large values decreases slowly, often following a power-law:
   \[
   P(X > x) \sim x^{-\alpha}, \quad 0 < \alpha < 2
   \]
2. **Infinite Variance or Mean**:
   - If \( 1 < \alpha < 2 \): Variance is infinite.
   - If \( 0 < \alpha < 1 \): Both mean and variance are infinite.
3. **Outliers**: High probability of rare, extreme values.

### Examples of Heavy-Tailed Distributions:
1. **Pareto Distribution**: Models wealth and internet file sizes.
2. **Cauchy Distribution**: Infinite variance.
3. **Log-Normal Distribution**: Common in file size distributions.

---

## Networking and Heavy-Tailed Distributions

### Role in Networking:
Heavy-tailed distributions are prevalent in networking systems and traffic modeling:
1. **File Sizes**: Web traffic exhibits heavy-tailed file size distributions where a few very large files dominate the traffic.
2. **Burstiness**: Leads to long-range dependence and persistent bursty traffic.
3. **Traffic Patterns**: Packet inter-arrival times often follow heavy-tailed distributions, contributing to self-similarity.

### Example:
- **File Size Distribution in Web Traffic**:
  - Majority of web files are small, but a small number of very large files account for a significant portion of total data transferred.
  - This can overwhelm queues and cause unpredictable traffic behavior, making system design more challenging.

### Implications:
- **System Challenges**: Networking systems must handle rare but impactful events (e.g., large traffic bursts).
- **Self-Similarity**: Superimposing many processes with heavy-tailed ON/OFF times results in traffic with long-range dependence.
---

## Summary
Heavy-tailed distributions model scenarios where extreme values occur more frequently than expected. In networking, they explain bursty traffic, long-range dependence, and challenges in traffic management.
----------------------

### How Heavy-Tails Explain Long-Range Dependence (LRD)

1. **Heavy-Tailed Distributions**:
   - Distributions with a "heavy tail" have extreme values that occur with non-negligible probability.
   - Examples: File sizes, inter-arrival times in network traffic.

2. **Connection to LRD**:
   - **Heavy-tailed events** (e.g., long file transfers) create **bursts** that persist over time.
   - These bursts cause the autocorrelation \( r(k) \) to decay **slowly (hyperbolically)**, leading to **long-range dependence (LRD)**.
   - Unlike exponential decay, heavy-tailed processes maintain correlations across long timescales.

3. **Networking Example**:
   - **Web traffic**: Heavy-tailed file sizes lead to persistent high demand ("bursty" traffic).
   - **Impacts**: Causes **self-similar traffic**, challenging traditional Poisson-based network models.

4. **Key Takeaway**:
   - **Heavy tails = Long bursts = Long-range correlations**, explaining why traffic is bursty at all scales.


-----------------------

Slide 34: average over different times: all lead to diff averages, and hence timescale matters, all true averages dyring different timescales but completely different in nature.


Slide 141: traffic at different timescales from [willinger98a]
  - the above 2 diags are fine timscale, the below are wide timescales, at every level they take a random interval and expand it.
  - they look at 2 diff traffic models: real world traffic models on the right and the other is artificial generated with poisson
  - What do they teach us?
      - the real world traffic is really bursty even at different large timescales whereas in poisson traffic smooths out, the longer timescale you look it smooths out, as it has a mean.
      - This is what leyland shows, nowhere in the timescale does real work traffic smooths out
      - poisson is not a good fit for real world networking
  Slide 165: Long-range dependence (LRD) is a statistical property of certain time series or processes where values far apart in time are strongly correlated with each other, meaning that events happening now are influenced by events that happened a long time ago. This dependence persists over long timescales.
-  Hurst Parameter (H)
    - One of the ways to characterize long-range dependence is by the Hurst parameter HH:
        For a self-similar process, 0.5<H<10.5<H<1 indicates long-range dependence. The closer HH is to 1, the stronger the memory of the process. It implies that the current state is heavily influenced by its past states even if they occurred far back in time.
    A Hurst parameter of 0.5 or lower typically indicates that the process is more like white noise (no correlation between values) or short-range dependent.   

Slide 178: Pre-Time Variance Plot: **Variance looks at burstiness over different timescakes**
Log10(m): timescales, x1, x2 in the previous slides where they descirbed how to see and aggregate timescales
log10(variance): burstiness
all graphs are log scales, and measure varinaces in each small timescales to large timescales.
# RESULT: as timscales increases, variance decreases SLOWLY(than linear decrease as log-log plot)
In poisson traffic variance falls off more rapidly as the curve smoothens out wrt time, but here variance faills off more slowly than we expect.

slide 204: 
autocorrelation: how much do i look like myself **looks at how ssimilar we are at diff timescales**
## compare the shift patterns to different timescales- to see self-similarity
self-similaroty should have a large auto-correlation because it is **LONG-RANGE DEPENDENT** or bursty, therefore AC is a nice metric to study self-similarity

# slide 208: 
AC is large and hence demos SS

Next, we will compare range against SDev

Slide219: range/SD Vs timescales
-  for poisson traffic SD is going to drop to 0, but for SS standdev doesnt drop to 0 that quickly,

## R/S Plot Analysis and Long-Range Dependence

### Overview of R/S Plot
- **R/S Plot** (Rescaled Range) is a statistical tool used to determine if a time series exhibits **self-similar** behavior, which is often a sign of **long-range dependence**.
- The R/S statistic analyzes **autocorrelation** over multiple time scales, helping to identify if the statistical properties of a time series remain consistent even when aggregated.

### Self-Similarity and Long-Range Dependence
- **Self-similarity** in network traffic implies that the data is **bursty** over many different time scales, from milliseconds to seconds to minutes. Unlike processes like Poisson, the burstiness of self-similar traffic doesn't smooth out over time.
- The **autocorrelation function** for self-similar traffic decays **hyperbolically** rather than **exponentially**, indicating a significant correlation between values even far apart in time.
  - **Poisson Process**: The autocorrelation decays quickly, roughly as \( k^{-1} \), meaning that after a short time, there's almost no correlation left.
  - **Self-Similar Traffic**: Autocorrelation decays as \( k^{-\beta} \) (with \( 0 < \beta < 1 \)), meaning correlations decay more slowly and remain significant over longer periods. This is characteristic of **long-range dependence**.

### Characteristics of Long-Range Dependence
- **Long-Range Dependence** (LRD) means that the time series values at a given point are correlated with values far in the future, leading to persistent patterns and burstiness.
- In the case of **self-similar processes**, the **Hurst parameter** \( H \) (with \( 0.5 < H < 1 \)) quantifies the degree of long-range dependence:
  - \( H = 0.5 \) corresponds to a **memoryless** process like a Poisson process.
  - \( H > 0.5 \) indicates **positive correlation** at long time scales, contributing to long-range dependence.

### R/S Plot Specifics
- In the **R/S Plot**, the rescaled range statistic is plotted against different aggregation levels, typically on a **log-log scale**. 
  - If the resulting plot is a **straight line**, it is indicative of **self-similarity**.
  - The **slope of the line** gives an estimate of the **Hurst parameter \( H \)**.
  - The formula for the Hurst parameter from the slope (\( \beta \)) is: 
    \[
    H = 1 - \frac{\beta}{2}
    \]

### Implications of the Plot
- The **variance** of the aggregated series decreases **slower** than it would for a traditional Poisson process. This means that even when we aggregate the data at larger time scales, the burstiness persists.
- The **slope** of the R/S plot (typically greater than -1 for self-similar traffic) confirms that the variance reduction isn't as rapid as for Poisson, which implies **greater variability** and **persistence** in network traffic.
  
### Summary Points
- **Self-similar traffic**: Burstiness across multiple time scales, **long-range dependence**, slowly decaying autocorrelation.
- **Poisson traffic**: Short bursts, **exponentially decaying autocorrelation**, minimal long-range dependence.

### Key Takeaways
- **R/S Plot** is a critical tool to determine **self-similarity** and **long-range dependence** in time series.
- **Self-similarity** in network traffic results in persistent, bursty behavior that doesn't average out, which has important implications for **network design**, **buffering**, and **traffic management**.

Slide 224: frequency is a measure of timescale vs amount of power in the frequency(periodogram) **frequency looks at how much energy we have at different timescales**
## another method of seeing large timescales vs small timescales

All the same idea in the plots but measuring different aspects of the same thing.



slide 288: Web On times(how long is each tcp connection open):
real data curve pretty muhch like a straight line, so from 3 to 0 is three orders of magnitude linear decrease and hence its pareto heavy-tailed and that would explain Self-Similarity
that led us to file sizes as we keep opening new flows and send-get files..
file sizes are heavy tailed:
why?
some web-pages are light, medium and heavy

# FINAL QUESTION: what happens at really large levels of aggregation: follow slide

