## Questions

## Test Prep
5. Explain the ways in which DNS (the Domain Name System) achieves fault-tolerance, i.e.,
such that most or all domain names can be resolved to IP addresses even if some (but
not all) DNS servers crash

-------------

#### DNS
  - the purpose of DNS is to map domain names (www.gatech.edu) to IP addresses
  - in memory the client can have a *stub resolver*
    - if the stub resolver does not have the domain name -> IP mapping, it will go to the *local DNS resolver*
    - there are 'A' queries and 'NS' queries
  - Since there is a lot of back-n-forth between the nameservers and the local DNS resolver, the local DNS resolver will cache the responses.

#### Record Types
  - *A record*: map domain name to IP
  - *NS record*: map domain name to authoritative nameserver
    - also known as *referral*
  - *MX record*: mail server
  - *Cname*: is an alias from the domain name to the canonical name
  - *PTR*: is the reverse of an A record. It maps from IP -> name
    - also known as *reverse lookup*
  - *AAAA*: maps domain name to IPv6

#### Examples
```
$ drill www.nytimes.com
;; ->>HEADER<<- opcode: QUERY, rcode: NOERROR, id: 30646
;; flags: qr rd ra ; QUERY: 1, ANSWER: 2, AUTHORITY: 4, ADDITIONAL: 0 
;; QUESTION SECTION:
;; www.nytimes.com. INA

;; ANSWER SECTION:
www.nytimes.com.  500INCNAMEnytimes.map.fastly.net.
nytimes.map.fastly.net. 30646INA151.101.21.164

;; AUTHORITY SECTION:
fastly.net. 238INNSns1.fastly.net.
fastly.net. 238INNSns1INNSns2.fastly.net.
fastly.net. 238INNSns1INNSns2INNSns3.fastly.net.
fastly.net. 238INNSns1INNSns2INNSns3INNSns4.fastly.net.

;; ADDITIONAL SECTION:

;; Query time: 31 msec
;; SERVER: 209.222.18.218
;; WHEN: Tue Sep 26 21:49:58 2017
;; MSG SIZE  rcvd: 157
```

 - In the `ANSWER SECTION` you will sometimes see more than 1 IP
    - This is for load balancing

### PTR Example
  - **REMEMBER**: the octets are reversed in a reverse lookup!

```
$ drill -Tx 130.207.244.165
;; ->>HEADER<<- opcode: QUERY, rcode: NOERROR, id: 42057
;; flags: qr rd ra ; QUERY: 1, ANSWER: 1, AUTHORITY: 3, ADDITIONAL: 0 
;; QUESTION SECTION:
;; 165.244.207.130.in-addr.arpa.  INAPTR

;; ANSWER SECTION:
165.244.207.130.in-addr.arpa. 300INPTRtlw-cache.rich.gatech.edu.

;; AUTHORITY SECTION:
244.207.130.in-addr.arpa. 300INPTRtlwINNSdns1.gatech.edu.
244.207.130.in-addr.arpa. 300INPTRtlwINNSdns1INNSdns2.gatech.edu.
244.207.130.in-addr.arpa. 300INPTRtlwINNSdns1INNSdns2INNSdns3.gatech.edu.

;; ADDITIONAL SECTION:

;; Query time: 160 msec
;; SERVER: 209.222.18.222
;; WHEN: Tue Sep 26 21:59:50 2017
;; MSG SIZE  rcvd: 142
```

  - you can see that the `in-addr.arpa` has the reverse octets `165.224.207.120`
