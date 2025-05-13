===
class notes
===

# new protocol:
    - no IP addresses, **instead use attributes(named data)**
    - not e2e, just with neighbors
    - using interest to discover where to send
        - gradients indicate where to send 
    -for sensors
        -cheap and can be deployed in many places
            - numerous sensors helps accuracy and precision of the data being picked up 
    - how do communicate with many devices
    - they do processing in the network (IN-NETWORK-PROCESSING)
        -want to save energy in sensor networks

# **THIS PROTOCOL IS NOT IP BASED**

directed diffusion?
– flood/mulicast queries

**sensors’s first data is exploratory (low-rate data) as this explores all reverse paths and as part of this you establish gradients which is state of the network and the reverse path and then querier reinforces an optimum path to get to his desired answer**


# How to Find Stuff in a Sensornet:
in Multi-hop, Ad Hoc, Wireless Net
• what is different about finding things with directed diffusion?
– querier floods an “interest” in something identified by attributes
– sensors that match respond with some data
– reinforce the low-delay path
• in what ways is this search? in what ways is it routing?
– diffusion is both search AND routing
• why be this decentralized? why not just send to a server?
– too expensive (in battery drain!) to preemptively send everything to a central size



# INTEREST PROPAGATION:
- 1) The querier floods the network with interests, in the process as that packet is flooded everywhere we learn the lowest latency path/route, thats the interest going out,
- 2) Exploratory Data Propagation adn Gradient Establishment: the responsive data comes back and in the paper they talk about Low Rate Data.
    - i) first response coming back from the sensor as exploring all the reverse paths(hence they also call it exploratory data)
    - ii) as part of this you establish gradients which is state in the network about all these paths
- 3) Final step- REINFORCE: no need to flood everything in both directions as it is very inefficient, the querier REINFORCES a path and then for subsequent data you only send high bit rate data on that one low latency path. 

# Compare to BGP which does routing in the internet
-   smaller scale: only OUR sensornet, and the path is only from sensor to use
-   BGP is the whole internet
        – diffusion is more dynamic (nodes are small,
-   maybe mobile, maybe battery), BGP is much more static
- **IMP**  diffusion is done on-demand (when you have a query) however, BGP is done ahead oftem.

- BGP set of policies to route and DD policy system itself is different.

# how does the querier pick the reinforced path?
    - querier picks **the next hop**
    - based on low delay, regardless th number of hops
    - number of localized decisions, each node receives a sensor feedback and choses the lowest delay: in the sense whichever reply comes first to that node
    - IN CLASS I ASKED, SO CHECK TO CLARIFY, oCT 16, in first 7 mins

# how do we get multiple path?
    - mobility of nodes
    - something changes 
    - and hence negative reinforcement to nullify if we get multiple path

## whats expensive?
    - energy and latency
    - refer slides

# Other than flooding?
    - directional propagation using location/geography
    - replay some data from cache(if it's fresh)
    - reinforcing single path avoids flooding everything all the time
-  **this paper emphasizes flooding to discover optimal paths but also explores positive resinforcements and other methods  they consider to avoid flooding**
- positive reinforcements help in single path and not flood

# Naming and Attributes(How?)
- attribute based naming; key: value pairs
    vs what we do in the Internet: IP, dns and URLs over them, search engines
- **Diffusion always does MULTICAST**
    
    ## Why?where do we want to do less IDs?
        - sensornets assume things are very dynamic: sensors or querier are moving, or sensors come and go, they are not fixed, also querier do not know whom to send the query, its dynamic.
        - attributes help us optimize and avoid flooding
            - we want to expose application0level ingo(location, sensor-type,etc) to the networks, so it can optimize.
    ## Why make this integration in the sensornets?
        - we care about data at some location and not specifically the identity
        - IT DOES NOT HAVE AS MANY APPLICATIONS AS THE INTERNET, JUST HAS LIMITED APPS

    ## Filters are used to suppress duplicate data using in-network proceesing {check the slides}
     ### Compare to the Internet: Firewalls and BGP routes accumulated through the internet
        - internet doesn't really do in-network processing
            - although recent work in 5G edge computing

# How can diffusion do better than omnicient multicast?
    - Diffusion does in-network processing and hence it lets send less data
    - because it sometimes notices two sensors see the same thing and choose both

# Radio energy model:
    - TDMA-MAc has low energy while idle but 802.11 radios are always listening for applications
    - the graph is almost the same, just diffusion a little bit worse, but almsot same.

Different types of USC developed TDMA-MAC sleeping kind of protocols

===
