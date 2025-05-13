# A Variegated Look at 5G in the Wild: Performance, Power, and QoE Implications

## Overview
This paper provides an in-depth examination of **5G performance**, focusing on:
- **Network performance**, **power consumption**, and **QoE** in real-world 5G deployments.
- Comparison of **5G** to **4G/LTE** in different configurations like **mmWave**, **low-band 5G**, and **NSA/SA 5G**.
- Key insights into the performance of **Adaptive Bitrate (ABR) streaming** algorithms and **web browsing** over 5G.

---

## Key Topics
1. **5G Performance vs. 4G**  
    - 5G offers **higher throughput** and **lower latency** but at a **higher power cost**.
    - **mmWave 5G** shows incredible speed improvements, especially in high-throughput scenarios,so **high bandwidth** but suffers from **signal degradation due to shorter range, more interference** with increasing distance. 
    - **Low-band 5G** provides more stable performance at longer ranges compared to **mmWave 5G**, though with lower peak throughput.

2. **Radio Resource Control (RRC) State Transitions**
    - **RRC_INACTIVE** state in **SA 5G** helps reduce power consumption, allowing UEs to maintain low-power states when not actively transmitting.
    - **NSA 5G** devices consume more energy due to longer times spent in **RRC_CONNECTED** and frequent handoffs between 4G and 5G.
    - **mmWave 5G** consumes more energy, especially during **RRC state transitions**, but becomes more efficient at higher throughput rates.

3. **Power Consumption in 5G vs. 4G**
    - **5G mmWave** shows **48-76% of total power** used for data transfer (both downlink and uplink), compared to **21-53% in 4G**.
    - **mmWave 5G** becomes **more energy-efficient** than 4G/LTE when throughput exceeds **187 Mbps** (downlink) and **189 Mbps** (uplink).
    - For **low-throughput scenarios**, **4G LTE** and **low-band 5G** are more power-efficient.

---

## 5G and Adaptive Bitrate (ABR) Streaming
### 1. **Performance of ABR Algorithms**
- The study compares seven ABR algorithms, categorized into **buffer-based**, **throughput-based**, **control theory-based**, and **machine learning-based**:
    - **FastMPC** and **robustMPC** perform best across both 4G and 5G, minimizing stalls and optimizing video quality.
    - **Pensieve**, a machine learning-based ABR algorithm, shows **58.2% higher video stalls** over 5G compared to 4G, due to throughput prediction issues.
    - **Buffer-based** algorithms (e.g., BBA, BOLA) perform poorly under 5G due to their reliance on buffer size and inability to adapt to real-time network changes.

### 2. **Challenges in ABR for 5G**
   - **Throughput prediction** is a critical challenge in 5G due to **variable throughput conditions**.
   - **Chunk length** impacts QoE, with shorter chunks (1-2s) resulting in better video performance. Longer chunks (4-8s) degrade QoE due to delayed bitrate adjustments.
   - **Interface selection** (switching between 4G and 5G) improves QoE and reduces energy consumption.

### 3. **Proposed ABR Solutions**
   - The paper recommends **5G-aware ABR algorithms** to account for 5G's throughput variability.
   - Fine-grained bitrate decisions with smaller chunks and **adaptive interface selection** between 4G and 5G can enhance performance and reduce energy consumption.

---

## Web Browsing over 5G
### 1. **Page Load Time (PLT) vs. Energy Consumption**
- **mmWave 5G** improves **PLT** for larger websites or pages with many objects, reducing load times by up to **50%**.
- For **smaller tasks**, **4G is more energy-efficient**, as mmWave’s power cost for loading small pages outweighs the performance benefits.
  
### 2. **Dynamic Interface Selection for Energy Efficiency**
- The study proposes a **dynamic interface selection model** that switches between **4G and 5G** based on **page size** and **content complexity**.
    - For pages smaller than 4 MB, **4G** is selected to save energy.
    - For larger pages, **5G** is chosen to reduce load time.
- This model offers **26.9% energy savings** while maintaining fast page load times.

### 3. **Data Collection for Web Browsing**
- Web browsing data was collected using **Google Chrome** across **1,500 websites**. The experiments show **higher energy consumption** for **mmWave 5G**, especially for smaller websites, suggesting that **adaptive network selection** is crucial for optimizing both performance and energy efficiency.

---

## Key Findings

1. **5G Performance**:
    - **mmWave 5G** shows outstanding performance for **high-bandwidth** applications but suffers from **distance sensitivity** and **higher energy consumption**.
    - **Low-band 5G** offers more stable performance across longer ranges, making it more suitable for **mobile** and **outdoor** use cases.

2. **Power Consumption**:
    - **5G** is **less energy efficient** than **4G** at low throughput but becomes **significantly more efficient** for **high-throughput** applications.
    - **RRC_INACTIVE** in **SA 5G** helps mitigate some of the power costs, but **mmWave 5G** still demands much more energy during active transmissions.

3. **Video Streaming on 5G**:
    - **ABR algorithms** face significant challenges in 5G networks, particularly with **throughput prediction** and handling **variable bandwidth**.
    - **robustMPC** and **FastMPC** outperform other ABR algorithms in **QoE**, offering better video playback and fewer stalls.

4. **Web Browsing on 5G**:
    - **mmWave 5G** reduces **page load time** for large pages but at a higher energy cost, making **dynamic interface selection** between **4G** and **5G** critical for **energy efficiency**.
    - The proposed **dynamic interface selection scheme** achieves **26.9% energy savings** while maintaining optimal browsing performance.

---

## Conclusion

The paper concludes that **5G**, particularly **mmWave 5G**, provides significant performance improvements over 4G in high-throughput scenarios. However, this comes at a higher energy cost, especially for tasks like **web browsing** and **video streaming**. Dynamic strategies like **interface selection** and **throughput-aware ABR** algorithms are necessary to optimize the use of 5G in real-world applications. The paper provides extensive datasets and tools for further research in 5G power and performance optimization.

---

## Future Directions
- Further research is needed to **improve ABR algorithms** for **5G**, especially by incorporating better **throughput prediction models**.
- Developing more **energy-efficient interface selection schemes** that dynamically switch between **4G** and **5G** based on the task at hand will be crucial for optimizing **QoE** and **energy consumption**.

====================
## CLASS NOTES
====================

# Key Ideas:

- All the new things in 5G
- new radio states 
-deployment modes:
    - NSA(Not Stand-Alone): Relies on the existing 4G signal infrastructure, used as a **transition strategy, reusing prior control-plane support**
    - SA: Uses new 5G signalling
        - EXPECT TO be low latency as it has the new shiny stuff **but however paper measurements show its not always better** 

## Measurements:
- 2 carriers: Verizon and Tmobile
- Challenges in their measurement setup : 
    -different UEs(devices) have different hardware
    - they want to measure wireless changes, bu they take measurements to the internet 
        so problems in the internet can interfere with what they think wireless
    - complexity in carrier networks, all different carrier and proprietary data

# Distance vs Latency:
Verizon latency
- Long distance => higher RTT, linear curve becuse of the physics like speed of light or cables
- very similar but mmwave seems slightly lower in first graph

- NSA gets better latency even though it transitions


