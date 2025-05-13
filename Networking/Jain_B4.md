# B4: GOOGLES SDN AND TE WAN
traditional WAN are overprovisioned that are underutilised.
    -   want to avoid loss(congestion)
    -   need to handle link failures

# solutions:
• new SDN-based architecture (using OpenFlow) to manage their WAN
• do Traffic Engineering (TE): will steer traffic on certain paths to get high utilization
• with a centralized controller (it’s replicated)


## Overview

The article discusses Google's efforts to optimize Wide Area Networks (WANs) using SDN and Traffic Engineering (TE). Traditional WANs are often overprovisioned and underutilized to avoid congestion and handle failures. Google’s approach leverages centralized control and prioritization to achieve higher utilization and reliability.

## Problems with Traditional WANs

- WANs are **overprovisioned** and operate at only 20-30% capacity.
  - Goal: Avoid loss (congestion).
  - Goal: Handle link failures reliably.
- Networks handle:
  - **Large Data Transfers** (e.g., backups, replication).
  - **High-Priority User Traffic** (e.g., search or video streaming).

## Solutions Introduced by Google

### 1. Software-Defined Networking (SDN)
- Google implements SDN using **OpenFlow** to centralize control over their network.

### 2. Traffic Engineering (TE)
- Dynamically steers traffic along specific paths to maximize utilization.

### 3. Centralized Controller
- A replicated controller manages real-time traffic decisions, ensuring reliability and scalability.

## Why Google’s Approach Works

### 1. Complete Network Control
- Google owns and manages every aspect of their network, including:
  - **Traffic Generation:** Controlling what traffic enters their network.
  - **Routers and Backbone:** Ensuring optimal operation without external dependencies.

### 2. Prioritization of Traffic Types
- **Latency-Insensitive Traffic** (e.g., backups) is deprioritized.
- **Latency-Sensitive Traffic** (e.g., search, video) is prioritized for real-time responsiveness.

### 3. Friendly Environment
- No external entities disrupt network behavior, allowing full optimization.

## How It All Ties Together

- **Objective:** Address inefficiencies of traditional WANs by improving utilization and reliability.
- **Key Strategy:**
  - Use SDN and TE to dynamically allocate resources and route traffic efficiently.
- **Results:**
  - Achieved higher utilization.
  - Reduced operational costs associated with overprovisioning.
  - Enhanced reliability for both latency-sensitive and bulk data traffic.

---

*Google’s holistic control of their ecosystem enabled them to tailor WAN management with cutting-edge solutions, setting a precedent for SDN-based traffic engineering.*

# Great WAN utilization:
- they have a bunch of low priority and also high priority.

- 10% is high priority and about 90% low priority traffic

- they drop a lot of low priority traffic, a good amount. They are willing to do this as it is low priority and will recover

- high priority traffic loss is 0(no loss). HP traffic always pushes the LP out of the way. 

- the aggregate utilization is about 100%
