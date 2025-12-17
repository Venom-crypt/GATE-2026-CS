# Computer Networks - GATE 2026 Practice Problems

## Part 1: Subnetting and CIDR Problems

### Problem 1.1
Given network 192.168.0.0/22, you need to create 4 equal subnets.
**Questions:**
a) What is the new CIDR notation for each subnet?
b) How many usable hosts per subnet?
c) List the network address for each subnet
d) What is the broadcast address for the third subnet?

**Solution:**
- Original: /22 = 1024 total hosts
- For 4 subnets: Need 2 additional bits = /24
- Each subnet: 256 total IPs, 254 usable
- Subnets:
  - 192.168.0.0/24 (0-255)
  - 192.168.1.0/24 (256-511)
  - 192.168.2.0/24 (512-767)
  - 192.168.3.0/24 (768-1023)
- Third subnet broadcast: 192.168.2.255

### Problem 1.2
Network: 10.0.0.0/8. Create 256 subnets with 254 hosts each.
**Questions:**
a) What is the subnet mask?
b) What is new CIDR notation?
c) How many bits for network and host portions?

**Solution:**
- 254 hosts = 8 bits needed (2^8 = 256, minus network and broadcast)
- 256 subnets = 8 bits needed (2^8)
- Total bits used: 8 (original) + 8 (subnets) = /16
- Subnet mask: 255.255.0.0
- Each subnet gets /24 notation

### Problem 1.3
Calculate how many Class B networks can fit into 172.16.0.0/12

**Solution:**
- /12 = 1,048,576 total IPs
- Class B = /16 = 65,536 IPs each
- Number of Class B networks: 1,048,576 / 65,536 = 16 networks

---

## Part 2: TCP Sequence Numbers and Handshake

### Problem 2.1
Client initiates connection. Client's initial sequence: 1000, Server's initial sequence: 5000
**Questions:**
a) What is the sequence number in client's first SYN?
b) What is acknowledgement number in server's SYN-ACK?
c) What is sequence number in server's SYN-ACK?
d) What is acknowledgement number in client's final ACK?

**Solution:**
- Client SYN: Seq=1000
- Server SYN-ACK: Seq=5000, Ack=1001 (client's seq+1)
- Client ACK: Seq=1001, Ack=5001 (server's seq+1)

### Problem 2.2
TCP segment lost causing timeout. CWND = 32 MSS, SST = 16 MSS
**Questions:**
a) What happens to SST and CWND?
b) How many RTTs to return to CWND = 32?

**Solution:**
- After timeout: SST = CWND/2 = 16 MSS, CWND = 1 MSS
- Slow start: 1 → 2 → 4 → 8 → 16 (4 RTTs)
- Congestion avoidance: 16 → 17 → 18 → 19 → ... → 32 (16 RTTs)
- Total: 20 RTTs to return

### Problem 2.3
Three duplicate ACKs received. CWND = 64 MSS, SST = 32 MSS
**Questions:**
a) What happens (Fast Retransmit)?
b) New CWND and SST values?
c) Will it slow start or congestion avoid?

**Solution:**
- Fast Retransmit triggered (3 dup ACKs)
- SST = CWND/2 = 32 MSS (unchanged)
- CWND = SST = 32 MSS (cut in half)
- Resumes congestion avoidance (linear increase)

---

## Part 3: Routing and Shortest Path

### Problem 3.1
Graph with nodes A, B, C, D, E
Edges: A-B(1), A-C(4), B-C(2), B-D(5), C-D(1), C-E(3), D-E(2)
Use Dijkstra to find shortest path from A to E

**Solution:**
- Initial: A=0, others=∞
- Process A: B=1, C=4
- Process B: C=3, D=6
- Process C: D=4, E=6
- Process D: E=6
- Shortest path A→E: 6 (A→B→C→D→E)

### Problem 3.2
Which routing protocol would you choose for:
a) A small network with <15 hops?
b) A large AS (Autonomous System)?
c) Internet backbone?

**Solution:**
a) RIP (simple, adequate for small networks)
b) OSPF (faster convergence, scalable within AS)
c) BGP (inter-domain routing, policy-based)

### Problem 3.3
Distance Vector routing. Initial DVs:
- Node A: A=0, B=4, C=1
- Node B: A=4, B=0, C=2
- Node C: A=1, B=2, C=0

After first exchange, update Node A's DV.

**Solution:**
- From B's DV: A=4, B=0, C=2
  - Path via B to C: 4+2=6
- From C's DV: A=1, B=2, C=0
  - Path via C to B: 1+2=3
- A's new DV: A=0, B=3 (via C), C=1
- Updated DV changed: B distance reduced from 4 to 3

