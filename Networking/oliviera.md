# Key Ideas S1:
## AS topologies

mostly limitations but more about AS topology

SL Topology slide backup links are always there but not visible as we dont want to use them always, just use when our primary links fail

tier 1 AS have best coverage as they are the top, get all the routes, they do not have hidden links or invisible links, so global overview of the entire ntwk

tier 2: routers are not fully utilized, also there is a gap bw public view and the router config 

in tier 2 results, where do you see the hidden links:
    the gap bw third blue line and lowest line are hidden links which come up wrt time
        these get better with time
        over time the primary links fails(the 2nd black line) and then the 3rd blue line which are the hidden links come up and appear to converge over time with the primary links. (hidden links are basicaly BACKUP links)

    the gap bw the first and the second line shows invisible links
        cause of invisible links:
            no valley principle but ILs get better with more vantage points
CHECK diag: you know what it means(we discussed in class)

CONTENT PROVIDER:
IXP projection the red is the ground truth:
    the gap is due to the invisible link




# types of AS business relationships:
- Customer - Provider relationship
- Peer to Peer
- Sibling to Sibling
- other types of links:
    - Backup links which are also called hidden links and are seen only when primary paths fail 
    -Hidden Links:

   ## Definition: 
   - Hidden links are backup or secondary connections between ASes that are not typically active under normal conditions. These links become visible when primary connections fail or degrade, activating the hidden links as part of a failover mechanism.
    Characteristics:
        Often seen in Tier-2 ASes.
        Detected when the primary path is unavailable (e.g., during outages or failures).
        They serve as backup links that are crucial for maintaining resilience and redundancy in the network.
        Hidden links may not show up in public topology measurements because they are only used in certain failure scenarios.
        Over time, these hidden links may become more apparent as they activate in response to failures.
- PNI: direct link bw 2 ASes


# Invisible Links:

    Definition: Invisible links are connections between ASes that exist but are not visible due to routing policies (such as the no-valley principle) or because of the lack of sufficient vantage points in the measurement system.
    Characteristics:
        Invisible links often exist but are not announced or advertised due to routing policies that prevent their use in certain situations (e.g., avoiding suboptimal routing paths that violate the no-valley rule).
        These links may become visible when measured from more vantage points or through more sophisticated topology discovery methods.
        Invisible links are often observed as gaps between the expected and observed network structures.
        Improving the vantage point diversity can help detect these links, as they become visible when probed from different network locations.\



# Challenges in How to Measure the AS-level Topology?
- NO Valley routing which are due to provider customer services and lack of proper vantage points
- Hidden Links
- Invisible Links



# IN results, WHy do you think TIER 1 ASes have a good view of the overall network?
-   because they are at the top of the heirarchy, allor most of the traffic goes through them and we can see their BGP coverage. **And hence it has the BEST COVERAGE.**
-   *However. there is a delay of 100 days to converge on the truth because of HIDDEN LINKS, because these links only show up when something fails in the real world and hence it takes time to converge*.

# What about TIER 2 ASes in results?
-   there is a big gap bw most of the public views down and the groun truth that is observed by the router configs, mostly becasue the hidden links come up in sometime as the links start to fail.But eventually for all the public and lower plots the hidden links come up and we see they converge.

- Also the large gap which never closes bw routers and the others below is because of **INVISIBLE LINKS** which are not seen in granular depth as the lower tiers face *NO VALLEY* rule and hence never see the entire network. Whereas the router configs and syslogs have a more global view of whats going on.

# content provider result:
- this gap is all because of Invisible links, for hypergiant content providers as they peer in all places wherever possible so as to not pay and just transit for business and increase reachability.
    ========================= GPT INGESTED ========================


# Key Ideas: AS Topologies

## General Concepts

1. **AS Topologies**:
    - **Tier-1 AS**:
        - Have the best global coverage, getting all the routes.
        - Do not have hidden or invisible links, providing a global overview of the entire network.
    - **Tier-2 AS**:
        - Routers are not fully utilized.
        - A gap exists between public views and the router configurations.
        - Differences between observed connectivity and actual configurations.
    
2. **Backup Links**:
    - Backup links are always present but remain invisible under normal operation.
    - These links activate only when the primary links fail.
    - Backup links help improve network resilience and redundancy, essential during failures of primary paths.

## Hidden and Invisible Links

1. **Hidden Links**:
    - Found primarily in Tier-2 results.
    - **Where to identify hidden links**: 
        - The gap between the third blue line and the lowest line in graphical results indicates hidden links, which emerge over time.
        - When primary links fail (e.g., represented by the second black line), hidden links (third blue line) become active and converge with the primary links over time.
    - These links serve as **backup links**, and their activation is dynamic, occurring when needed.
    
2. **Invisible Links**:
    - The gap between the first and second lines on AS topology graphs indicates invisible links.
    - **Cause of invisible links**:
        - They arise due to the **no-valley principle**, which is a routing policy that prevents the propagation of certain routes.
        - Invisible links improve when more vantage points are considered, revealing connections that were previously undetected.

## Content Provider View

- In the IXP projection, the red line represents the **ground truth**.
    - The gap between the red line and other observed data suggests the existence of invisible links.

## Key Points from Oliveira Paper

**Title**: "In Search of the Elusive Ground Truth: The Internet's AS-level Connectivity Structure"

1. **Overview of the Paper**:
    - The paper discusses the challenges in identifying the **ground truth** of the Internet's AS-level topology due to incomplete data and the presence of hidden/invisible links.
    - It highlights the limitations of current measurement tools and techniques, which often provide only a partial view of the true AS-level structure.

2. **Causes of Hidden Links**:
    - Hidden links are backup paths that exist but are not always visible unless activated by specific conditions, such as the failure of a primary link.
    - The presence of hidden links contributes to the overall resilience of the Internet but complicates accurate AS-level mapping.

3. **Invisible Links**:
    - Invisible links are a result of routing policies such as the **no-valley principle**, which limits the paths that certain ASes are allowed to use or advertise.
    - These links become visible when enough diverse vantage points are used in measurements, suggesting that topology mapping is heavily dependent on the vantage point distribution.

4. **Measurement Techniques**:
    - The paper critiques current AS topology measurement techniques, including **traceroute-based mapping**, pointing out their inherent limitations in detecting hidden and invisible links.
    - It suggests that a combination of measurement tools and techniques is needed to approach the ground truth.

5. **The Role of IXPs**:
    - Internet Exchange Points (IXPs) play a crucial role in shaping AS-level connectivity, often housing direct interconnections between ASes that are not visible to traditional measurement tools.
    - These interconnections are frequently missed, contributing to the gap between the **ground truth** and observed topologies.

6. **Improving Visibility**:
    - The paper recommends using more diverse and distributed vantage points to improve the visibility of both hidden and invisible links.
    - Additionally, improved modeling of AS relationships and routing policies can help close the gap between observed data and the actual AS topology.

7. **Conclusion**:
    - Accurately mapping the AS-level connectivity of the Internet remains a challenge due to hidden and invisible links.
    - The **ground truth** of the Internet’s AS-level structure is elusive, but with better tools, vantage points, and methodologies, the gap can be narrowed.


