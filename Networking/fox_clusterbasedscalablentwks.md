# Key Ideas:
-building scalable network services
    - scalable to a large # of machines to send a lot of data and hence **mostly amount of data(bitrate)**, and towards that a large # of concurrent users
    - incremental availability
    - cost-effective
-fault-tolerant
    - avoid single points of failures
    - want to provide high availability of data
        - maybe willing to sacrifice consistency 
    - **What is a big deal to Fox computers and not to the users?**
        - **any specific servers the user data is on as far as it is accessible, and thats what mostly matters wrt to cloud and networks, we want our data to be secure and could care less about where they are stored, treat the servers like cattle not pets**
        
 - BASE consistency over ACID
   - BASE : Basically Availability, Soft State, Eventual Consistency
   - ACID : Atomicity, Consistency, Isolation, Durability
       - *Relaxing consistency can be much faster*
 - talk about 2 services: Transcend and Inktomi

 - 
