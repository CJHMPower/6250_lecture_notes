1. How does the “narrow waist” allow a large variety of different protocols to be used on the Internet?
  - the only required protocol is IP, that means that every other layer can implement whatever protocol they desire - so long as it can interface with
    IP. The network hold no state outside of the endpoint (host) and client.

2. Discuss how the design principles of the Internet are manifest in the ARP protocol.  Also discuss how the absence of some points from the design
   principles (e.g., security and mobility) has influenced ARP.
  - I think ARP protocol is referring to packets/datagrams.
  - ARP queries allow an IP address to be resolved into a MAC address for a destination computer on the network.
  - The most valued design principle of the internet is that communication must still be possible despite loss of networks or gateways. The state is
    all stored via the client and host - so network failure does not result in total loss of all transmission.

3. What are the advantages of using circuit-switched networks instead of packet-switched networks (like the Internet) for streaming video?
  - I'll need to rewatch the lecture to grab this one completely, but I can use the terms I know now.
  - Since 'best effort' delivery is the only assurance that the internet allows, packets can be lost. Via windowing and buffering sizing, this isn't
    usually an issue in normal internet traffic.
    - However, for VOiP and streaming video - the procedure for handling lost packets isn't as easy as requesting them again. The stream has to be
      persistent.

4. In what way are caching HTTP proxies a violation of the end-to-end argument?  Also, what are the consequences of this?
  - http proxies sit in between a client and host. Since the packets are inspected on the proxy, they violate the e2e agreement.
  - Negative consequences are that it increases the attack surface on unencrypted packets if a malicious attack were to take place.
  - Positive consequences are the efficiency increases in response times for common requests.

5. The forwarding tables for all switches in this network are initially empty.
   Host 192.168.1.1 sends and ARP request to discover the MAC address of the
host with IP 192.168.1.2. What entries will be in the forwarding table of each
switch after host 192.168.1.1 receives the ARP reply?

|switch|port|node|
|---|---|---|
|sw3|1|A|
|sw3|2|B|
|sw1|3|A|
|sw1|2|B|
|sw4|2|A|
|sw4|1|B|
|sw2|1|B| This one was incorrect
|sw2|1|A|

6.  When a host on the private network side of a NAT with IP 192.168.1.100 opens a TCP
connection to a web server at 123.45.67.89, what entry will be added to the following
NAT table, and what will be the source and destination IP and port on the packet that
the NAT router forwards on the WAN (public) side? (Recall that the default port for
HTTP is port 80.)
