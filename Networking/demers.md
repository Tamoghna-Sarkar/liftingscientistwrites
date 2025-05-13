===
FAIR QUEUING: class notes
===

# Key ideas
 - end users can get First come first serve access to get more than their fair share
 - Demer's fix: FAIR QUEUING
    - emulate round robin bit by bit
        -because its fair
        - THE IMPORTANT THING HERE IS THE WAY THEY MODEL THE BIT-BY-BIT SYSTEM
 - Bidding: "thank you for not hogging the entire network and helping us in statistical muxin"
    - and hence we let you bid and after bidding you can get ahead in the network to get more high rate flows
- **MOST IMPORTANT IN THE PAPER IS ALSO THAT THEY DEFINE MAX-MIN FAIRNESS**

# Fair Queuing:
    - Definition: MAX-MIN
    -Why is fairness hard?
        - routers dont have a complete view
        - also senders are distributed, different RTT and they react differently

## Max-Min: Divide the available capacity equally at the start and then whatever is left, give it to the ones that demand more ##

# What about bad users and what are they going to do?
    -they can ask for more and say they want 1, but we give them whatever is fair.

# What about request?
    -go through slide, not that important

## Round in Fair Queuing:
    -round
    -NAC: number of active flows
    -dR(t)/dt: 1/N active flows
    start and finish times: S_i, F_i: i is the packet 
    etc
   **follow the above from lecture and slide**

- Bitwise FQ might look fair but actually not fair as we do not route packets bitwise here and there, networks dont work that way.
    -   Hence, we work with S_a, and F_a(start time and finish time)
    - when packet arrives we start sending the pkt, and then pkt B arrives,  we have to accomodate B after a As far as it gets serviced before its end time Fb.
    - Also packets might have different size, so its not fair if D being small has to wait all the way. Weare trying to accomodate different sizes of packets.
    -   so send packet whenever they come, BUT DOES IT WORK?
        - Its not fair, as using round robin, packet sizes are different and take different amount of times, and hence might violate end times.
        - In order to avoid that, we have schedule packets accordingly after re-ordering. (SOME WHAT LIKE HEFT)
**THIS IS WHAT DEMERS ROUND ROBIN FOR QUEUING DEALS WITH, IT DEFINES THE THEORITICAL TIME SYNC AND WHEN YOU NEED TO REORDER THE PACKETS AND NOT DO STRICTLY FIRST-COME-FIRST-SERVE.**

# THIS ABOVE IS WHERE FQ DOESN'T WORK
PREMEPTING SOME PACKETS OR HAVE TO INTERRUPT PACKETS IF SOME EARLIER PACKET COMES LATER AND THE LATER PACKET HAS ALREADY BEEN STARTED TO BE PROCESSED, IN THIS CASE WE WILL INTERRUPT THE PROCESS AND RESEND ACCORDINGLY.


## BUNCH OF SIMULATION RESULTS
    - simple topologies
    - combination of queuing techniques etc


# BIG CHALLENEG WITH FAIR QUEUING:
- keeping track of state and real routers cant keep state for all the flows,
    - however, the basic idea now is push the state to the **EDGE**

    