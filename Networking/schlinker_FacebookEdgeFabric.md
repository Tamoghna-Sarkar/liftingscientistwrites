# FOR REVISION, LISTEN TO LECTURE FOR THE RESULTS SLIDE(some doubts)

# **Edge Fabric Capacity-Aware Routing** Goals:

-   find overloaded peerings/paths and shift the some of that traffic load somewhere else
BECAUSE overloading is a problem.

## • input:

– routing (from all Peer Routers)
    • includes all the possible paths

– per-prefix load (from all PRs)

– capacity of peer links (static)

– (all for one PoP)

## algorithm:

– look at each peering,
    check if it’s overloaded,

if so:
    compute an alternative path

## • output:

– set of override BGP routes
    • split prefixes if necessary, so each bundles is about 250Gb/s

– injected with local-pref to take priority

----------------------

# **Edge Fabric Performance-Aware Routing GOALS**

1) – want to find lower-latency paths

2) – lower latency improve QoE in videos

## • input:

- performance info about paths from app-level measurements
- passive measurement of the network
using operational data

### Above: in general: they pick out 1% of the traffic to their customers randomly and send that to an alternate path. They are willing to let some customer traffic to not do the best things and send the best to others, who cares if somebody takes a bit of more time to see an image in the newsfeed..tolerable loss.

## algorithm:

– want to route customers on the alternate path

– tunnel the traffic out an alternate path

## output:

– alternate routing tables to divert traffic

# Results:
WHat do we see after they turn on load balancing?

- rather than all links be the same, some links have high utilizations and some have low. FB has 15% links that are overloaded. 

even though some people are hurt the overall performance improved

# **visit lecture slides**