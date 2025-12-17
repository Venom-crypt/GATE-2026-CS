# Computer Networks - GATE 2026 Quick Reference Cheat Sheet

## OSI Model Layers (Memory Aid: "All People Seem To Need Data Processing")

1. **Application** (Layer 7): HTTP, FTP, SMTP, DNS, Telnet
2. **Presentation** (Layer 6): Encryption, compression, format conversion
3. **Session** (Layer 5): Connection management, dialog control
4. **Transport** (Layer 4): TCP, UDP, flow control, congestion control
5. **Network** (Layer 3): IP, routing, logical addressing
6. **Data Link** (Layer 2): Framing, MAC, error detection
7. **Physical** (Layer 1): Cables, signals, bits

## TCP/IP Model Layers

1. **Link Layer**: Physical + Data Link (Ethernet, PPP)
2. **Internet Layer**: IP, ICMP, IGMP
3. **Transport Layer**: TCP, UDP
4. **Application Layer**: HTTP, DNS, SMTP, FTP, SSH

## Key Protocols by Port

| Protocol | Port | Type |
|----------|------|------|
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| SSH | 22 | TCP |
| Telnet | 23 | TCP |
| SMTP | 25, 465, 587 | TCP |
| DNS | 53 | UDP/TCP |
| DHCP | 67, 68 | UDP |
| FTP | 20 (data), 21 (control) | TCP |
| POP3 | 110 | TCP |
| IMAP | 143 | TCP |
| SNMP | 161 | UDP |
| NTP | 123 | UDP |

## CIDR Quick Reference

| CIDR | Subnet Mask | Total IPs | Usable | /24 Equivalent |
|------|-------------|-----------|--------|----------------|
| /30 | 255.255.255.252 | 4 | 2 | 1/64 |
| /28 | 255.255.255.240 | 16 | 14 | 1/16 |
| /27 | 255.255.255.224 | 32 | 30 | 1/8 |
| /26 | 255.255.255.192 | 64 | 62 | 1/4 |
| /25 | 255.255.255.128 | 128 | 126 | 1/2 |
| /24 | 255.255.255.0 | 256 | 254 | 1 |
| /22 | 255.255.252.0 | 1024 | 1022 | 4 |
| /20 | 255.255.240.0 | 4096 | 4094 | 16 |
| /16 | 255.255.0.0 | 65536 | 65534 | 256 |
| /8 | 255.0.0.0 | 16777216 | 16777214 | 65536 |

## IPv4 Class Reference (Historical)

| Class | Range | Network Bits | Host Bits |
|-------|-------|--------------|-----------|
| A | 1-126 | 8 (/8) | 24 |
| B | 128-191 | 16 (/16) | 16 |
| C | 192-223 | 24 (/24) | 8 |
| D | 224-239 | Multicast | - |
| E | 240-255 | Reserved | - |

## TCP vs UDP

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | 3-way handshake | None |
| Reliability | Guaranteed delivery | Best effort |
| Ordering | In-order | Not guaranteed |
| Speed | Slower | Faster |
| Header | 20 bytes min | 8 bytes fixed |
| Checksum | Mandatory | Optional |
| Flow Control | Yes (sliding window) | No |
| Congestion Control | Yes | No |
| Streaming | Stream-based | Datagram-based |
| Use | HTTP, FTP, SMTP | DNS, VoIP, Gaming |

## TCP Connection States

**Three-Way Handshake (Establishing):**
- SYN: Client sends SYN=1, Seq=x
- SYN-ACK: Server sends SYN=1, ACK=1, Seq=y, Ack=x+1
- ACK: Client sends ACK=1, Seq=x+1, Ack=y+1

**Four-Way Termination (Closing):**
- FIN: One side sends FIN=1
- ACK: Other side acknowledges
- FIN: Other side sends FIN=1
- ACK: First side acknowledges

**States:**
- LISTEN: Server waiting
- ESTABLISHED: Connected
- TIME_WAIT: After FIN received
- CLOSE_WAIT: After sending FIN acknowledgement

## TCP Congestion Control

**Slow Start:**
- CWND starts at 1 MSS
- Doubles every RTT: 1, 2, 4, 8, 16...
- Stops at Slow Start Threshold (SST)

**Congestion Avoidance:**
- CWND increases by 1 MSS per RTT (linear)
- Continues until packet loss

**Loss Detection:**
- 3 Duplicate ACKs: Fast retransmit (CWND = CWND/2)
- Timeout: Severe congestion (CWND = 1, SST = CWND_old/2)

**Relationship:**
- Actual window = min(rwnd, cwnd)
- rwnd = Receiver advertised window (flow control)
- cwnd = Congestion window (congestion control)

## Error Detection Methods

**Parity Check:**
- Detects odd/even number of bit errors
- Single parity detects single bit errors only
- Even parity: Total 1's should be even

**Checksum:**
- Uses 1's complement arithmetic
- Divide data into fixed segments, add them
- Complement sum for checksum
- Limitations: Can't locate error position

**CRC (Cyclic Redundancy Check):**
- Polynomial division method
- Best error detection for LANs
- CRC-16: Detects up to 16-bit bursts
- CRC-32: Detects up to 32-bit bursts

## Routing Protocols Comparison

| Aspect | RIP | OSPF | BGP |
|--------|-----|------|-----|
| Type | Distance Vector | Link State | Path Vector |
| Metric | Hop count | Cost | Attributes |
| Max Hops | 15 | Unlimited | Unlimited |
| Update | Periodic (30s) | Triggered | Triggered |
| Convergence | Slow | Fast | Very Slow |
| Scalability | Poor | Good | Excellent |
| Scope | Interior | Interior | Exterior |