---

## Part 4: Error Detection

### Problem 4.1
Calculate CRC for data 1101000 using generator polynomial 1011 (CRC-3)
**Steps:**
1. Append 3 zeros (polynomial length - 1): 1101000000
2. Perform XOR division
3. Remainder is CRC

**Solution:**
```
       1101
    -----------
1011 | 1101000000
       1011
       ----
        1100
        1011
        ----
         1110
         1011
         ----
          1010
          1011
          ----
             10000
             1011
             -----
              1010 (Remainder - this is CRC)
```
CRC = 010 (or 10 in binary)

### Problem 4.2
Sender sends data with even parity. Data contains 5 ones and 1 parity bit = 0
Receiver gets data with 5 ones and 1 parity bit = 0
**Question:** Is there an error?

**Solution:**
- Receiver counts ones: 5 (odd)
- Expected: even number of ones with even parity
- **Yes, error detected** (bit flip occurred)

### Problem 4.3
32-bit checksum using one's complement. Segments: 1011001101, 1110010010, 0110011001
**Calculate checksum:**

**Solution:**
```
  1011001101
+ 1110010010
-----------
 11001011111 (carry added back)
     011011111
+ 0110011001
-----------
 01001010110

Checksum = NOT(01001010110) = 10110101001
```

---

## Part 5: Network Protocols

### Problem 5.1
DNS Query for www.example.com from India
**Questions:**
a) How many DNS queries are needed?
b) What are the steps?
c) Which queries are recursive and which are iterative?

**Solution:**
- Query 1 (Recursive): Client to Local Resolver
- Query 2 (Iterative): Local Resolver to Root Server
- Query 3 (Iterative): Local Resolver to .com TLD Server
- Query 4 (Iterative): Local Resolver to example.com Authoritative Server
- Total: 4 queries minimum
- Response returned through chain back to client

### Problem 5.2
ARP Request: Who has 192.168.1.1? Tell 192.168.1.10 (MAC: AA:BB:CC:DD:EE:FF)
**Questions:**
a) What is broadcast address?
b) Who replies?
c) Is reply broadcast or unicast?
d) What is in the reply?

**Solution:**
a) Broadcast: FF:FF:FF:FF:FF:FF
b) Device with IP 192.168.1.1 replies
c) Unicast to requester (AA:BB:CC:DD:EE:FF)
d) Reply contains: Sender IP (192.168.1.1), Sender MAC (device's MAC)

### Problem 5.3
DHCP Lease process. 4 messages DORA.
**Map messages to their purposes:**
a) DHCP Discovery
b) DHCP Offer
c) DHCP Request
d) DHCP Acknowledgement

**Solution:**
- Discovery: Client broadcasts "I need IP"
- Offer: Server responds with IP proposal
- Request: Client confirms selection
- Acknowledgement: Server confirms lease

---

## Part 6: Fragmentation

### Problem 6.1
Packet of 3000 bytes (including 20-byte IP header) enters router with 1500 byte MTU.
**Questions:**
a) How many fragments?
b) What is data size in each fragment?
c) What are the fragment offset values?
d) Which fragments have MF=1?

**Solution:**
- Data size: 3000-20 = 2980 bytes
- Max payload per fragment: 1500-20 = 1480 bytes
- Number of fragments: ceil(2980/1480) = 3
- Fragment data sizes: 1480, 1480, 20 bytes
- Fragment offsets: 0, 185, 370 (in 8-byte units: 1480/8=185, 1480*2/8=370)
- MF=1 for fragments 1 and 2; MF=0 for fragment 3

### Problem 6.2
Router receives fragmented packet. Fragment offset=185, MF=1
**Questions:**
a) Starting byte position?
b) What does MF=1 mean?
c) Where does reassembly occur?

**Solution:**
a) Byte position: 185 * 8 = 1480 bytes
b) MF=1 means: More fragments coming after this one
c) Reassembly only occurs at final destination host

---

## Part 7: NAT and Port Forwarding

### Problem 7.1
Internal network: 192.168.1.0/24
Public IP: 203.0.113.5
Three internal hosts making connections:
- Host A (192.168.1.10:5000) → External server (8.8.8.8:80)
- Host B (192.168.1.11:5000) → External server (8.8.8.8:443)
- Host C (192.168.1.12:80) → External server (1.1.1.1:25)

**Create NAT translation table:**

