===
XCP running stuff
===

- lets not send a ecn bit and leave it to the users to figure out how much to send, instead the routers have a global view, so let the router send feedback about how much to send exactly. 

- separate fairness from congestion control

- and do this without per-flow state in the router

# information in the paper:
- cwnd
- rtt
- feedback

- each router in the way might say, we can go faster than what we are going now. We are going at 1mbps, router looks at current speed and sees over the last time period if it had any additional capacity and thus adjusts the feedback about explicitly how much rate the sender can send at.
- this information goes through each router and each router can have their say

# What did tcp do?
- it doesnt say its rate
- in routers, either they forward the traffic or they drop it

# router feedback utilization:
- keep utilization high
- keep queues small
- do w/o per flow state

## use a feedback function ϕ:
-  it computes ϕ based on how busy it is and therefore over the  capacity is available and gives feedback accordingly, if available positive feedback, if the network is above capacity then provide -ive feedback
- it also has something to see **STANDING QUEUES**  (something like CoDel).

### key things in ϕ formula:
- **S** : spare bw
- **Q** : Mean queue size/ queuing delay
- **d** : mean RTT 

# Feedback has more components than TCP and hence can say exactly in feedback about what rate to send in.

# Algorithm:
If:
    we have extra capacity, lets increase everyone equally, AIMD : to **achieve fairness**
elif:
    we have are over capacity, proportionally decrease, MIMD
else:
    shuffle stuff, helps fairness, but why fairness improves using shuffling?
        - we can be fully utilized but UNFAIR to users
        - shuffling is to mix things around to avoid stopping in a steady state
        - but the shuffle is a small amount such that we dont have too many oscillations

# Why not more info before?
- we did not give more feedback earlier because routers were slow earlier and reqd more overheads, but XCP sends more information.

**If we had per-flow state, we could penalize every state according to their parameters.**

# IDEA W/O PER-FLOW STATE:
- The pkt header tells you the information you need to know.
- the router has these constants Tp, Tn etc which it uses to scale the feedback it gives.
- **penalize relative to rate from the last interval, estimate the number of packets that was sent and their RTTS**
**putting it together slide**: they normalize and scale over all RTTs and packet sizes
- **-ive feedback** : more feedback to longer RTTs and bigger pkt sizes and less feedback to shorter RTTs and smaller pkt sizes
- **+ive feedback**: the only difference is (rtt)^2

# **ENDING: THE MAGIC THAT MAKES XCP WORK ARE THESE WEIRD CONSTANTS, AND THE COMBINATION OF THE RIGHT FEEDBACK INFORMATION**

**HIGH UTILISATION, MINIMAL QUEUING, LOW LATENCY, SEPERATES CONGESTION CONTROL AND FAIRNESS CONTROL**


# How XCP Separates Congestion Control and Fairness Control

In **XCP (eXplicit Control Protocol)**, **congestion control** and **fairness control** are separated to allow more efficient and fair use of network resources. This separation is one of XCP's core innovations, allowing it to manage network utilization and bandwidth allocation more flexibly than traditional TCP. Here’s how XCP achieves this:

## 1. Congestion Control: Managed by the **Congestion Controller**
- **Purpose**: The congestion controller in XCP adjusts the aggregate traffic rate across the network to match the available link capacity and drain any excess queue at each router.
- **Mechanism**: Each router calculates the **total feedback** for all flows traversing the link, which is based on two factors:
  - **Spare Bandwidth**: The difference between the link’s capacity and the current traffic rate.
  - **Queue Size**: If there is a queue at the router, the congestion controller will reduce the total traffic rate to clear the backlog.
- **Formula**: The feedback provided by the congestion controller is calculated as:
  
  \[
  \Delta = \alpha \cdot d_{avg} \cdot \text{Spare} - \beta \cdot \text{Queue}
  \]
  
  Where:
  - **α** and **β** are constants.
  - **d_avg** is the average delay (round-trip time).

  This aggregate feedback, **Δ**, is a single, unified rate adjustment for all flows passing through the router, aimed at ensuring efficient use of the link capacity without causing excessive queuing or packet loss.

## 2. Fairness Control: Managed by the **Fairness Controller**
- **Purpose**: The fairness controller ensures that each flow receives a fair share of the available bandwidth. This addresses the issue of certain flows potentially monopolizing network resources at the expense of others.
- **Mechanism**: After the congestion controller has determined the aggregate feedback **Δ**, the fairness controller **distributes this feedback among individual flows**. The distribution depends on whether **Δ** is positive (indicating spare capacity) or negative (indicating congestion):
  - **If Δ > 0 (Spare Capacity)**: The positive feedback (allowing more traffic) is **divided equally** among the flows to prevent any single flow from disproportionately benefiting.
  - **If Δ < 0 (Congestion)**: The negative feedback (requiring a rate decrease) is allocated **proportionally** based on each flow’s current rate, meaning larger flows reduce their rate more significantly, while smaller flows reduce their rate less.

  This proportional allocation maintains **fairness across flows** since the fairness controller avoids penalizing smaller flows excessively.

## Why This Separation is Important
- **Flexibility and Stability**: By separating congestion control and fairness control, XCP can handle network conditions more flexibly, allowing it to achieve high utilization without causing excessive queuing. Each controller operates with distinct objectives, reducing the risk of conflicts that can arise when one mechanism tries to manage both utilization and fairness.
- **Efficient Resource Allocation**: The congestion controller maximizes the use of link capacity, while the fairness controller ensures equitable bandwidth distribution, even in the presence of flows with varying characteristics (e.g., different RTTs or data rates).
- **Analytical Tractability**: This separation allows for a more mathematically tractable approach, simplifying the design and tuning of each control mechanism independently.

In summary, XCP’s **separate congestion and fairness controllers** work in tandem to achieve efficient network utilization and fair bandwidth allocation across flows, a design choice that enhances both performance and fairness in high bandwidth-delay networks.