## Distance Vector (Bellman-Ford)

**Bellman-Ford Equation:**
```
Dx(y) = min{c(x,v) + Dv(y)} for all neighbors v
```

**Problems:**
- Slow convergence
- Counting to infinity
- Large routing tables

**Solutions:**
- Maximum hop count limit
- Split horizon
- Poison reverse

## Link State (Dijkstra)

**Steps:**
1. Router floods link state to all routers
2. All routers build complete topology
3. Each router runs Dijkstra algorithm
4. Creates shortest path tree
5. Updates routing table

**Advantages:**
- Fast convergence
- Triggered updates only
- More scalable

## DNS Resolution Process

**Recursive Query (Client to Resolver):**
Client → Resolver → Root → TLD → Authoritative → Resolver → Client

**Iterative Query (Between servers):**
Resolver asks Root, TLD, and Authoritative separately

**Process:**
1. Browser checks cache
2. If miss, queries resolver
3. Resolver queries root server
4. Root gives TLD server address
5. Resolver queries TLD server
6. TLD gives authoritative server address
7. Resolver queries authoritative server
8. Gets IP and returns to client

**Record Types:**
- A: IPv4 address
- AAAA: IPv6 address
- CNAME: Alias
- MX: Mail exchanger
- NS: Name server
- SOA: Zone start
- TXT: Text records
- PTR: Reverse DNS

## ARP Process (IP → MAC)

1. Check local ARP cache
2. If found, use cached MAC
3. If not found, broadcast ARP request
4. Target responds with unicast ARP reply
5. Update ARP cache
6. Send frame with destination MAC

**Proxy ARP:**
- Router responds for hosts on other networks
- Enables cross-network communication

## DHCP (DORA) Process

1. **Discovery**: Client broadcasts DHCP Discovery
2. **Offer**: Server sends DHCP Offer with IP
3. **Request**: Client broadcasts DHCP Request
4. **Acknowledgement**: Server sends DHCP ACK

**Ports:** 67 (server), 68 (client)

## ICMP Types

**Error Messages:**
- Type 3: Destination unreachable
- Type 11: Time exceeded (TTL=0)
- Type 12: Parameter problem

**Informational:**
- Type 8: Echo request (Ping)
- Type 0: Echo reply
- Type 5: Redirect
- Type 13/14: Timestamp

## NAT Translation

**Outgoing:**
- Private IP:Port → Public IP:Port

**Incoming:**
- Public IP:Port → Private IP:Port (based on table)

**Types:**
- Static: 1-to-1 permanent mapping
- Dynamic: Pool of public IPs
- PAT: Multiple private to single public (port-based)

## Framing Techniques

**Byte Count:**
- Include data byte count in header
- Problem: If count corrupted, synchronization lost

**Flag Byte Stuffing:**
- Use special bit pattern (flag) for boundaries
- Stuff bits to prevent flag appearing in data
- Example: HDLC uses 01111110

**Character Stuffing:**
- Stuff ESC character before special characters
- Receiver unstuffs after receiving

## IP Fragmentation

**Fields:**
- Identification: Identifies fragments of same packet
- Flags: DF (Don't Fragment), MF (More Fragments)
- Fragment Offset: Position in original packet (8-byte units)

**Reassembly:**
- Only at destination, not at intermediate routers
- All fragments must arrive
- If one missing, entire packet discarded

**When it occurs:**
- Packet size > outgoing link MTU
- Typical MTU: 1500 bytes (Ethernet)

## Socket Programming Quick Steps

**TCP Server:**
1. socket() → bind() → listen() → accept() → read/write() → close()

**TCP Client:**
1. socket() → connect() → read/write() → close()

**UDP Server:**
1. socket() → bind() → recvfrom() → sendto() → close()

**UDP Client:**
1. socket() → sendto() → recvfrom() → close()

## Important Formulas

**CIDR Calculation:**
- Host bits = 32 - prefix
- Total addresses = 2^(host bits)
- Usable = 2^(host bits) - 2

**Slow Start:**
- CWND after n RTTs = 2^n MSS
- Stops when CWND ≥ SST

**Fragment Count:**
- Total = ⌈Data size / (MTU - IP header)⌉
- Max payload = MTU - IP header

**Fragmentation Fields:**
- Fragment Offset in 8-byte units: Offset = (byte position) / 8

## Common Mistakes to Avoid

1. **CIDR Calculation**: Remember -2 for network and broadcast addresses
2. **TCP Sequence Numbers**: SYN consumes sequence number
3. **Fragmentation**: Only happens at source if DF=1 is set, else at intermediate routers
4. **DNS**: Root server doesn't know final IP, only TLD server location
5. **ARP**: ARP request is BROADCAST, reply is UNICAST
6. **Routing**: RIP max is 15 hops, 16 = unreachable
7. **TCP Handshake**: Sequence number in ACK is server's sequence + 1
8. **Window Size**: min(rwnd, cwnd) determines actual data size
9. **MTU**: 1500 bytes Ethernet, minus 20 bytes IP header = 1480 bytes payload max
10. **Error Detection**: CRC detects, Hamming corrects

## High-Weightage Topics for GATE

1. Subnetting and CIDR (/25)
2. TCP 3-way handshake and sequence numbers (/20)
3. Routing algorithms comparison (/15)
4. Error detection methods (/12)
5. DNS resolution process (/12)
6. NAT and port forwarding (/10)
7. Congestion control phases (/10)
8. MAC addressing and bridging (/10)
9. IPv4 fragmentation (/8)
10. ARP and DHCP (/8)

---

**Total: 130/200 marks typically = 65% of exam**

Print this page and review daily during last 2 weeks before exam!
