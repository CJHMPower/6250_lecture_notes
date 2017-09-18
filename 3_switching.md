## Questions
  - are buffer sizing and TCP flow proof going to be tested?
    - you should rewatch the last 2 portions of the lecture

--------------

# Switching
  - Each host on a network has an ethernet adaptor. The ethernet adaptor has a
    MAC address (also known as LAN or physical address).
    - Hosts can send packets/datagrams to another host by using the LAN of its
      ethernet adaptor.
  - Broadcast MAC address: this is an address that will result in ALL hosts on
    the network receiving the datagram
  - PROBLEM: a host knows the DNS of the other host it wants to send a packet
    to, but doesn't know the LAN of that host's ethernet adaptor.
    - So, how can a host learn the MAC address of another host's ethernet
      adaptor?
    - *SOLUTION*: The *Address Resolution Protocol* [ARP]

### ARP
  - Just to recap, ARP is used to resolve the LAN of an ethernet adaptor given
    the IP of the host it wants to send a packet to.
  - **ARP query**: The first step is to use the *broadcast MAC address* to send a query to all
    hosts on the network. This is known as the ARP query.
    - This message is basically saying *"who has the IP address of
      xx.xx.xx.xx?"*
    - The response from the host is the MAC address of the host's ethernet
      adaptor.
  - **ARP table**: when the host that issued the original ARP query receives a
    reply, it creates a table of IP to MAC of other hosts on the network.
    - So when the host wants to send another packet to an IP:
      - It checks the ARP table for the MAC of that host's IP
      - it wraps the packet in an *ethernet frame*

### Hubs
  - Hubs == *broadcast mediums*
  - PROBLEMS:
    - every packet gets broadcast to every connection.
    - There is a lot of flooding
    - high chance of collision
    - configuration vulnerability. A single misconfigured hub and cause problems
      for the entire network
    - TL;DR - hubs need some isolation features

### Switches
  - have traffic isolation
  - switches can break a subnet into multiple LAN segments, allowing each
    segment to have traffic isolation wrt the network at large
  - PROBLEMS:
    - requires a *switch table*. This maps destination MAC addresses to output
      ports on the switch.

### Learning Switches
  a. if there is no entry in the forwarding table?
    - FLOOD. This means that it will send the request to all LAN segments (to
      each output port).
    - It also means that it learns the output port of the sender, since the
      original request must have gone to the output port associated with the
sender.

### Loops
  - most underlying physical topologies have loops. This is a safety redundancy.
    If any particular link on the network fails, you'd like to have hosts remain
connected.
  - since all switches will go through these cycles, you can have *forwarding
    loops* and *broadcast storms*

### Spanning tree
  - a loop free topology that covers every node in the graph

#### Construct a Spanning tree
  a. elect a root
    - typically this is the switch with the smallest ID
  b. add each switch
    - exclude a link if it isn't the shortest path to the root (distance is
      measured by *hops to root*)
  c. determine the root
    - the problem is that initially each switch think it is the root
    - the switches run an election process to determine which has the smallest
      ID
    - if a switch learns of another switch with a smaller ID than the current
      root, they update their view of the root

### Switches vs Routers
SWITCHES
  - normally operate at layer 2 (this is where ethernet operates)
  - auto configuring
  - forwarding is usually fast
  - major limitations
    - Broadcast. Both ARP queries and STP impose a pretty high load on switches

ROUTERS
  - operate at layer 3 (this is the IP layer)
  - router level topologies are not restricted to a spanning tree

### Buffer Sizing
  - the buffer is memory used to store a packet to be sent at some later time
  - PROBABLY A TEST QUESTION:
    - 2T == time taken for packet to travel from source to destination in units
      seconds
    - C == capacity of the bottleneck link (from switch to destination) in units
      bits per second
    - `2T*C` == recommended buffer size in bits
  - Problems with allocating too much buffer size
    - cost $$$
    - delay in interactive traffic
    - delay in feedback about network congestion
