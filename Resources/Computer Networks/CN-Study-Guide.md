# Computer Networks Complete Study Guide - GATE 2026

## Part 1: Layering - OSI and TCP/IP Protocol Stacks

### 1.1 OSI Model (7 Layers)

The OSI (Open Systems Interconnection) model is a conceptual framework that standardizes the functions of a network system into seven layers, developed by ISO.

#### Layer Structure (Bottom to Top):

**1. Physical Layer (Layer 1)**
- Transmits raw bits over physical media (cables, wireless)
- Converts frames to electrical signals, light pulses, or radio waves
- Concerned with electrical, mechanical, and procedural aspects
- No error detection or correction at this layer

**2. Data Link Layer (Layer 2)**
- Creates frames from raw data
- Uses MAC (Media Access Control) addresses for local network communication
- Handles error detection and correction
- Flow control and framing
- Divided into two sublayers: LLC (Logical Link Control) and MAC

**3. Network Layer (Layer 3)**
- Routes packets between networks using IP addresses
- Logical addressing and routing decisions
- Handles fragmentation and reassembly
- Protocols: IP, ICMP, IGMP

**4. Transport Layer (Layer 4)**
- Ensures reliable end-to-end data delivery
- Manages flow control and error checking
- Connection-oriented (TCP) and connectionless (UDP) protocols
- Segment organization and sequencing

**5. Session Layer (Layer 5)**
- Manages communication sessions between applications
- Handles session establishment, maintenance, and termination
- Dialog control and synchronization

**6. Presentation Layer (Layer 6)**
- Data formatting and conversion
- Encryption and decryption
- Data compression
- Character set translation

**7. Application Layer (Layer 7)**
- Provides network services directly to user applications
- HTTP, FTP, SMTP, DNS, Telnet, SSH
- User interface and application-specific functions

#### OSI Model Advantages:
- Modular structure with clear separation of concerns
- Standardization for interoperability
- Flexibility for independent layer development
- Easy troubleshooting and debugging
- Technology independence

### 1.2 TCP/IP Protocol Stack (4-5 Layers)

The TCP/IP model is a practical, working protocol suite that describes how the Internet actually functions.

#### Layer Structure:

**1. Link Layer (Network Access/Host-to-Network)**
- Combines physical and data link layers of OSI
- Hardware addressing and frame transmission
- Ethernet, PPP, frame relay
- Protocols: MAC, ARP

**2. Internet Layer**
- Corresponds to OSI Network Layer
- IP addressing and routing
- Protocols: IPv4, IPv6, ICMP, IGMP, ARP

**3. Transport Layer**
- Matches OSI Transport Layer exactly
- End-to-end communication and reliability
- Protocols: TCP, UDP, SCTP

**4. Application Layer**
- Combines OSI Session, Presentation, and Application layers
- All user-facing protocols
- Protocols: HTTP, HTTPS, FTP, SMTP, DNS, SSH, Telnet, POP3, IMAP

#### Some models include Internet Protocol Layer as a separate 5th layer

### 1.3 OSI vs TCP/IP Comparison

| Aspect | OSI | TCP/IP |
|--------|-----|--------|
| Layers | 7 layers | 4-5 layers |
| Development | ISO (conceptual) | DoD (practical) |
| Purpose | Reference model | Working model |
| Flexibility | Less flexible in practice | More flexible and extensible |
| Implementation | Theoretical framework | De facto Internet standard |
| Adoption | Educational/standards | Actual implementation |
| Complexity | More detailed | Simplified |
| Protocol Independence | Yes | Tied to specific protocols |

### 1.4 Data Flow Through Layers

- **Downward (Sending)**: Each layer adds its header/control information (encapsulation)
- **Upward (Receiving)**: Each layer removes its header and passes data up (de-encapsulation)
- PDU (Protocol Data Unit) names vary by layer: Data, Segment (TCP), Datagram (UDP), Packet (IP), Frame (Ethernet)

---

## Part 2: Switching Concepts

### 2.1 Circuit Switching

**Characteristics:**
- Dedicated communication channel established before data transmission
- Resources reserved for the entire duration of connection
- Fixed bandwidth allocation
- Three phases: setup, communication, teardown
- Used in traditional telephone networks

**Advantages:**
- Guaranteed bandwidth and latency
- Simple implementation
- Real-time communication suitable

**Disadvantages:**
- Inefficient resource utilization
- Setup time overhead
- Not suitable for bursty data traffic
- Higher cost

### 2.2 Packet Switching (Datagram Approach)

**Characteristics:**
- Data divided into packets/datagrams
- Each packet routed independently
- No pre-established connection required
- Statistical multiplexing of network resources
- Used in modern data networks (Internet)

**Advantages:**
- Efficient bandwidth utilization
- Flexible routing
- No setup delay
- Fault tolerant
- Suitable for bursty traffic

**Disadvantages:**
- No guaranteed delivery order
- Variable latency
- No bandwidth guarantee
- Packet loss possible
- Need for error handling at higher layers

### 2.3 Virtual Circuit Switching

**Characteristics:**
- Combination of circuit and packet switching benefits
- Connection-oriented with logical circuit establishment
- Packets follow the same path once circuit is established
- Used in Frame Relay, ATM, MPLS

#### Three Phases:

**1. Setup Phase**
- Source sends a setup/call request packet to destination
- Switches create entries in their forwarding tables
- Destination sends acknowledgment/call acceptance packet
- Virtual circuit identifier (VCI) assigned to each link
- Conversational parameters negotiated (MSS, path, QoS)

**2. Data Transfer Phase**
- Packets follow the established path
- All packets arrive in order
- VCI used to identify the circuit at each hop
- Flow control and error control mechanisms active

**3. Teardown Phase**
- Source sends special disconnect/teardown packet
- Switches remove corresponding forwarding table entries
- Destination acknowledges
- Resources released

#### Types of Virtual Circuits:

**Permanent Virtual Circuit (PVC)**
- Pre-configured, always available
- No setup delay
- Used for permanent connections
- Example: Leased lines

**Switched Virtual Circuit (SVC)**
- Established on-demand
- Setup delay incurred
- Better resource utilization
- Example: On-demand dial-up

#### Advantages Over Datagram:
- Guaranteed packet ordering and delivery within the circuit
- Flow control across the entire path
- Resource reservation possible
- Lower overhead per packet (VCI instead of full address)

#### Disadvantages:
- Setup overhead
- Less flexible routing
- More complex implementation

---

## Part 3: Data Link Layer

### 3.1 Framing

Framing is the process of organizing data into frames with defined boundaries.

#### Frame Structure:
```
[Preamble] [Start Flag] [Header] [Data/Payload] [Trailer/FCS] [End Flag]
```

#### Framing Techniques:

**1. Byte Count Method**
- Includes count of data bytes in header
- Problem: If count is corrupted, synchronization lost

**2. Flag/Byte Stuffing**
- Uses special bit pattern (flag) to mark frame boundaries
- Stuffing used to prevent flags appearing in data
- Example: HDLC uses 01111110 as flag
- Destuffing removes inserted bits at receiver

