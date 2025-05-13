# routing might get affected due to excessive congestion in the network as in IP routing and traffic data flow through the same channel causing interference.
    -   shows that routing can fail if there is too much traffic

# experiment:
- the above line is the good traffic
-they also do a 2node vs 3node thing noise traffic, using different paths and the result is it affects the recivery time.
- whats important is after the link fails, IS ALL THE TRAFFIC DUMPED?because the good traffic, it was going over the same link that failed or does it get diverted through the other link in which case the buffer has to drain?

## measurements:
- u2d and d2u: router failures and recovery times, they look at this with 2 protocols: OSPF and BGP at varying capacity(125-500% load)

# solving the ospf markov model:
- its a log-log plot, the plots seem linear but all of them are exponential decays(seen linear as its plotted on log-log plot)
-  the expected time to failure/**survival time** decreases exponentially or multiplicatively, as p increases/failure prob increases

# u2d OSPF vs BGP:
-  setting up ospf is easy as its over udp and its markov model is easy
- setting up bgp is difficult and also complex as it relies on tcp and tcp does its best to keep retxs to not loose connections and keepalives, also its complex for tcp as they need 3 way handshakes, and a bunch of initial setup mechanisms, after doing this they have to send keepalive. and data transmission.

- tcp will be a lot slower than UDP while setting up.

# LOOK AT RESULTS PLOT IN THE SLIDES