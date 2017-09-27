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
