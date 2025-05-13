# Key Ideas:
- BGP is easy to attack
    -   route hijacking, specifically prefix hijacking and sub-prefix hijacking
    - route leaks

- there are also some defenses
    - prefix filtering
    - RPKIs: **protects only the ORIGIN**
    - BGPsec: **protects every hop** 


Defenses that Goldberg brings up:

- ROute filtering: what it announces and what it accepts
    implemented in BGP configs in RIB
    info comes either from yourself(ex: w/your customer)
    people using big databases(routing arbiter) of publicly provided routing policies...[continued in slide]
- RPKIs : [definition in slide]
    Advs:
        All security is online, and can be done daily[so router cpu doesnt matter]
    Cons:
        we dont mostly talk to all originators and RPKI protects only the origin, whereas most routing requires several hops
- BGPsec: 
    how does this go beyond RPKIs or what RPKIs does?
        lets us validate every hop on the path and not just the originator.
    Downside:
        On-line cryptography(an people are worried processors are not that fast)
    
sdns do "flow level" routing and not routing specific to BGP.

What did Goldberg say about RPKI?
    -Uses PKI
    - We sign that some AS with its route origin number or AS number, along with USC's cryptography key, and publishes this signature along with their public keys.
    - Whoevers receiving it gets the route and the ASPath from BGP and once a day they check whether the originator is the correct origin.
        - once a day they check the public key of the originator that it was indeed the actual originator who signed the key and the details are correct. 
## one main compelling reasons to deploy rpkis?
 - it can deployed by individually and doesn't depend on others like BGPsec which requires everyone to deploy it.
 - it took a lot time to deploy RPKIs


 ## Defenses: BGPSEC
• what:
– let’s sign all BGP messages with our peers
• why?
– can validate every step on the AS path
• why not?
– relatively little benefit until almost everyone does it
– **on-line cryptographs, can be taxing on router CPUs** 

