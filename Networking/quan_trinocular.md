===
CLASS RUNNING JOTS(TAKE NOTE FROM RECORDED AGAIN, LOST W/O COMMIT- recovered but some Imp stuff at the end wrt results, SO visit LECTURE)
===

# BASIC IDEA: if you ping all the addresses then you know some blocks are active, some arent. Who replies and who doesnt is relative to time, things change and services suspended, or something happened?

# how to minimize probing traffic:
    probe one by one
    find one is up
    distribute the load
    if we probe and not hear back, try again. not hearing back is a soft sign of block down but not strictly says down, so try again
    multiple fails lead to block down.

## need to reduce background radiation:
- tries to send minimal traffic, because in the internet even if you dont participate in a anything, you have random things sent to you which hog up sometimes, so trinocular wants to reduce that.


 # Belief up vs probe diag: bayesian inference algo
    some times there are -ive replies in the positive 1 portion too, but after a certain tries they hear back, so block is not down which could have been thought initially. After big # of fails, we go down to zero. even while transitions from 0 to 1, there were a few +ive spikes and could have been thought block might be up, but followed by -ive answer, which suggests not all adresses in the block might be up..next we go up after 2-3 +ive replies and go back to 1


 # WHy do we need to observe from different locations?
    if there is a failure near 1 Vantage Points(VP), its the problem of the VP and not the internets, diagram shows in the slide all the VPs from multiple locations, and look from different perspectives that all of them converge after a point of time.

    Slide 41:
    first diagram:v
    second diagram: diurnal: changing everyday, 14 bands(maybe people are sleeping the other parts and hence off)
    third diagram: ISPs giving out addresses dynamically, people coming and people go. Hence, very random.


    Slide 43:Precise: Detect all outages?
    where is it wrong?
        overestimates in a lot of cases because I am probing every 11 mins and the outage happens maybe shorter than that. so we miss short outages.
        the chance of detecting the outage is pretty random, if we happen to probe at the right time when the outage is going on, only then we are detecting correctly else missed. We have to be lucky enough to hit the probe at the same time in order to be able to detect the outage.

        Adaptive probing helps?
            usually it helps us reach an inference but sometimes it doesnt. 


    Sl44: parsimonious probing rate
why do we need 1 probe?
- outages are rare but not the best reason
- the one probe gave us a positive reply, and thats what we need to confirm if the block is down, 
    check lecture:
    yes we are happy, as they send min number of pkts to the internet[bkround radiation]

    A[E(b)] = historical Pr[any address in b replies]
    E(b) = ever active addresses

Does this makes us a happy?
yes, reduces bckground traffic

A small A means a lot of black, which means a lot of non-responsive addresses. [check lecture]

# bottomline of the last result **Why Parsimonious**:
**by learning the "a" value and learning about history, in a lot of cases we can correctly detect outages with very little probes**
**if the availability of responsive addresses are less, then it takes a lot of probes to detect outages** 


====
GPT INGESTED TRANSCRIPT FROM A PPT
====


# Trinocular: Understanding Internet Reliability Through Adaptive Probing

## Authors:
- Lin Quan
- John Heidemann
- Yuri Pradkin
- USC/Information Sciences Institute
- SIGCOMM, 14 Aug 2013, Hong Kong, China

---

## Why Study Internet Outages?
- Examples of Major Outages:
    - Hurricane Sandy (Oct 2012)
    - Egypt Internet Shutdown (Jan 2012)
    - Japan Earthquake (Mar 2011)
    - Syria Internet Shutdown (May 2013)
- Causes of Outages:
    - Link failures
    - Natural disasters
    - Political events
    - Network infrastructure issues
    - Home router failures

---

## Our Goal: Track All Outages
- Track all Internet outages continuously, for all ping-responsive IPv4 /24 blocks (3.4M in total).
- Examples: 
    - Egypt Internet shutdown, Jan 2012
    - Hurricane Sandy, Oct 2012
    - Japan Earthquake, Mar 2011
    - Syria Internet shutdown, May 2013
    - Network infrastructure issues

---

## Why Study Internet Reliability?
- **For Internet Users**: 
    - Am I getting the reliability I pay for?
    - Which ISP is most reliable?
- **For Network Operators**: 
    - Is my ISP reliable compared to others?
- **For Researchers**: 
    - Is Internet health improving? 
    - Do correlated events affect multiple ISPs?

