# Key:
- revisit some internet architecture from early times
- not jsut connectivity, now we can do different applications
- can now provide QoS
  - applications get gaurantees about bitrate, latency, jitter
  ## main thing they ask is: HOW SHOULD WE DO QoS?
- need explicit requests: reservations
- and maybe we need admissions  
  
  ## how to think about this change and how do we evaluate if its better?

## tries to quantify user-happiness and also find a trade-off that if users like to have gaurantees about services, would they be willing to pay for this

letting *U(j)* being the happiness


## Elastic traffic: more users is okay and can be handled

- here i dont need admission control as with more people they will sort it out how to balance and adapt. 

## Inelastic traffic: more users is too much, can make things worse for everybody

- it needs **reservation**, where we gaurantee your requirement, and you use that and interrupt no one else and nobody interrupts you. 

- admission control can help


# What can we use to improve things for everyone?

  

# Traffic Options:
- semi adaptive video streams sometimes where you handle some loss
- Other thing complicated about video is teleconferencing:
  -  real time!
 
  # overprovision the network or gauranteeing the service(Integrated services QoS):
  -  overprov: create pipes with a lot of BW enough that you dont have to worry about # of users
  -  integrated services(QoS): you reserve some bitrate, and you're gaurantee to get some)
  -  

# mostly ppt