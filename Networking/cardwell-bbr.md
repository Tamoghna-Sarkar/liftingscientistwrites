# Previous TCP:
- did estimate BtlBW too" was more implicit in the design - > cwnd/RTT

# But in BBR:
-   It's more explicitly modeled to find BttlBw
- periodically send more traffic to see if more capacity is available

2 things needed to be modeled:
- RTT + n (n is the queuing delay)