---

## How Is Our Goal Different?
- Tracking all Internet outages for all IPv4 /24 blocks, all the time, with precision.
- Prior work is less complete or precise:
    - **[Hubble, NSDI'08]** and **[iPlane, OSDI'06]** focus on BGP prefixes.
    - **[Thunderping, IMC'11]** studies a small sample on demand.
    - **[Dainotti et al., IMC'11]** relies on passive analysis.

---

## Why Is Outage Detection Hard?
- **Precision**: Frequent probing is required for accuracy.
- **Accuracy**: Need to probe many addresses per block.
- **Full Coverage**: Must probe millions of blocks.
- **Challenges**:
    - CPU constraints: CPU tops out at 30k probes/s per core.
    - Traffic on target blocks: Aim to stay under 1% of background radiation.
    - Prober ISP goodwill: Avoid complaints by limiting traffic.

---

## Trinocular Approach
- **Trinocular**: Active ping probes to study reliability at the Internet edge.
    - Probes only when needed (informed by Bayesian inference).
    - Models who will reply and how likely.
    - Outage duration precision: ±330s.
    - Adds only 0.7% to background radiation.
    
---

## Pings Tell You Something, But Not Everything
- **Positive reply**: Block is up.
- **Negative reply**: Ambiguous; could mean the block is down, computer crashed, address was reassigned, or firewall is enabled.

---

## Probing Whole Blocks Resolves Ambiguity
- Probing whole blocks provides more certainty but also generates more traffic, potentially leading to complaints.

---

## Adaptive Probing with Bayesian Inference
- Probing whole blocks is wasteful; adaptive probing is more efficient.
- Probes only when needed and stops when sufficient information is collected.
    - Example: Probe one by one until a positive response, then stop early.
    - If several attempts fail, the block is down.

---

## Ever-active Blocks: Who Will Respond?
- E(b) is who will respond, derived from 3 years of censuses.
    - **Sparse**: Addresses that respond intermittently.
    - **Medium**: Moderate response frequency.
    - **Dense**: High response rate.

---

## Bayesian Inference: Probing Just Enough
- Bayesian belief (B) of block state evolves over time with each positive or negative response.
    - **Down** if B(U) < 0.1.
    - **Up** if B(U) > 0.9.
    - If unsure, declare unknown and probe more.

---

## Trinocular Probing Types
- **Periodic Probing**: Guarantees freshness (every 11 minutes).
- **Adaptive Probing**: Ensures accuracy (3s intervals to resolve uncertainty).
- **Recovery Probing**: Handles down-to-up transitions.

---

## Trinocular Instances and Outage Scope
- Multiple Trinocular instances can differentiate local outages from global outages.
    - **Local Outage**: Detected by some instances, but not all.
    - **Global Outage**: Detected by all instances.

---

## Validation: Does Trinocular Work?
- Detects all outages longer than 11 minutes (probing interval).
- Adds less than 1% to background radiation (20-33 packets/hour).

---

## Probing Granularity: What Size Blocks to Probe?
- Routable prefixes versus /24 blocks:
    - **Routable prefixes**: Larger granularity but less precise.
    - **/24 blocks**: More granular, more accurate, but more work (8x more work than prefixes).

---

## Coverage: What is Theoretically Possible?
- IPv4 address space shown on Hilbert curve:
    - Green: Routed.
    - Black: Not routed.
    - Active probing requires responding targets.

---

## Coverage: What is Actually Possible?
- **Prefix-based vs. /24 block-based** coverage:
    - Prefix-based approaches infer many blocks, but at the cost of precision.
    - /24 block-based approaches cover more blocks directly.

---

## Granularity vs. Precision
- **Coarser granularity (prefix-based)**: Misses 37% of outages and overcounts outages by 25%.
- **/24 block-based approach**: More accurate but requires more resources.

---

## Observations about the Internet
- **Local vs. Global Outages**: Some affect all, estimate 15% are global, and many are local.
- **Hurricane Sandy**:
    - Outage rate tripled to 1.2% post-Sandy.
    - Gradual return to baseline after four days.

---

## Conclusion
- Trinocular can measure outages for the entire Internet edge with known precision.
- A single prober adds less than 1% of background radiation.
- Data available at: [http://www.isi.edu/ant/traces/internet_outages](http://www.isi.edu/ant/traces/internet_outages)


