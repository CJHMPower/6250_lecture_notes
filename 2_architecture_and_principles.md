#### Questions
  - what do they mean by *out of band*? This is in L1E6 when talking about
    circuit switching

------------------

  - IPv4 only has 2^32 addresses. 1/256 of these are owned by MIT
  - *BGP* is the current inter-domain routing protocol

  - There were 2 fundamental goals of the original design of the internet
    - *Multiplexed utilization of existing interconnected networks*

## Goal 1: Sharing
  - this is the *Multiplexed Utilization* portion of the above quote
  - Statistical multiplexing (aka *packet switching*) was invented to solve the
    sharing problem

### Packet Switching
  - *datagram* == packet
  - each packet has a destination address, but no state-transfer is set up ahead
    of time.
    - you don't allocate resource end-to-end for a packet before you send it.
    - the downside of this is that you can lose or drop packets
  - this uses "*best effort*" service
    - it's called this because everyone is sharing resource nodes to get to
      their destination. What this means is that there are not many guarantees
on the level of service as your packets travel between nodes. There can be
delays and such.
  - This is in contrast to *Circuit Switching*. That is what phone calls use.
    They set up and allocate resources end-to-end before you connect a phone
call, which means that those resources cannot be shared
    - It sets them up *out-of-band*

## Goal 2: Interconnection
  - *narrow waist* was invented to solve the connectedness problem

### Narrow Waist
  - "*IP over anything*"
  - in the OSI model, each layer has multiple protocols **except for the network
    layer**
    - the only real protocol available on the network layer is IP
    - this means that everything connected to the internet must utilize IP
  - One downside to this is that it makes it very difficult to change anything
    about IP

-----------

## End 2 End Argument

  - *Dumb Pipe*
  - Basically, all of the logic to determine what to do with packets should be
    at the endpoints. The intermediate network shouldn't know anything about
what is going to happen with the information being shared at all

### E2E Violations
  - VPN tunnels
  - TCP splitting
    - If a proxy is used for retrying packet transfer, this violates.

#### NAT
  - since only your default gateway (usually your home router) has a public IP -
    all devices in your home that connect to the internet will have a public IP
that's only visible to your home network.
  - this means that all packets that are received from the public internet need
    to be translated through a table. This is NAT.
  - inspecting the packets in this way violates e2e