**3. Character Count with Stuffing (Combination)**
- Uses both byte count and flags
- More reliable

### 3.2 Error Detection Techniques

#### 1. Parity Check

**Single Parity Bit:**
- Even parity: Total 1's in frame should be even
- Odd parity: Total 1's in frame should be odd
- Detects single-bit errors only
- Can't locate or correct errors

**Example (Even Parity):**
```
Data: 1011001 (4 ones)
Add parity bit: 0 (to make even)
Frame: 01011001 (5 ones - even)
```

#### 2. Checksum (One's Complement)

**Process:**
- Divide data into fixed-size segments (typically 16 bits)
- Add all segments using one's complement arithmetic
- Complement the sum to get checksum
- Append checksum to data

**Verification:**
- Add all segments including checksum
- Result should be all 1's if no error

**Limitations:**
- Cannot detect multiple errors in different positions
- Cannot locate error position

#### 3. Cyclic Redundancy Check (CRC)

**Most Common Method:**
- Uses polynomial division
- Treats bit string as polynomial
- Divides data by generator polynomial
- Remainder is CRC

**Advantages:**
- Detects most single and double-bit errors
- Detects all burst errors up to CRC length
- Industry standard for LANs and WANs
- Better error detection than checksum

**Common Generator Polynomials:**
- CRC-16: Detects errors up to 16 bits
- CRC-32: Detects errors up to 32 bits

**Example (CRC-4 with polynomial x³+x+1):**
```
Generator: 1011
Data + zeros: 1101 000 (4 zeros for CRC-4)
Perform XOR division
CRC value becomes the remainder
```

### 3.3 Error Correction (Forward Error Correction)

- **Hamming Code**: Can correct single-bit errors
- **Reed-Solomon Code**: Corrects multiple errors, used in QR codes
- **Convolutional Code**: Used in wireless communications
- **Low-Density Parity-Check (LDPC)**: Modern approach

**Trade-off:** More bits needed for correction, reduces efficiency

### 3.4 Medium Access Control (MAC)

MAC layer controls how devices access the shared transmission medium.

#### Primary Functions:
- Frame delimiting and recognition
- Destination and source addressing (MAC addresses)
- Frame check sequence generation and verification
- Control of access to physical medium
- Half-duplex retransmission and backoff
- Discard of malformed frames

#### MAC Address (Hardware Address)
- 48-bit address (6 bytes)
- Format: XX:XX:XX:XX:XX:XX (hexadecimal)
- First 24 bits: OUI (Organizationally Unique Identifier) - manufacturer
- Last 24 bits: Vendor assigned

#### MAC Sublayer in Ethernet:
- Append/check Frame Check Sequence (FCS) - 32-bit CRC
- Prepend/remove preamble and Start Frame Delimiter (SFD)
- Add/remove MAC addresses
- Half-duplex compatibility features
- Interframe gap enforcement

### 3.5 Ethernet Bridging and Switching

#### Network Bridge:
- Layer 2 device connecting network segments
- Uses MAC address table (forwarding information base)
- Reduces collision domain
- Improves network efficiency

#### Bridge Learning Process:
1. Switch initially has empty MAC table
2. When frame arrives, switch records source MAC → incoming port
3. Switch looks up destination MAC in table
4. If found: forwards to appropriate port (filtering)
5. If not found: floods to all ports except incoming (flooding)

#### MAC Address Table Functions:
- Dynamic learning from source addresses
- Static entries can be manually configured
- Aging of entries (typically 5 minutes)
- Prevents duplicate entries

#### Switch Functions:
- **Unicast**: Frame sent to known destination port
- **Broadcast**: Frame sent to all ports (destination FF:FF:FF:FF:FF:FF)
- **Multicast**: Frame sent to group of ports
- **Flooding**: Frame sent to all ports when destination unknown

#### Advantages of Switching:
- Increases available bandwidth per port
- Reduces collision domain
- Allows full-duplex communication
- Improves network performance

---

## Part 4: Routing Protocols

### 4.1 Routing Fundamentals

**Routing Table Contents:**
- Destination network/address
- Next hop address
- Metric (cost/distance)
- Interface to use
- Administrative distance

### 4.2 Distance Vector Routing (Bellman-Ford Algorithm)

**Concept:**
- Each router maintains a distance vector
- DV contains distances/costs to all known destinations
- Routers share DV with immediate neighbors only
- Routers update their DV based on neighbors' information

