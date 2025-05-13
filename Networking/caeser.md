  - Private Network Interconnect (PNI) are basically ASes peering bw themselves. JHs comment was that PNI were respnosible for flatenning the internet, lots of direct connection to save money


# BGP messages: OPEN, UPDATE, NOTIFICATION, KEEPALIVE

- UPDATE: announces address prefixes, next hop router's IP address, some policies like LOCALPREF, announce your routes and also routes from and to your other customers

BGP selection rules: 
- Highest LocalPref
- Lowest ASPath
- Lowest MED
- lowest IGP id

# whats the point AS-PATH?
-  prefer shortest AS path, may or may not be since BGP does not consider performance, 
-  and also to avoud loop suppression. 

# Using BGP to implement policies:
-  ASPath prepending where necessary for **TRAFFIC ENGINEERING**


# LocalPref: 
- Only by Local AS. · E.g. The value of LocalPref is set by AS itself by the order Customer > Peer > Provider


# What types of traffic engineering do ISPs perform and how?
· Answer:
1. Outbound traffic control (by changing LocalPref and IGP costs)
2. Inbound traffic control (by AS prepending and MED)
3. Remote control (by changing community attributes)
