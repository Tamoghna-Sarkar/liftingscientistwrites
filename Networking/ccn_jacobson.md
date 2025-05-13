# Networking Named Content: Detailed Notes

## 1. Introduction
- **Problem with IP-based Networks**:
  - Modern network use revolves around retrieving content, but IP networks are designed for host-to-host communication.
  - Issues with current IP-based networking:
    - **Availability**: Mechanisms like CDNs and P2P networks are used to address issues, but they introduce complexity.
    - **Security**: Reliance on host location and connection integrity compromises trust.
    - **Location-dependence**: Tying content to physical host locations complicates network configuration and usability.

- **Content-Centric Networking (CCN)**:
  - Replaces the host-based model with a content-based model.
  - **Named Data** becomes the focal point, decoupling content retrieval from specific hosts or locations.
  - Provides inherent scalability, security, and performance improvements.

---

## 2. CCN Node Model
- **Core Packets**:
  1. **Interest Packets**: Requests for content.
     - Includes:
       - `ContentName`: Name of the desired content.
       - `Selector`: Additional preferences like publisher or data scope.
       - `Nonce`: Unique identifier to avoid duplicate Interests.
  2. **Data Packets**: Responses containing the requested content.
     - Includes:
       - `ContentName`: Matching name of the requested content.
       - `Signature`: Cryptographic hash for data integrity and authenticity.
       - `Signed Info`: Metadata about the publisher and versioning.
       - `Data`: The actual content.

- **Processing Workflow**:
  - **Interest Packets**:
    - Check the **ContentStore** (cache) for matching data.
    - If not found, check the **PIT (Pending Interest Table)** to aggregate similar requests.
    - Forward the Interest using the **FIB (Forwarding Information Base)** if no cache or PIT entry exists.
  - **Data Packets**:
    - Follow the trail of breadcrumbs left in the PIT to deliver the content to requesters.
    - Cache the content at intermediate nodes for future reuse.

- **Caching**:
  - Content is cached opportunistically in the **ContentStore** of intermediate nodes.
  - Replacement policies like **Least Recently Used (LRU)** manage cache capacity.

---

## 3. Transport and Routing
- **Transport Model**:
  - CCN operates over unreliable networks but ensures reliability via retransmission of unsatisfied Interests.
  - Flow balance is maintained: One **Interest** retrieves one **Data** packet.

- **Routing**:
  - Leverages hierarchical naming for efficient routing.
  - Dynamic path selection enables resilience and flexibility in forwarding Interests.

- **Mobility Support**:
  - CCN does not bind data to specific node addresses, allowing seamless operation across multiple interfaces and dynamic environments.

---

## 4. Security and Policy Controls
### 4.1 Content-Based Security
- **Core Idea**:
  - Trust is embedded in the content itself, decoupling it from network paths or hosts.
  - Data packets include cryptographic signatures for integrity and provenance.
- **Validation**:
  - Consumers validate:
    - **Integrity**: Content completeness.
    - **Provenance**: The source of the content.
    - **Pertinence**: Whether the content satisfies the Interest.

### 4.2 Managing Trust
- Flexible trust models:
  - Supports hierarchical PKI and non-hierarchical models like SDSI.
  - Namespaces can be cryptographically linked to publishers for validation (e.g., `/parc.com/george`).

### 4.3 Content Protection
- Content is encrypted and decrypted only by authorized users.
- Decryption keys are distributed as **ContentObjects**.

### 4.4 DoS Protection
- CCN mitigates DDoS attacks by:
  - Aggregating duplicate Interests to prevent flooding.
  - Using caching to reduce load on upstream links.
  - Limiting suspicious traffic at routers.

### 4.5 Policy Controls
- Organizations can enforce routing and access policies based on content names.
  - Example: `/parc.com/public` content can be restricted to specific users.

---

## 5. Evaluation
### 5.1 Bulk Data Transfer Performance
- CCN achieves throughput comparable to TCP but has slightly lower efficiency due to larger headers and unoptimized implementation.
- Proper pipelining improves CCN’s performance, matching TCP over time.

### 5.2 Content Sharing Efficiency
- CCN excels in multi-client scenarios:
  - Multiple clients pulling the same content benefit from **Interest aggregation** and in-network caching.
  - Total transfer time remains nearly constant regardless of the number of clients.

### 5.3 Voice-over-CCN (VoCCN) and Strategy Layer
- Demonstrated CCN’s dynamic path selection using VoCCN.
- **Automatic Failover**:
  - The strategy layer adapts to link failures by dynamically switching to alternate paths.
  - No packets were lost during VoIP calls; only minor delays (~50 ms) were observed.

---

## 6. Conclusion
- **Key Contributions**:
  - CCN simplifies networking by focusing on content rather than hosts.
  - It improves scalability, security, and efficiency, addressing the limitations of IP-based networks.
- **Incremental Deployment**:
  - CCN can be deployed as an overlay, coexisting with IP to enable gradual adoption.
- **Prototype Implementation**:
  - The open-source implementation, **Project CCNx**, demonstrates CCN’s utility for content distribution and point-to-point communication.

---

## Figures
- **Figure 5**: Bulk data transfer performance (TCP vs. CCN).
  - CCN has higher latency and therefore needs pipelining, its got quite a bit of overhead due to large variable headers, names etc. The x-axis shows the traffic inflight. In the beginning when a lot less traffic is in flight, the throughput is less as it might be single or very less packets in the connections and as the traffic over the connection increases, the throughput increases. Implication that: Multiple connections as well as high RTT connections are needed to utilize the link. 
  TCP also has some overheads but not as much as CCN and hence even though it has latency, CCN has reasonable performance.

- **Figure 6**: Total transfer time vs. number of clients (TCP vs. CCN).
- **Figure 7**: Automatic failover during VoCCN communication.
- they take some stuff down, they flip bw wired and wirless. 

---

## References
1. Van Jacobson, et al., “Networking Named Content,” Communications of the ACM, 2012.
2. Related works on CCN, SDSI, and content-based security are detailed in the references section of the paper.

---

## Acknowledgments
Special thanks to the authors and contributors for their work on CCN and Project CCNx.

=================================

# Class notes:

besides introducing ccn, they go about considering security
- integrity of data using crypto checksums
- pertinent: what does the data mean and repped using the name itself
- provenance: who gaurantees what data, public-key signatures
- and discuss DDoS attacks



- ccn addresses are a bit harder than ip based 128bits, but ccn addresses are veyr detailed and powerful

# trust in the internet:
  - all majorly fallback on public key encryption(PKI) methods and trusting certificate authorities at some point. CCN has the same kind of property
