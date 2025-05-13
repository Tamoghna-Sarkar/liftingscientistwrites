=== class notes

# applications/protocol are often ineffcient in how they use 4G and ramins on as we keep streaming video in bursts of videos 
but radio is off in phones just like in sensornets when they are not listening

# tcp over high bdp links is slow because TCP rcv wind is too small

# how is 4G different from other networks?
    - fairly high BDP
    - uses radios: 
        - we care abut power and battery consumption: radio sleep

    - the http issue(3rd point in key ideas slide): after the lingering http not ON, RADIO IS OFF, and let it timeout. But even while timing out it has to send an extra radio waleup message to inform it times out.


concept of lingering flows: graph ;difference between payload byte is the lingering flow from the tcp flow
lingering as app hopes to reuse connection.

tail time: TIMEOUTs


## what is a lingering flow?
– the app doesn’t close the flow,
– in case it might be able to reuse it later,
– but then it eventually times out and closes it
(if it hasn’t used it)
• how does this graph show flows linger?
– difference between green and red
## • why do flows linger?
– app hopes to resuse connection
## • why is this a problem?
– if you put the radio to sleep, then you have to
wake it up again to close
– if you don’t put the radio to sleep, but you
don’t use it, you’re wasting battery

# Huge RTTs

looking at diff rtts bw diff things, its normalized over the cdf for annonymization of RTTs
they break down into several parts to identify where the problem is bw C-S to identify which component is the most painful. **the monitor to server, in their wired backhaul network**
 
## but why is this a problem?
        - big queues in routers: bufferbload, latency is high, big buffers

# Passive BW Estimation:
    -  does it in the middle of the network, do better because of timestamps

# Utilization:
 - not using full network, because of the window buffer is too small
