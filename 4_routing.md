## Questions

  1. Are BGP communites used to change inbound traffic?

-----------

## Routing
  - traffic over the internet flows through many *autonomous systems*
  - traffic **inside** the AS is called *intradomain routing*
  - traffic **between** AS is called *interdomain routing* (yes these look confusing and swapped, but they aren't)

## Intra-domain routing (within AS)
  - a topology exists of nodes and edges
    - nodes are sometimes called *points of presence* or **PoP**
    - PoPs are generally near high population centers (nodes are centers of traffic)
    - the edges are usually fiber lines and oftern run parallel to physical transportation routes like highways and railroads

  - just like in 6340, the network needs to find paths through the graph
  - there are two kinds of intra domain routing
    - distance vector
    - link state

#### Distance Vector Routing
  - sends *vectors* to it's neighbors (these are basically copies of its routing table)
  - the routers compute the weight/cost to each destination, based on the shortest available path
    - DVR routing is based on the *Bellman-Ford* algorithm
    - remember BF takes **at most** `n-1` iterations (where `n` is the # of nodes)
  - News of failures travel slowly because of the *count to infinity* problem
    - the solution to *count to infinity* is called *poison reverse*
      - you basically update a node to say that it's cost to a higher node is infinity, which forces the lower cost path

#### RIP (Routing Information Protocol)
  - count to infinity value was 16
  - edges had unit (1) cost
  - table refresh every 30 seconds
  - **Split Horizon**: updates are sent to all neighbors except the one that induces the update
  - because of the 180 sec timeout there is **slow convergence**
  - also the count to infinity is not ideal

#### Link State Routing
  - alternative to RIP
  - most modern networks today use this
    - two examples are *open shortest paths first (OSPF)* and *Intermediate System/Intermediate System (ISIS)*
  - each node distributes a network map to every other node in the network
    - then each node computes a *shortest path first computation* (SPF)
    - this is the Dijkstra algorithm
    - grows with O(n^2)

  - One problem is scaling, to deal with scaling both use a hierarchy with a notion of *areas*(OSPF) a.k.a *levels*(ISIS)

## Inter-domain routing (between AS)
  - AS's send route informs the reachability to some node via *route advertisements*
  - **Border Gateway Protocol** (BGP) is the protocol of these route advertisements
  - the three most important attributes of BGP are:

```ruby
route_advertisement = {
                        destination: 130.207.0.1,
                        next_hop: 192.5.89.89,
                        as_path: [10578, 2637]
                      }
```

  - the `destination` and `next_hop` routers are on the same subnet
  - the `as_path` is a sequence of IPs that signify the path along AS's to the destination
    - the `as_path.last == 2637` is known as the *origin AS*

  - *external* BGP is the protocol between adjacent AS's and refers to external destinations
  - *internal* BGP is the protocol inside an AS and also refers to external destinations

#### IGP vs IBGP
  - **both** are for routing **inside** an AS
  - *internal gateway protocol* refers to internal destinations
  - *internal border gateway protocol* refers to external destinations

#### BGP Route Selection Process
  - prefer higher local preference
  - shortest AS path length
  - multi exit discriminator (MED)
    - signifies that one exit point in the AS is preferred over all others
    - the exit points are ranked by their MED value
    - **lower** MED values are preferred
  - want shortest IGP value
    - meaning from origination to next_hop

#### Local Preference
  - useful for configuring primary and backup routes
  - basically, the default is 100 and network operators can adjust the LPV to control **outbound** traffic
    - usually local preference values LPVs are used to control outbound traffic
    - if you want to control incomin traffic to yourself, you can use **BGP Communites**. These are basically *tags* that are attached to a route to effect how a neighboring AS sets local preference.

#### Multi Exit Disriminator (MED)
  - overrides hot potato routing
    - you set a higher MED on one of your incoming routes
  - it's used to force the sender to route traffic across it's own backbone instead of dumping traffic onto the reciever
  - one issue is that a small change in IGP routing inside the senders network can cause 10x thousands of IP routing addresses to change

#### Inter-Domain Routing Business Relationships
  - there are 2 kinds of interdomain business relationships

  1. Customer/Provider
    - money goes from the customer to the provider
    - is independent of the direction that traffic flows
  2. Peering (a.k.a. Settlement-Free Peering)
    - AS can exchange traffic with another AS free of charge

  - the rules of preferences are Customer > Peer > Provider
  - also need to consider filtering/export decisions
    - basically the rules are this:
      - if you learn a new route via a customer; advertise that route to all connections
      - if you learn a new route via a provider or a peer, only advertise that to customers. You do not want other providers routing through you when you're paying for the routing

#### Interdomain routing can oscillate indefinitely

  - BGP is not guaranteed to be stable
