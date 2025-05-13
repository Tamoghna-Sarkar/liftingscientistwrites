# keeping only stuff that we didnt know during lectures, revise entire from ppt

# Policy Language: FSL, name-based policies like a department wise("sales department")


## what are Enterprise problems?
• malware
• copyright violation and
subpoenas
• “security”

## Ethane goals(JH version:)
To have a system that lets an enterprise manage its traffic at finer granularity, flow-by-flow, and manage it not by just ips but also tie them to human users and track them throughout, whenever the flow starts, intercept and take them to a centralized controller and they can take actions based off of policies.

# Ethane Components
• controller:
    – central box
    – knows all the policies
    – programs the (dumb) switches
• switches
    – forward packets for new flows to the controller
    – for existing flows: we expect the switch to have a rule in place (send where or drop)
        • controller set up rules for each flow
• admins set the policies (in the control)
    – in a new langage FSL
• *IMPORTANT* **authentication of users and devices**
• flows

# Per-Flow Processing
• Can handle tens of thousands of flows arriving per millsecs. shouldnt be an issue as everybody is not clicking or doing something every second/millisec or something 

# State Reliability in Ethane:
• Ethan does have a bunch of state, that is criticial to its working and it has to recover in order for it to work again. The most general approach is to *REDO* it just like we did in mapreduce. 

---