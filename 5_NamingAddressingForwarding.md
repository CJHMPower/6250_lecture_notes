## Questions
  - for test prep #3, why does it match even though the 9 in 39 doesn't match with the 2 in 32

-----------------------

## Test Prep Answers (first draft)
1. Consider a subnet that has the following IP addresses. What is the smallest subnet (i.e.,
subnet with the longest prefix) that could include all these addresses? (Use CIDR, i.e.
“slash”, notation.)
```
192.168.165.1
192.168.165.96
192.168.160.1
192.168.179.145
```
  - First guess I think it's 192.168.1.0/17
    - I think this because the first 2 octets are the same in all IPS (so that's 8x2 == 16). Then you have the 3rd octet all starting with 1. So 16+1
      is how long the longest prefix would be that included all of these IPs.

2. How many addresses are in the subnet 10.11.12.128/26 ? (Show your work.)
  - 2^6. This is 32 bits - 26 bits = 6 bits.

3. Now consider a router with the following forwarding table:

|Subnet| Next | Router Cost |
| --- | --- | --- | --- |
|73.214.32.0/20| 192.168.1.1| 3|
|73.214.0.0/16|192.168.2.1|4|
|73.214.42.0/24|192.168.3.1|5|
|73.214.37.0/24|192.168.4.1|6|
( Note: 42 = 32 + 8 + 2 and 37 = 32 + 4 + 1 )

a. To which router will a datagram with destination IP address 73.214.106.198 be
sent next?
  - 2.1

b. To which router will a datagram with destination IP address 73.214.42.64 be sent
next?
  - 3.1

c. To which router will a datagram with destination IP address 73.214.39.216 be
sent next?
  - 1.1 **I don't understand this answer**

-----------------------
#### IP Addressing
  - each IPv4 address is 32 bits, split into 4 octets
    - there are 2^(# of bits) permutations of any number
    - so an IPv4 address has 2^32 possible values

#### Before 1994
  - split into *network ID* portion and *host ID* portion
  - any IP that starts with a 0 is a **class A** address
    [first bit is 0] [next 7 bits are network ID] [2^24 host ID]
  - **class B**
    [ 1 0 ] [ 2^14 Network ID] [ 2^16 host ID]
  - **class C** can only have 255 hosts on it (2^8 == 256)
    [1 1 0] [2^21 Network ID] [ 2^8 host ID]

#### IP Allocation
  - Internet Assigned Numbers Authority
  - Ethernet, MPLS, and ATM use exact matching
  - IPv4 and IPv6 use LPM

#### CIDR (Classless Interdomain Routing)
  - in 1994 we began running out of class c address space
  - Okay... So the /XX indicates the **length of the network ID** (that's the first portion).
    - remember that it goes [network ID][host ID]
  - Slowed growth of global routing tables
    - ~ linear growth from 1994-1998

#### LPM
  - Imagine you have 2 addresses
    - `65.14.248.0/22` (more IPs, since there are 2^10 host IDs)
    - `65.14.248.0/24` (subset, 2^8 host IDs)
  - You would match on the `/24` prefix, since it is more specific
  - The benefits are:
    a. efficiency
    b. aggregation

### LPM video
  - A router's job is to find out how to get a packet through the network
    - this happens in 2 parts:

  1. Figure out what its paths are to get to every destination
    - it uses a routing table to determine the answer to this question (which port it should send the packet out of)
    - the router will take the rules from the routing table and apply them to a k-ary tree.

#### Multihoming
  - contributed to the growth of global routing tables ~1999
  - makes it difficult for upstream providers to aggregate IP prefixes together
    - for instance: Say you have 2 ISPs that get the same IP address that an AS wants to advertise.
    - If the ISPs wanted to aggregate the (say /24) prefix into their prefix (say /8) than the one with the longest prefix would get all the traffic -
      so they can't really aggregate it
