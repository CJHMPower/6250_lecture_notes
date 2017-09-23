# The Design Philosophy of the DARPA Internet Protocols
  - using the 3 pass approach

## 1st pass
  - just going to read the pieces and not take any notes during the first pass.

## 2nd pass: notes

#### Fundamental Goal

  - TCP and IP are fundamental layers to the design of todays internet, but weren't in the original proposal.
    - they were integrated due to testing before the final draft of the proposal.

  - The original goal of DARPAs research was to develop a technique to  utilize all the existing/interconnected networks that already existed.
    - the very fist networks that were to be connected were ARPANET and the ARPA packet radio network

  - One interesting problem is that each network represented (and still does today) an independent domain of control.
    - i.e. each network is administrated by the org that controls that network.
    - a goal of the original internet was to tackle this issue
  - Networks are connected to each other via gateways
    - gateways are layers of internet packet switches
    - gateways use a packet switching algorithm called *store and forward*

#### Second Level Goals
  - The ordering of the 7 goals of the internet highly influenced its design
    - They are ordered from most to least important
  - Since the original context for the internet project was military, the survival of the network was placed higher than its accountability

#### Survivability

# Questions for test - self answers

1. Discuss the advantages of the fate-sharing survivability model present in Internet
Protocols today, and how this model differs from a distributed state/replicated
survivability model.
  - here's where answer goes

2. Explain how the Internet architecture achieves the flexibility required to support a wide
variety of networks? What functions is the network assumed to provide, and more
importantly, what functions of the network are NOT assumed?

3. How has the ARPANET design goal of distributed resource management across the
network not been met in today’s Internet?

4. Describe two of the design advantages of datagrams as the basic architectural feature of
the Internet
