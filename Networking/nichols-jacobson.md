===
Controlling Queuing Delay
===

Queues are there to absorb flows and do statistical muxing

AQM
FCFS was started initially, then stat mux came in and then buffering, so AQM

Key Ideas:
    - bufferbloat- always a queue in the router, lots of uneccessary delay, but we cant gaurantee everybody uses BBR
    - soujorn time to estimate how long packets are there in the queue
    - the idea of good queue and bad queue
        - good: resolves in 1 RTTs
        - bad: long term,never clearing queues 
    -router takes decision to drop packets early, partly this is to indicate users to slow down
    - propse CoDel, easy AQM method; improvement over RED


## RED drops randomly to ensure fairness and also to signal congestion as a lower bound to the users

 track minimum queue size- what is minimum queue size?
    -   look at minimum queues over last RTTs
Why?
    - helps us know the every RTT update and More IMP: helps us know if the queue cleared up sometime.
    - in the minimum it gives us a more granular view of the standing queues
    -nichol argues that we are comfortable to have big queues given that they drain: goo queues because thats a queue doing it s job of absorbing bursts, HOWEVER: AS OPPOSED TO whats seen in the minimum queue curve on the slide for MINIMUM vs MEAN queue length, where in the minimum queue length curve we see the standing queue,

# NICHOL's main arg: we need to recognize the role of queuing in a system and need a metric to highlight GOOD vs BAD queues, and not just is there a queue but DOes the queue drain.

- What is soujourn time?
    -   time when pkt enters and when it drains.
    - **Nichols said, for 20 years we used average queue length which was wrong, use MINIMUM queue length, it gives us the actual delay. Min sojourn time captures whether the queue drains**

    sojourn time vs target time to understand standing queues. but how do we know or set the target latency?
        - it is a parameter we can judge based on our physical configurations, have a bigger latency if the router is a 10GB or make it small when the router is 512kb

    other challenge is how to figyre out the interval, it is tricky, just use RTT, interval should be >= typical RTT

Does Codel work?
    - deployed in openwrt routers 
        
# Over Range of RTTs slide:
-   latency is fairly okay in bott CoDel and RED
-   But in case of link utilization, as the latency increases, RED's performance worsens
    -   this is because REDs dropping random packets and marking them, which tells TCP to hold back packets and control by a lot, and when TCP cuts sending rate, overall utilization
    -   However, CoDel has better performance as it does not send congestion signals as much(functions mostly on minimium queues over an RTT and as it track min queues it takes actions rarely when there is bad queues and sojourn time>target)
    -   
