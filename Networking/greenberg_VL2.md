Key:
- how to build data centers for a cloud provider

**clos network stuff in data center, Valiant Load Balancing, ECMP**: *Read from GPT chat*


# traffic analysis

time on the x-axis, index of the containing cluster in the y-axis, but what does this y-axis thing mean?**each index means a unique pattern**

traffic patterns changes and never constant and hence cannot even optimise anything.

run length of how often they capture the same things in a row and time to repeat(in log scale).
run length is super short: 1 or 2 of the same pattern and then its gone
Even log scale, stuff doesn't repeat very well

## the implication of this is:
- we cannot predict what is gonna happen, everything is random and have to deal with worst case all the time.

### why is the datacenter traffic pattern so random?
OR 
### another way to think, why would Mapreduce  result in random traffic patterns?
-  MR takes big job, split jobs gives to different workers, then shuffling and sorting and send data around, and all the data in the last phase has to go to the same place, "hot keys occur many times" but why? thats just nature of human language and its pretty random.
- its not an accident that we dont have regularity in data centers but its just like that, **its a feature and not a drawback**


# Goals of VL2

• agile:
    – what? any users VM can run anywhere
    – why? so they can get high utilization on their data center (they don’t want users to need specific machines, or specific sets of machines)

• performance isolation (CPU and disk):
    – what? no tenant should interfere with any others wrt CPU, networking, memory
    – why? for security (you can’t access my stuff), we have an SLA (Service Level Agreement) as part of the abstraction when we sell VMs

• uniform high capacity (in communications):
    – what? full bisection bandwidth --- any VM can send at its interface bitrate to any other computer
    – why? it’s one kind of performance isolation

• flat addressing (layer-2 semantics):
    – what? all my VMs look like they’re on my own LAN, with IP addresses that don’t change (address isolation)
    – why? easy for tenants to understand (clean abstraction), and allows cloud operators to move stuff around, and we want to small switch tables

• cost effective

# Approaches in VL2 (how?)

• VLB (Valiant Load Balancing) and ECMP (Equal Cost MultiPath Routing)
    – high utilization through the core network, even if some routers fail

• Clos topology: gives uniform high banwidth or **bisection bw** 

• application addressing and location address