**Solution:**
| Internal | Internal Port | Public | Public Port | Remote | Remote Port |
|----------|---------------|--------|-------------|--------|------------|
| 192.168.1.10 | 5000 | 203.0.113.5 | 5000 | 8.8.8.8 | 80 |
| 192.168.1.11 | 5000 | 203.0.113.5 | 5001 | 8.8.8.8 | 443 |
| 192.168.1.12 | 80 | 203.0.113.5 | 5002 | 1.1.1.1 | 25 |

Port numbers changed to differentiate multiple internal sources

### Problem 7.2
Port forwarding: Public 203.0.113.5:80 → Internal 192.168.1.50:8080
External client connects to 203.0.113.5:80
**What happens?**

**Solution:**
1. NAT device receives connection on 203.0.113.5:80
2. Looks up port forwarding rule
3. Translates to 192.168.1.50:8080
4. Forwards connection to internal server
5. All subsequent traffic flows through NAT

---

## Part 8: Mixed Problems

### Problem 8.1
Network 10.0.0.0/8 with 100 departments, each needing 510 hosts.
**Questions:**
a) CIDR for each department?
b) Total subnets needed?
c) Subnetting scheme?
d) First and last department networks?

**Solution:**
a) 510 hosts = /23 (512 total, 510 usable)
b) 100 departments need 100 subnets of /23 = /15 (2^9 combinations)
c) Use /15 subnets: each department gets /23
d) First: 10.0.0.0/23, Last: 10.127.254.0/23

### Problem 8.2
Comparing protocols: Choose TCP or UDP for:
a) Video streaming
b) Online banking
c) Multiplayer game
d) Email sending
e) Instant messaging (real-time)

**Solution:**
a) UDP (speed > reliability)
b) TCP (reliability > speed)
c) UDP (low latency > perfect delivery)
d) TCP (reliability critical)
e) UDP (real-time communication)

### Problem 8.3
Slow start simulation. CWND = 1, MSS = 1460 bytes, SST = 16
**Show CWND growth for 10 RTTs:**

**Solution:**
- RTT 1: CWND = 1, Total = 1 MSS
- RTT 2: CWND = 2, Total = 3 MSS cumulative
- RTT 3: CWND = 4, Total = 7 MSS cumulative
- RTT 4: CWND = 8, Total = 15 MSS cumulative
- RTT 5: CWND = 16 (equals SST), Total = 31 MSS
- RTT 6: CWND = 17 (congestion avoidance), Total = 48 MSS
- RTT 7: CWND = 18, Total = 66 MSS
- ... continues linearly

---

## Part 9: Previous GATE Style Questions

### Problem 9.1 (GATE Style)
Which of the following are true?
S1: TCP handles both flow control and congestion control
S2: UDP handles flow control but not congestion control
S3: Flow control is receiver-based
S4: Congestion control is network-based

a) S1 and S3 only
b) S1, S3, and S4
c) All statements
d) None

**Answer:** (b) S1, S3, and S4
**Explanation:** TCP does both; UDP does neither (S2 false); S3 and S4 are correct

### Problem 9.2 (GATE Style)
For IPv4 address 192.168.1.128/25:
Number of host addresses = ?

a) 128
b) 126
c) 127
d) 125

**Answer:** (b) 126
**Explanation:** /25 = 7 host bits = 2^7 = 128 total - 2 (network, broadcast) = 126

### Problem 9.3 (GATE Style)
In DNS resolution for example.com, iterative queries are sent by:
a) Client to resolver
b) Resolver to root server
c) Both a and b
d) Neither a nor b

**Answer:** (b) Resolver to root server
**Explanation:** Client makes recursive query; servers make iterative queries to each other

---

## Answer Key Summary

**Part 1:** Subnetting fundamental concepts
- Remember: 2^n addresses from n bits
- Usable = Total - 2
- CIDR calculation consistent

**Part 2:** TCP mechanics
- Sequence numbers always in order
- SYN consumes sequence number
- Congestion control on timeout vs 3-dup-ACK

**Part 3:** Routing selection
- RIP: distance vector, limited hops
- OSPF: link state, faster
- BGP: inter-domain

**Part 4:** Error detection
- CRC > Checksum > Parity
- CRC detects errors, doesn't correct

**Part 5:** Protocols flow
- DNS: hierarchical resolution
- ARP: broadcast request, unicast reply
- DHCP: DORA process

**Part 6:** Fragmentation rules
- Only at source or intermediate routers
- Reassembly only at destination
- Fragment offset in 8-byte units

**Part 7:** NAT translation
- Stateful translation
- Port numbers distinguish multiple internal sources
- Port forwarding special case

**Part 8-9:** Application knowledge
- Protocol selection based on reliability vs speed
- Cumulative understanding of concepts

---

Good luck! These problems cover ~70% of likely GATE questions for Computer Networks.