**Bellman-Ford Equation:**
```
Dx(y) = min{c(x,v) + Dv(y)} for all neighbors v of x
```
Where:
- Dx(y) = Least distance from x to destination y
- c(x,v) = Cost of edge from x to neighbor v
- Dv(y) = Distance from v to y (from v's distance vector)

#### Algorithm Steps:
1. Each router initializes distances: 0 for itself, infinity for others
2. Each router sends its distance vector to all immediate neighbors
3. On receiving DV from neighbor, router updates its own DV using Bellman-Ford equation
4. If DV changes, router sends updated DV to neighbors
5. Process continues until convergence (no changes in DV)

#### Iterative Process:
- At t=0: Only direct neighbors known
- At t=1: One-hop paths propagated
- At t=2: Two-hop paths propagated
- Continues until all paths converge

#### Characteristics:
- **Asynchronous**: Each router works independently
- **Distributed**: No central control
- **Iterative**: Computation repeats
- **Convergence**: Eventually reaches stable state

#### Pros:
- Simple implementation
- Distributed operation
- Low computational overhead

#### Cons:
- **Slow Convergence**: Takes time proportional to network diameter
- **Counting to Infinity Problem**: When link fails, routers may indefinitely increase distance
- **Inefficient**: Entire routing table sent periodically
- **Limited Scalability**: Maximum hop count (15 for RIP)

#### Mitigation for Counting to Infinity:
- Maximum hop count (RIP uses 15, unreachable = 16)
- Split Horizon: Don't advertise route back on incoming interface
- Poison Reverse: Advertise route back with infinite metric
- Hold-down Timers: Wait before accepting lower metrics

#### Implementation: RIP (Routing Information Protocol)
- Uses hop count as metric
- Sends full routing table every 30 seconds
- Maximum 15 hops (limits network size)
- RIPv1, RIPv2, RIPng (for IPv6)

### 4.3 Link State Routing

**Concept:**
- Each router learns entire network topology
- Router builds graph of entire network
- Uses Dijkstra's shortest path algorithm
- Triggered updates when topology changes

#### Algorithm Overview:

**Four Steps:**

1. **Link State Advertisement**
   - Each router creates Link State Packet (LSP)
   - Contains list of directly connected neighbors with costs
   - Sends to all other routers

2. **Flooding**
   - LSP flooded throughout network
   - Each router forwards LSP to all neighbors except incoming
   - All routers end up with identical topology information
   - Uses sequence numbers to prevent infinite flooding

3. **Shortest Path Tree Formation**
   - Each router runs Dijkstra algorithm
   - Calculates shortest path to all destinations
   - Builds shortest path tree rooted at itself

4. **Routing Table Computation**
   - From SPT, extract next hop for each destination
   - Populate routing table

#### Dijkstra's Algorithm:

```
Initialize:
  SPT = {source}
  For all other nodes: distance = infinity
  distance[source] = 0

While SPT doesn't contain all nodes:
  1. Find node u NOT in SPT with minimum distance
  2. Add u to SPT
  3. For each neighbor v of u NOT in SPT:
     if distance[u] + cost(u,v) < distance[v]:
       distance[v] = distance[u] + cost(u,v)
       parent[v] = u
```

#### Characteristics:
- **Fast Convergence**: Updates only when topology changes
- **Efficient**: No full routing table periodic updates
- **Accurate**: Based on complete topology
- **Scalable**: Can handle larger networks

#### Pros:
- Fast convergence
- Only sends updates on topology changes
- More efficient bandwidth usage
- Lower overhead
- Better scalability

#### Cons:
- Higher computational overhead (Dijkstra algorithm)
- More complex implementation
- Higher memory requirements (storing full topology)
- Requires more processing power

#### Implementation: OSPF (Open Shortest Path First)
- Uses cost/metric instead of hop count
- Can have more hops in larger networks
- Hierarchical routing with areas
- Triggered updates
- Support for equal-cost multipath
- OSPFv2 (IPv4), OSPFv3 (IPv6)

### 4.4 Other Routing Concepts

#### Flooding Algorithm:
- Simple routing where router forwards to all ports except incoming
- No routing table needed
- Guarantees packet reaches destination if path exists
- Used in broadcast scenarios
- Inefficient for unicast (creates loops and duplication)

#### Shortest Path Routing:
- General concept: minimize total cost to destination
- Dijkstra (link-state) and Bellman-Ford (distance-vector) implement this
- Cost can be delay, hop count, bandwidth, reliability, or combination

---

## Part 5: IP Addressing, CIDR, and Fragmentation

### 5.1 IPv4 Addressing

#### Address Format:
- 32-bit address
- Divided into 4 octets (bytes)
- Dotted decimal notation: A.B.C.D (each 0-255)
- Example: 192.168.1.1

#### Address Components:
- **Network Portion**: Identifies the network
- **Host Portion**: Identifies specific host on network

#### Traditional Classful Addressing:

| Class | Range | Network Bits | Host Bits | Network Count | Hosts/Network |
|-------|-------|--------------|-----------|---------------|---------------|
| A | 1-126 | 8 | 24 | 126 | 16.7M |
| B | 128-191 | 16 | 16 | 16,384 | 65,534 |
| C | 192-223 | 24 | 8 | 2M | 254 |
| D | 224-255 | (Multicast) | - | - | - |
| E | 240-255 | (Reserved) | - | - | - |

**Problems with Classful:**
- Inefficient use of address space
- Class C too small, Class B too large for many organizations
- IP address exhaustion

### 5.2 CIDR (Classless Inter-Domain Routing)

**Notation:** IP_ADDRESS/PREFIX_LENGTH
- Example: 192.168.1.0/24
- /24 means first 24 bits are network, remaining 8 are host
- Also called "slash 24" notation

#### CIDR Calculation:

For 192.168.1.0/24:
- Network bits: 192.168.1 (first 24 bits)
- Host bits: .0-255 (last 8 bits)
- Total addresses: 2^8 = 256
- Usable hosts: 256 - 2 (network and broadcast) = 254

#### Common CIDR Prefix Lengths:

| CIDR | Subnet Mask | Total Hosts | Usable Hosts |
|------|-------------|------------|--------------|
| /32 | 255.255.255.255 | 1 | 1 |
| /30 | 255.255.255.252 | 4 | 2 |
| /28 | 255.255.255.240 | 16 | 14 |
| /27 | 255.255.255.224 | 32 | 30 |
| /26 | 255.255.255.192 | 64 | 62 |
| /25 | 255.255.255.128 | 128 | 126 |
| /24 | 255.255.255.0 | 256 | 254 |
| /23 | 255.255.254.0 | 512 | 510 |
| /22 | 255.255.252.0 | 1,024 | 1,022 |
| /21 | 255.255.248.0 | 2,048 | 2,046 |
| /20 | 255.255.240.0 | 4,096 | 4,094 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /8 | 255.0.0.0 | 16,777,216 | 16,777,214 |

#### Subnetting Example:
Network: 192.168.0.0/22
- Usable IPs: 1022
- To create 4 subnets of /24 each:
  - 192.168.0.0/24 (0-255)
  - 192.168.1.0/24 (256-511)
  - 192.168.2.0/24 (512-767)
  - 192.168.3.0/24 (768-1023)

#### Supernetting (Route Aggregation):
- Combining multiple networks into larger block
- Reduces routing table entries
- Example: 192.168.0.0/24 and 192.168.1.0/24 combine to 192.168.0.0/23

### 5.3 IP Fragmentation

**Need for Fragmentation:**
- Different networks have different Maximum Transmission Unit (MTU)
- Typical Ethernet MTU: 1500 bytes
- Packet exceeding MTU must be fragmented
- Example: Tunnel with smaller MTU, WAN links

#### Fragmentation Fields in IPv4 Header:
- **Identification**: Identifies fragments of same packet
- **Flags**: 3 bits
  - Reserved (0)
  - Do Not Fragment (DF)
  - More Fragments (MF)
- **Fragment Offset**: Position of fragment in original packet (in 8-byte units)

#### Fragmentation Process:
1. Router receives packet larger than outgoing link MTU
2. DF flag checked: if set, drop packet and send ICMP error
3. If DF=0, divide data into fragments ≤ MTU
4. Each fragment gets:
   - Same identification number as original
   - Sequential fragment offset
   - MF=1 for all except last fragment
   - Same source and destination IP
   - IP header plus fragmented data

#### Reassembly:
- Only occurs at final destination (not at intermediate routers)
- Destination waits for all fragments (identified by ID)
- Reassembles based on fragment offset
- If fragment missing, entire packet discarded

#### Issues:
- Fragmentation reduces efficiency
- Loss of one fragment requires retransmission of entire packet
- Processing overhead
- Path MTU Discovery (PMTUD) helps avoid fragmentation
  - Uses ICMP to discover minimum MTU on path
  - Sends packets with DF bit set
  - Adjusts packet size based on ICMP feedback

---

## Part 6: IP Support Protocols

### 6.1 ARP (Address Resolution Protocol)

**Purpose:** Map IP addresses to MAC addresses

#### Scenario:
- Source knows destination IP but not MAC address
- Cannot send frame without MAC address
- ARP resolves IP → MAC mapping

#### ARP Request/Reply Process:

1. **ARP Cache Check**
   - Source checks local ARP cache
   - If match found, use cached MAC
   - If not found, send ARP request

2. **ARP Request Broadcast**
   - Source broadcasts ARP request to entire LAN
   - "Who has IP X.X.X.X? Tell me your MAC address"
   - Request contains: sender IP, sender MAC, target IP

3. **Processing**
   - All devices receive broadcast
   - Each device checks if target IP matches its own
   - Only matching device responds

4. **ARP Reply (Unicast)**
   - Target device sends unicast ARP reply
   - Contains: target MAC, target IP, sender's IP
   - Only sender receives reply

5. **Cache Update**
   - Source caches new MAC-to-IP mapping
   - Uses for future communications
   - Typically cached for 4-20 minutes

#### ARP Packet Format:
- Hardware Type (2 bytes): Ethernet = 1
- Protocol Type (2 bytes): IPv4 = 0x0800
- Hardware Address Length (1 byte): 6 for Ethernet
- Protocol Address Length (1 byte): 4 for IPv4
- Operation Code (2 bytes): 1=Request, 2=Reply
- Sender Hardware Address (6 bytes): MAC of sender
- Sender Protocol Address (4 bytes): IP of sender
- Target Hardware Address (6 bytes): MAC of target (empty in request)
- Target Protocol Address (4 bytes): IP of target

#### Types of ARP:
- **Unicast ARP**: Replied with unicast
- **Broadcast ARP**: Used for standard requests
- **Gratuitous ARP**: Host announces its own IP-MAC mapping
  - IP sender = IP target = host's own IP
  - Used for duplicate detection or cache update

#### Proxy ARP:
- Router replies to ARP request for host on different segment
- Router's MAC returned for IP on remote network
- Allows communication across router without routing changes

#### Security Issues:
- **ARP Spoofing**: Attacker sends false ARP replies
- **ARP Poisoning**: Corrupts ARP caches on network
- Mitigation: Static ARP entries, ARP monitoring, authentication

### 6.2 DHCP (Dynamic Host Configuration Protocol)

**Purpose:** Automatically assign IP addresses and network configuration

#### Benefits:
- Eliminates manual IP configuration
- Centralized management
- Reduces configuration errors
- Efficient use of address space (reuse of addresses)

#### DHCP Components:
- **DHCP Server**: Maintains pool of available IPs
- **DHCP Client**: Host requesting configuration
- **DHCP Relay**: Forwards DHCP messages across networks

#### DHCP Lease Phases (4 Steps - DORA):

**1. DHCP Discovery (Broadcast)**
- Client: Broadcasts DHCP Discovery message
- Source IP: 0.0.0.0 (client has no IP yet)
- Destination: 255.255.255.255 (broadcast)
- Message: "I'm a new client, anyone offering IP addresses?"

**2. DHCP Offer (Broadcast)**
- Server: Responds with DHCP Offer
- Proposed IP address, lease duration, gateway, DNS servers
- Destination: Broadcast (client still 0.0.0.0)
- May receive multiple offers from different servers

**3. DHCP Request (Broadcast)**
- Client: Selects one offer and broadcasts DHCP Request
- Message: "I accept the offer from this server"
- Destination: Broadcast (other servers informed of selection)

**4. DHCP Acknowledgement (Unicast)**
- Server: Sends DHCP ACK
- Confirms all configuration parameters
- Lease time specified
- Now client has IP and configuration

#### DHCP Configuration Parameters:
- IP Address
- Subnet Mask
- Gateway/Default Router
- DNS Server addresses
- Lease Time (typically 24-48 hours)
- Other options: NTP server, WINS server, etc.

#### Lease Renewal:
- At 50% lease time: Client sends DHCP Request to original server
- If server responds: Lease renewed
- If server doesn't respond: Client waits until 87.5% time
- Then broadcasts renewal request
- At 100%: Lease expires, client must release IP

#### DHCP Options:
- Option 1: Subnet Mask
- Option 3: Router
- Option 6: DNS Server
- Option 51: Lease Time
- Option 58: Renewal Time
- Option 59: Rebinding Time

### 6.3 ICMP (Internet Control Message Protocol)

**Purpose:** Send error and informational messages in IP network

#### ICMP Message Types:

**Error Messages:**
1. **Destination Unreachable** (Type 3)
   - Codes: Host unreachable, network unreachable, port unreachable
   - Router can't reach destination network or host drops packet

2. **Source Quench** (Type 4)
   - Network congestion indication
   - Request to slow down transmission (rarely used now)

3. **Time Exceeded** (Type 11)
   - Codes: TTL expired, fragment reassembly timeout
   - Triggered when TTL reaches 0

4. **Parameter Problem** (Type 12)
   - Invalid IP header field detected
   - Points to problem field location

**Informational Messages:**
1. **Echo Request** (Type 8)
   - Ping request
   - Asks for echo reply

2. **Echo Reply** (Type 0)
   - Ping response
   - Returns received data

3. **Timestamp Request/Reply** (Types 13/14)
   - Request current time from host
   - Host responds with timestamp

4. **Address Mask Request/Reply** (Types 17/18)
   - Request subnet mask of network
   - Less commonly used now (DHCP preferred)

5. **Redirect** (Type 5)
   - Router informs host of better route
   - "Use this other router for destination X"

#### ICMP Header Format:
- Type (1 byte): Message type
- Code (1 byte): Specific message
- Checksum (2 bytes): 16-bit checksum
- Rest of Header (4 bytes): Type-specific data
- Data Section: Original IP packet (for error messages)

#### Common ICMP Usage:
- **Ping**: Connectivity testing (Echo Request/Reply)
- **Traceroute**: Route path discovery (TTL Exceeded messages)
- **Path MTU Discovery**: Finding smallest MTU on path

#### Characteristics:
- Connectionless, unreliable
- No flow control or congestion control
- Encapsulated in IP datagrams
- Not for user data transmission

### 6.4 NAT (Network Address Translation)

**Purpose:** Allow hosts with private IPs to communicate with external networks

#### Private IP Addresses (RFC 1918):
- Class A: 10.0.0.0/8
- Class B: 172.16.0.0/12
- Class C: 192.168.0.0/16
- Not routable on Internet

#### NAT Translation Process:

**Outgoing Packet:**
1. Internal host sends packet with:
   - Source: Private IP (e.g., 192.168.1.10)
   - Destination: External IP (e.g., 8.8.8.8)

2. NAT device intercepts packet:
   - Creates mapping entry in translation table
   - Replaces source IP with public IP
   - May replace source port (for PAT)

3. External server receives:
   - Source: Public IP (NAT device)
   - Destination: 8.8.8.8

**Incoming Response:**
1. External server sends response:
   - Source: 8.8.8.8
   - Destination: Public IP (NAT device)

2. NAT device:
   - Looks up translation table using destination port/IP
   - Finds corresponding private IP
   - Replaces destination with private IP

3. Internal host receives:
   - Source: 8.8.8.8
   - Destination: 192.168.1.10 (its private IP)

#### Types of NAT:

**1. Static NAT**
- One-to-one mapping between private and public IP
- Permanent or semi-permanent assignment
- All connections from specific private IP use same public IP

**2. Dynamic NAT**
- Pool of public IPs mapped to private hosts
- Different public IP each time host connects
- When host finishes, public IP returns to pool

**3. PAT (Port Address Translation) / NAT Overload**
- Many private IPs map to single public IP
- Distinction using port numbers
- Translation table: [Private IP:Port] ↔ [Public IP:Port]
- Most common in home networks

#### Translation Table:
| Private IP:Port | Public IP:Port | Remote IP:Port | Status |
|-----------------|----------------|----------------|--------|
| 192.168.1.10:5000 | 203.0.113.1:5000 | 8.8.8.8:80 | Active |
| 192.168.1.11:5001 | 203.0.113.1:5001 | 8.8.8.8:80 | Active |

#### Advantages:
- Conserves public IP addresses
- Hides internal network topology (security)
- Allows private network behind single public IP
- Solves IPv4 address shortage

#### Disadvantages:
- Breaks end-to-end communication model
- Incompatible with some protocols
- Increases latency (translation overhead)
- Complicates troubleshooting
- Stateful (consumes memory for translations)

#### Port Forwarding:
- Special case of NAT
- External port on public IP mapped to specific internal port/host
- Allows external access to internal service
- Example: 203.0.113.1:80 → 192.168.1.10:8080

---

## Part 7: Transport Layer

### 7.1 UDP (User Datagram Protocol)

**Characteristics:**
- Connectionless protocol
- Unreliable (no delivery guarantee)
- Datagram-based (self-contained packets)
- No flow control or congestion control
- Minimal header overhead (8 bytes)
- Stateless operation

#### UDP Header Format (8 bytes):
- Source Port (2 bytes)
- Destination Port (2 bytes)
- Length (2 bytes): Total length of header + data
- Checksum (2 bytes): Optional (0 means no checksum)

#### UDP Characteristics:
- **Connectionless**: No handshake, no connection state
- **Unreliable**: No acknowledgment, no retransmission
- **Unordered**: Packets may arrive out of order
- **Broadcast/Multicast**: Supports group communication
- **Low Latency**: Minimal overhead and processing
- **Lightweight**: Suitable for resource-constrained devices

#### UDP Applications:
- **Real-time applications**: VoIP, video conferencing, online gaming
- **Streaming**: Audio/video streaming
- **DNS**: Fast, single-query/response
- **SNMP**: Network monitoring
- **NTP**: Time synchronization
- **DHCP**: Network configuration
- Any application where speed > reliability

#### Advantages:
- Low latency
- Low overhead
- Supports broadcast and multicast
- Simple implementation
- Fast transmission

#### Disadvantages:
- No reliability guarantees
- Application must handle errors and retransmissions
- No flow control (can overwhelm receiver)
- No congestion control (ignores network congestion)
- Packets may be lost, duplicated, or reordered

### 7.2 TCP (Transmission Control Protocol)

**Characteristics:**
- Connection-oriented protocol
- Reliable delivery (in-order, no duplicates)
- Stream-based (continuous byte stream)
- Flow control and congestion control
- 20-byte header minimum
- Maintains connection state

#### TCP Header Format (20 bytes minimum):
- Source Port (2 bytes)
- Destination Port (2 bytes)
- Sequence Number (4 bytes)
- Acknowledgement Number (4 bytes)
- Data Offset (4 bits): TCP header size
- Flags (12 bits): SYN, ACK, FIN, RST, PSH, URG, etc.
- Window Size (2 bytes): Flow control information
- Checksum (2 bytes): Header + data
- Urgent Pointer (2 bytes)
- Options (variable): MSS, timestamp, window scaling, etc.

#### TCP Characteristics:
- **Connection-oriented**: 3-way handshake establishes connection
- **Reliable**: Acknowledgments and retransmissions
- **Ordered**: Sequence numbers maintain packet order
- **Stream-based**: Data treated as continuous flow
- **Flow-controlled**: Sliding window mechanism
- **Congestion-aware**: Adapts to network conditions

### 7.3 TCP Connection Establishment (3-Way Handshake)

**Step 1: SYN (Synchronization)**
- Client → Server
- SYN flag = 1
- Sequence Number (Seq) = random initial sequence (e.g., 521)
- Includes MSS (Maximum Segment Size): e.g., 1460 bytes
- No data payload

**Step 2: SYN-ACK**
- Server → Client
- SYN flag = 1, ACK flag = 1
- Sequence Number (Seq) = random sequence (e.g., 2000)
- Acknowledgement Number (Ack) = client's Seq + 1 (e.g., 522)
- MSS from server: e.g., 500 bytes (both use minimum: 500)

**Step 3: ACK (Acknowledgement)**
- Client → Server
- ACK flag = 1, SYN flag = 0
- Sequence Number (Seq) = client's previous Seq + 1 (e.g., 522)
- Acknowledgement Number (Ack) = server's Seq + 1 (e.g., 2001)
- Can include data payload

**Result:** Connection established, both sides synchronized

### 7.4 TCP Connection Termination (4-Way Handshake)

**Step 1: FIN**
- One side → Other side
- FIN flag = 1
- Indicates no more data to send

**Step 2: ACK**
- Other side → First side
- ACK flag = 1
- Acknowledges receipt of FIN

**Step 3: FIN**
- Other side → First side
- FIN flag = 1
- Indicates its done sending

**Step 4: ACK**
- First side → Other side
- ACK flag = 1
- Confirms FIN receipt

**Note:** FIN-WAIT-2 state waits for FIN from other side

### 7.5 Flow Control

**Purpose:** Ensure sender doesn't overwhelm receiver

#### Mechanism: Sliding Window

**Window Concept:**
- Receiver advertises window size in TCP header
- Indicates how much data it can accept
- Sender can send data up to window size
- As receiver processes data, window slides forward

**Example:**
```
Receiver buffer size: 64KB
Initially: window = 64KB
Sender sends 40KB
Remaining window: 24KB
Receiver processes 40KB
Window slides: window = 64KB again
```

**Flow Control Variables:**
- **Last Byte Acknowledged (LBA)**: Up to what has been ACKed
- **Last Byte Sent (LBS)**: Last byte actually sent
- **Last Byte Received (LBR)**: Last byte receiver got
- **Last Byte Read (LBRd)**: What application has read

**Window Management:**
- Receiver continuously sends ACKs with updated window size
- Sender limited by: rwnd (receiver window) and cwnd (congestion window)
- If rwnd = 0: Sender waits (receiver buffer full)
- ACKs serve dual purpose: confirm receipt + update window

### 7.6 Congestion Control

**Purpose:** Prevent network overload when routers overwhelmed

**Congestion Indicators:**
- Timeout (packet lost due to congestion)
- Duplicate ACK (packet likely lost)
- RTO (Retransmission Timeout) expiration

#### Congestion Control Algorithms:

**1. Slow Start**
- Initial phase of connection
- CWND starts at 1 MSS (Max Segment Size)
- CWND increases by 1 MSS for each ACK received
- Exponential growth: 1, 2, 4, 8, 16... MSS
- Continues until:
  - CWND reaches Slow Start Threshold (SST)
  - Packet loss occurs
  - Timeout happens

**2. Congestion Avoidance**
- After reaching SST
- CWND increases by 1 MSS per round-trip time (RTT)
- Linear growth instead of exponential
- Continues until packet loss or timeout

**3. Congestion Detection**
- Triggered by packet loss
- Two types of detection:
  - **Timeout**: Significant congestion
    - SST = CWND / 2
    - CWND = 1 MSS
    - Restart slow start
  - **3 Duplicate ACKs**: Mild congestion (fast retransmit)
    - SST = CWND / 2
    - CWND = SST
    - Resume congestion avoidance

#### CWND Evolution:
```
Slow Start: CWND = 1, 2, 4, 8, 16... until SST
Congestion Avoidance: CWND increases by 1 per RTT
Loss detected: SST = CWND/2, CWND = 1 (timeout)
              or CWND = SST (3 dup ACKs)
```

**Difference from Flow Control:**
- **Flow Control**: Receiver-based (rwnd), prevents overwhelming receiver
- **Congestion Control**: Network-based (cwnd), prevents overwhelming network
- **Actual window**: min(rwnd, cwnd)

### 7.7 TCP vs UDP Comparison

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable delivery | Unreliable |
| Ordering | In-order delivery | No ordering guarantee |
| Speed | Slower (more overhead) | Faster (minimal overhead) |
| Header Size | 20 bytes minimum | 8 bytes fixed |
| Flow Control | Yes (sliding window) | No |
| Congestion Control | Yes (Slow start, etc.) | No |
| Error Detection | Yes | Optional checksum |
| Streaming | Stream-based | Datagram-based |
| Broadcast/Multicast | No | Yes |
| Use Case | Reliable transfer | Real-time, speed-critical |
| Examples | HTTP, HTTPS, FTP, SMTP | DNS, VoIP, gaming, streaming |

### 7.8 Socket Programming

**Socket:** Endpoint for network communication

#### TCP Socket Programming

**Server Side:**
1. **socket()**: Create socket
   ```
   socket(AF_INET, SOCK_STREAM)
   AF_INET = IPv4, SOCK_STREAM = TCP
   ```

2. **bind()**: Bind socket to address and port
   ```
   bind(socket, address, port)
   Specifies which port server listens on
   ```

3. **listen()**: Mark socket as accepting connections
   ```
   listen(socket, backlog)
   Backlog = queue for pending connections
   ```

4. **accept()**: Accept incoming client connection
   ```
   new_socket = accept(socket)
   Blocks until client connects
   Returns new socket for communication
   ```

5. **read()/write()**: Communicate with client
   ```
   data = read(socket)
   write(socket, response)
   ```

6. **close()**: Close connection
   ```
   close(socket)
   ```

**Client Side:**
1. **socket()**: Create socket
   ```
   socket(AF_INET, SOCK_STREAM)
   ```

2. **connect()**: Connect to server
   ```
   connect(socket, server_address, port)
   Initiates 3-way handshake
   ```

3. **write()/read()**: Send/receive data
   ```
   write(socket, message)
   response = read(socket)
   ```

4. **close()**: Close connection
   ```
   close(socket)
   ```

#### UDP Socket Programming

**Server Side:**
1. **socket()**: Create UDP socket
   ```
   socket(AF_INET, SOCK_DGRAM)
   SOCK_DGRAM = UDP
   ```

2. **bind()**: Bind to address and port
   ```
   bind(socket, address, port)
   ```

3. **recvfrom()**: Receive datagram and sender info
   ```
   data, sender_addr = recvfrom(socket, buffer_size)
   Blocks until datagram arrives
   ```

4. **sendto()**: Send datagram to specific address
   ```
   sendto(socket, message, receiver_address)
   ```

5. **close()**: Close socket

**Client Side:**
1. **socket()**: Create UDP socket
   ```
   socket(AF_INET, SOCK_DGRAM)
   ```

2. **sendto()**: Send datagram to server
   ```
   sendto(socket, message, server_address)
   No connection established
   ```

3. **recvfrom()**: Receive response
   ```
   data, server_addr = recvfrom(socket, buffer_size)
   ```

4. **close()**: Close socket

#### Key Differences:
- **TCP**: Uses read/write after connection; connection-oriented
- **UDP**: Uses sendto/recvfrom; no connection; addresses specified each time

---

## Part 8: Application Layer Protocols

### 8.1 DNS (Domain Name System)

**Purpose:** Translate human-readable domain names to IP addresses

#### DNS Hierarchy:

**1. Root Name Servers**
- Top level of hierarchy
- Know addresses of TLD servers
- 13 root servers worldwide
- Respond to queries for TLD servers

**2. Top-Level Domain (TLD) Servers**
- Manage domains like .com, .org, .in, .edu
- Know addresses of authoritative servers for each domain
- Example: .com TLD server knows address of geeksforgeeks.com's authoritative server

**3. Authoritative Name Servers**
- Specific to each domain
- Contain actual DNS records for that domain
- Authoritative answer indicated by AA (Authoritative Answer) flag

**4. Local/Recursive Resolver**
- Typically provided by ISP or network
- Performs recursive queries on behalf of client
- Caches results for faster future queries

#### DNS Resolution Process (Recursive Query):

1. **User enters URL** in browser
2. **Browser checks cache** for domain
   - If found, use cached IP
   - If not found, contact resolver

3. **Resolver queries Root Server**
   - "Where is geeksforgeeks.org?"
   - Root server responds with TLD server address

4. **Resolver queries TLD Server**
   - "Where is geeksforgeeks.org?"
   - TLD server responds with authoritative server address

5. **Resolver queries Authoritative Server**
   - "What's the IP for geeksforgeeks.org?"
   - Authoritative server responds with IP address

6. **Resolver returns IP to Browser**
   - Browser now has IP for geeksforgeeks.org
   - Browser can connect to web server

#### DNS Query Types:

**Recursive Query** (client to resolver)
- Client requests full resolution
- Resolver must find answer
- Most common from end users

**Iterative Query** (resolver to servers)
- Server doesn't have to find answer
- Responds with best information it has
- Used between resolvers and authoritative servers

#### DNS Message Format:

**Header (12 bytes):**
- Transaction ID (2 bytes): Matches queries with responses
- Flags (2 bytes):
  - QR: Query (0) or Response (1)
  - OPCODE: Query type (standard, inverse, status)
  - AA: Authoritative Answer
  - TC: Truncation (message too long)
  - RD: Recursion Desired
  - RA: Recursion Available
  - Z: Reserved
  - RCODE: Response code (0=no error, 3=no such domain)
- Question count (2 bytes)
- Answer RR count (2 bytes)
- Authority RR count (2 bytes)
- Additional RR count (2 bytes)

**Sections:**
- **Question Section**: Domain name and query type
- **Answer Section**: Resource records answering query
- **Authority Section**: Authoritative name servers
- **Additional Section**: Additional helpful information

#### Common DNS Record Types:

| Record | Purpose | Example |
|--------|---------|---------|
| A | IPv4 address | geeksforgeeks.org → 185.199.109.153 |
| AAAA | IPv6 address | geeksforgeeks.org → 2607:f8b0:4004:812::200e |
| CNAME | Alias to another domain | www → geeksforgeeks.org |
| MX | Mail exchanger | Route email to mail server |
| NS | Name server | Points to authoritative server |
| SOA | Start of Authority | Zone information |
| TXT | Text records | SPF, DKIM, verification |
| PTR | Reverse DNS | IP to domain name |
| SRV | Service | Service location info |

#### DNS Caching:
- Resolver caches results
- Each record has TTL (Time To Live)
- Browsers also cache DNS results
- Reduces number of queries to authoritative servers

#### DNS Protocol:
- Uses **UDP port 53** for queries and responses (primary)
- Uses **TCP port 53** for zone transfers (secondary)
- Fast, connectionless for most operations
- TCP used when response too large or for zone transfers

### 8.2 SMTP (Simple Mail Transfer Protocol)

**Purpose:** Send emails from clients to mail servers and between mail servers

#### SMTP Components:
- **Mail User Agent (MUA)**: Email client (Outlook, Gmail, etc.)
- **Mail Transfer Agent (MTA)**: SMTP server (sends/relays email)
- **Mail Delivery Agent (MDA)**: Local delivery to mailbox
- **Mail Access Protocol**: Retrieves email (POP3, IMAP)

#### Email Delivery Process:

1. **User composes email** in MUA
2. **MUA connects to SMTP server** on port 25 or 587
3. **SMTP server** relays email to recipient's SMTP server
4. **Recipient's SMTP server** passes to MDA
5. **MDA stores** in recipient's mailbox
6. **MUA (recipient)** retrieves using POP3/IMAP

#### SMTP Connection Phases:

**Phase 1: Connection Establishment**
- Client connects to SMTP server (port 25, 465, or 587)
- Server responds: 220 <domain> ESMTP <server>
- Client initiates with: HELO or EHLO (Extended HELO)

**Phase 2: Message Submission**
- **MAIL FROM**: Sender email address
  ```
  MAIL FROM:<sender@example.com>
  Server response: 250 OK
  ```
- **RCPT TO**: Recipient email address (can repeat for multiple)
  ```
  RCPT TO:<recipient@example.com>
  Server response: 250 OK
  ```
- **DATA**: Begin message data
  ```
  DATA
  Server response: 354 End data with <CR><LF>.<CR><LF>
  ```
- **Message body** with headers (From, To, Subject, etc.)
- **Dot on line by itself** terminates message
  ```
  .
  Server response: 250 Message queued
  ```

**Phase 3: Connection Termination**
- **QUIT**: Close connection
  ```
  QUIT
  Server response: 221 Bye
  ```

#### SMTP Message Format:

**Headers:**
- **From**: Sender's email address
- **To**: Recipient's email address
- **Subject**: Email subject
- **Date**: When sent
- **Cc/Bcc**: Carbon copy recipients
- Custom headers possible

**Body:** Message content (plain text or HTML)

#### SMTP Characteristics:
- **Push Protocol**: Client pushes data to server (unlike POP/IMAP)
- **TCP Port 25**: Standard SMTP (unencrypted)
- **TCP Port 465**: SMTPS (SMTP over SSL/TLS)
- **TCP Port 587**: Submission port (encrypted)
- **Store-and-Forward**: Email stored at intermediate servers if destination unavailable
- **Relay**: MTAs relay email for other MTAs

#### Error Responses:
- **2xx**: Success (220, 250, 354, etc.)
- **4xx**: Temporary failure (try again later)
- **5xx**: Permanent failure (don't retry)

### 8.3 FTP (File Transfer Protocol)

**Purpose:** Transfer files between networked computers

#### FTP Architecture:

**Two TCP Connections:**
1. **Control Connection** (Port 21)
   - Command/response channel
   - Persistent throughout session
   - Authentication, commands, responses

2. **Data Connection** (Port 20)
   - File data transfer
   - Separate connection for each transfer
   - Closes after each file transfer
   - Opens on demand

#### FTP Mode Types:

**Active Mode (Default):**
1. Client initiates control connection to server (port 21)
2. Client sends PORT command with IP:port to listen on
3. Server connects back to client on specified port for data transfer
4. After transfer, data connection closes
5. Control connection remains open

**Passive Mode (Firewall-friendly):**
1. Client initiates control connection
2. Client sends PASV command
3. Server responds with IP:port where server will listen
4. Client connects to server's port for data transfer
5. More reliable with firewalls/NAT

#### FTP Commands:

| Command | Purpose |
|---------|---------|
| USER | Provide username |
| PASS | Provide password |
| LIST | List files in directory |
| NLST | Non-verbose file list |
| CWD | Change working directory |
| PWD | Print working directory |
| RETR | Retrieve (download) file |
| STOR | Store (upload) file |
| DELE | Delete file |
| RMD | Remove directory |
| MKD | Make directory |
| RNFR/RNTO | Rename file |
| TYPE | Set transfer type (ASCII/binary) |
| PORT | Specify data port (active mode) |
| PASV | Enter passive mode |
| QUIT | Close connection |

#### FTP Data Transfer Modes:

**1. Stream Mode (Default)**
- Data sent as continuous stream of bytes
- TCP handles segmentation
- No special markers
- Efficient for standard files

**2. Block Mode**
- Data sent in blocks with 3-byte header
- Each block contains:
  - Descriptor code (control info)
  - Byte count
  - Data
- Used for structured data

**3. Compressed Mode**
- Data compressed before transfer
- Reduces bandwidth
- Uncommon in practice

#### FTP File Types:

**ASCII Mode:**
- Text files
- Line endings converted (CRLF ↔ LF)
- Platform-specific line endings

**Binary Mode:**
- All file types
- No conversion
- Preserves exact bytes
- Used for images, executables, archives

#### FTP Session Example:
```
USER john
331 Password required
PASS password
230 Login successful
TYPE I
200 Type set to BINARY
PASV
227 Entering Passive Mode (192,168,1,1,20,20)
RETR myfile.zip
150 File status ok, opening data connection
226 Transfer complete
QUIT
```

#### FTP Characteristics:
- **Plaintext authentication**: Credentials sent unencrypted
- **Stateful**: Server maintains connection state
- **Two connections**: Control and data
- **Slow**: Not optimized for modern networks
- **Firewall issues**: Difficult with firewalls/NAT

#### Variants:
- **SFTP**: FTP over SSH (secure, encrypted)
- **FTPS**: FTP over SSL/TLS
- **SCP**: Secure copy using SSH

### 8.4 HTTP (Hypertext Transfer Protocol)

**Purpose:** Transfer web pages and resources over the internet

#### HTTP Characteristics:
- **Stateless**: Server doesn't maintain client state
- **Connectionless**: Connection closes after each request/response
- **Request-Response**: Client initiates, server responds
- **Port 80**: Default HTTP port
- **Port 443**: HTTPS (HTTP over SSL/TLS)
- **Text-based**: Human-readable protocol

#### HTTP Methods:

| Method | Purpose | Body | Safe | Idempotent |
|--------|---------|------|------|-----------|
| GET | Retrieve resource | No | Yes | Yes |
| POST | Create/Submit data | Yes | No | No |
| PUT | Replace resource | Yes | No | Yes |
| DELETE | Delete resource | No | No | Yes |
| PATCH | Partial update | Yes | No | No |
| HEAD | Like GET, no body | No | Yes | Yes |
| OPTIONS | Query available methods | No | Yes | Yes |

#### HTTP Status Codes:

**1xx: Informational**
- 100: Continue
- 101: Switching protocols

**2xx: Success**
- 200: OK
- 201: Created
- 204: No Content
- 206: Partial Content

**3xx: Redirection**
- 300: Multiple Choices
- 301: Moved Permanently
- 302: Found (temporary)
- 304: Not Modified (cached)
- 307: Temporary Redirect

**4xx: Client Error**
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 405: Method Not Allowed

**5xx: Server Error**
- 500: Internal Server Error
- 501: Not Implemented
- 502: Bad Gateway
- 503: Service Unavailable
- 504: Gateway Timeout

#### HTTP Request Format:
```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Accept-Language: en-US
Connection: keep-alive

```

**Request Line:** Method, URI, HTTP version
**Headers:** Additional information
**Blank Line:** Separates headers from body
**Body:** Optional (for POST, PUT)

#### HTTP Response Format:
```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234
Connection: keep-alive

<html>
...page content...
</html>
```

**Status Line:** HTTP version, status code, reason phrase
**Headers:** Response metadata
**Blank Line:** Separates headers from body
**Body:** Response content

#### HTTP Versions:

**HTTP/1.0:**
- Connection closes after each request/response
- Multiple requests require multiple connections
- Inefficient

**HTTP/1.1 (Persistent Connections):**
- Keeps connection open by default
- Multiple requests on same connection
- Pipelining (send multiple requests before responses)
- Chunked transfer encoding
- Host header required

**HTTP/2:**
- Binary framing (not text)
- Multiplexing (multiple requests over one connection)
- Server push
- Header compression
- Better performance

**HTTP/3:**
- Based on QUIC protocol
- UDP-based instead of TCP
- Faster connection establishment
- Better for lossy networks

---

## Part 9: Important GATE Exam Concepts

### 9.1 Common Exam Questions and Topics

1. **OSI vs TCP/IP**: Differences, layer mapping, protocol placement
2. **Subnet Calculation**: CIDR notation, subnet mask, host calculations
3. **Routing Algorithms**: RIP vs OSPF, convergence time, complexity
4. **Error Detection**: CRC generation, parity concepts, Hamming codes
5. **TCP/UDP**: When to use which, reliability vs speed
6. **Congestion Control**: Slow start, CWND calculation, threshold
7. **DNS Resolution**: Iterative vs recursive, query process
8. **NAT/PAT**: Address mapping, translation table concepts
9. **Fragmentation**: When occurs, reassembly location, flags
10. **Window Size**: Flow control, congestion window calculations

### 9.2 Quick Reference Tables

#### Layer Functions Summary:

| Layer | Function | Devices | Protocols |
|-------|----------|---------|-----------|
| Physical | Bits | Hub, repeater | Ethernet cable, fiber |
| Data Link | Frames, MAC | Bridge, switch | Ethernet, PPP |
| Network | Routing, IP | Router | IP, ICMP, IGMP |
| Transport | End-to-end | Host | TCP, UDP |
| Application | Services | Server | HTTP, FTP, SMTP, DNS |

#### Routing Protocol Comparison:

| Feature | RIP | OSPF | BGP |
|---------|-----|------|-----|
| Type | Distance Vector | Link State | Path Vector |
| Algorithm | Bellman-Ford | Dijkstra | Path Vector |
| Metric | Hop count | Cost | BGP attributes |
| Max Hops | 15 | Unlimited | Unlimited |
| Convergence | Slow | Fast | Slow |
| Scalability | Poor | Good | Excellent |
| Scope | Within AS | Within AS | Between AS |
| Updates | Periodic | Triggered | Triggered |
| Bandwidth | High | Moderate | Low |

### 9.3 Calculation Examples for Exam Practice

**Example 1: CIDR Subnetting**
Given: 172.16.0.0/22, create 4 subnets
- /22 = 1024 total hosts, 512 hosts per /23
- Subnets:
  - 172.16.0.0/23 (0-511)
  - 172.16.2.0/23 (512-1023)
  - 172.16.4.0/23 (1024-1535)
  - 172.16.6.0/23 (1536-2047)

**Example 2: CRC Calculation**
Data: 1101000, Generator: 1011
- Append 3 zeros (generator length - 1): 1101000000
- Perform XOR division: Result is CRC
- [Calculation continues...]

**Example 3: TCP Window Calculation**
- Slow start: CWND = 1 MSS
- After 4 ACKs: CWND = 16 MSS (2^4)
- If SST = 32 MSS: After reaching SST, switch to congestion avoidance
- Linear increase: CWND += 1 MSS per RTT

---

## Part 10: Key Takeaways for GATE Exam

1. **Understand the layers**: Know what happens at each layer and how they interact

2. **Routing concepts**: RIP is simple but slow, OSPF is complex but fast

3. **Addressing**: Master CIDR notation and subnetting calculations

4. **Transport protocols**: TCP for reliability, UDP for speed

5. **Error handling**: CRC is most common for error detection, Hamming for correction

6. **DNS process**: Hierarchical resolution through root → TLD → authoritative servers

7. **TCP behavior**: Slow start, congestion avoidance, and detection phases

8. **NAT translation**: Multiple private IPs through single public IP using port numbers

9. **Connection vs connectionless**: TCP handshake vs UDP's direct sending

10. **Fragmentation**: Occurs at source or intermediate routers, reassembly at destination only

---

## Study Strategy for GATE 2026

### Phase 1: Conceptual Understanding
- Study OSI and TCP/IP models thoroughly
- Understand the purpose of each layer
- Learn how data flows through layers

### Phase 2: Protocol Deep Dive
- Focus on one protocol family at a time
- Routing protocols: RIP → OSPF → BGP
- Transport: UDP → TCP with flow/congestion control
- Application: DNS → HTTP → SMTP → FTP

### Phase 3: Calculations and Problem Solving
- Practice CIDR subnetting
- CRC calculations
- TCP sequence numbers and window sizes
- Routing table calculations

### Phase 4: Previous Year Questions
- Solve GATE questions from last 10 years
- Identify recurring topics
- Focus on high-weightage areas
- Time yourself

### Phase 5: Mock Tests
- Take full-length mock exams
- Analyze weak areas
- Revise and strengthen

---

## Important Formulas and Metrics

**CIDR Notation:**
- Host bits = 32 - prefix length
- Total addresses = 2^(host bits)
- Usable hosts = 2^(host bits) - 2

**Slow Start Growth:**
- CWND after n RTTs = 2^n MSS
- Stops when CWND ≥ SST

**Fragmentation:**
- Fragment offset measured in 8-byte units
- Total fragments = ⌈Data size / (MTU - IP header)⌉
- Maximum payload per fragment = MTU - IP header (usually 1500 - 20 = 1480 bytes)

---

Good luck with your GATE 2026 preparation!
