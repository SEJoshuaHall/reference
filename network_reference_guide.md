# Network Reference Guide
By Joshua Hall, Edited by Claude.ai
## Table of Contents

1. Network Fundamentals
2. Network Models
3. Physical Layer
4. Data Link Layer
5. Network Layer
6. Transport Layer
7. Application Layer
8. HTTP and the Web
9. Network Security
10. Network Diagnostics
11. Network Performance
12. Command Line Tools
13. Network Design Best Practices

## Network Fundamentals

### Definition

- **Network**: A collection of computing devices connected together for the purpose of sharing resources and communicating.
- **Protocol**: A set of rules that governs how data is transmitted between devices on a network.
- **Packet**: A unit of data transmitted over a network, containing control information and user data.
- **Bandwidth**: Theoretical maximum data transfer capacity of a network, measured in bits per second (bps).
- **Latency**: The delay between sending and receiving data, measured in milliseconds (ms).

### Key Concepts

- The internet is a vast _network of networks_ comprising both the physical infrastructure (devices, routers, cables) and the protocols enabling it to function.
- Data travels across networks in small units called packets, which contain headers (control information) and payload (actual data).
- Networks use addressing schemes to identify and route data between devices.
- Communication happens through established protocols, ensuring interoperability between different systems.

### Types of Networks

|Network Type|Coverage|Description|Examples|
|---|---|---|---|
|**PAN** (Personal Area Network)|Within a few meters|Connects personal devices|Bluetooth connections, USB connections|
|**LAN** (Local Area Network)|Building, campus|Connects devices in a limited area|Office networks, school networks|
|**MAN** (Metropolitan Area Network)|City-wide|Connects LANs in a city|City-wide university networks|
|**WAN** (Wide Area Network)|Country, global|Connects LANs over wide areas|The Internet, corporate global networks|

### Network Topologies

|Topology|Description|Advantages|Disadvantages|
|---|---|---|---|
|**Bus**|All devices connect to a single cable|Easy to implement, cost-effective|Single point of failure, limited scalability|
|**Star**|All devices connect to a central hub/switch|Easy to add devices, fault isolation|Dependent on central hub|
|**Ring**|Devices connect in a closed loop|Equal access for all devices|Single point of failure disrupts the ring|
|**Mesh**|Devices interconnect with multiple paths|Highly redundant, fault-tolerant|Complex, expensive to implement|
|**Tree**|Hierarchical combination of other topologies|Scalable, manageable hierarchy|Dependent on root node|

## Network Models

### The OSI Model

The Open Systems Interconnection (OSI) model provides a conceptual framework for understanding network interactions across seven distinct layers:

|Layer|Name|Function|Examples|
|---|---|---|---|
|7|Application|User interface & application services|HTTP, SMTP, FTP|
|6|Presentation|Data format translation, encryption|TLS/SSL, JPEG, ASCII|
|5|Session|Session establishment, management|NetBIOS, PPTP|
|4|Transport|End-to-end connections, reliability|TCP, UDP|
|3|Network|Logical addressing, routing|IP, ICMP, OSPF|
|2|Data Link|Physical addressing, media access|Ethernet, PPP, MAC|
|1|Physical|Transmission of raw bit streams|Cables, hubs, repeaters|

### The TCP/IP Model

The TCP/IP model (Internet Protocol Suite) is a more practical, implementation-focused model with four layers:

|Layer|Corresponds to OSI|Function|Protocols|
|---|---|---|---|
|Application|5-7|User applications, data presentation|HTTP, SMTP, DNS, FTP|
|Transport|4|End-to-end communication|TCP, UDP|
|Internet|3|Logical addressing, routing|IP, ICMP, ARP|
|Link|1-2|Physical addressing, media access|Ethernet, Wi-Fi, MAC|

### Protocol Data Units (PDUs)

Each layer in these models handles data in specific units:

|Layer|PDU Name|
|---|---|
|Application|Data|
|Transport|Segment (TCP) / Datagram (UDP)|
|Network/Internet|Packet|
|Data Link|Frame|
|Physical|Bit|

## Physical Layer

### Transmission Media

|Type|Description|Advantages|Disadvantages|
|---|---|---|---|
|**Twisted Pair**|Copper wires twisted to reduce interference|Inexpensive, easy to install|Limited bandwidth, susceptible to EMI|
|**Coaxial Cable**|Central copper conductor with insulating layer|Better bandwidth, less EMI|More expensive, harder to install|
|**Fiber Optic**|Glass/plastic strands transmitting light|Highest bandwidth, immune to EMI|Most expensive, specialized equipment|
|**Wireless**|Radio frequency transmission|No physical cabling needed|Interference issues, security concerns|

### Physical Layer Standards

|Standard|Medium|Data Rate|Maximum Distance|Notes|
|---|---|---|---|---|
|10BASE-T|Twisted pair|10 Mbps|100m|Legacy Ethernet|
|100BASE-TX|Twisted pair|100 Mbps|100m|Fast Ethernet|
|1000BASE-T|Twisted pair|1 Gbps|100m|Gigabit Ethernet|
|10GBASE-T|Twisted pair|10 Gbps|100m|10 Gigabit Ethernet|
|1000BASE-SX|Multimode fiber|1 Gbps|550m|Short-range fiber|
|1000BASE-LX|Single-mode fiber|1 Gbps|5km|Long-range fiber|
|IEEE 802.11n|Wireless|Up to 600 Mbps|70m indoor|Wi-Fi standard|
|IEEE 802.11ac|Wireless|Up to 3.5 Gbps|35m indoor|Wi-Fi 5 standard|
|IEEE 802.11ax|Wireless|Up to 9.6 Gbps|30m indoor|Wi-Fi 6 standard|

### Physical Layer Devices

|Device|Function|OSI Layer|
|---|---|---|
|Repeater|Amplifies or regenerates signal|Layer 1|
|Hub|Connects multiple devices, broadcasts data to all ports|Layer 1|
|Media Converter|Converts between different physical media|Layer 1|
|Modem|Modulates/demodulates signals for transmission|Layer 1|

## Data Link Layer

### Functions

- Framing: Encapsulating network layer packets into frames
- Physical addressing: Using MAC addresses for local device identification
- Error detection: Checking frames for transmission errors
- Media access control: Determining which device can access the medium at a given time

### MAC Addressing

- 48-bit (6-byte) physical address assigned to network interfaces
- Format: XX:XX:XX:XX:XX:XX (each X is a hexadecimal digit)
- First 3 bytes: Organizationally Unique Identifier (OUI), assigned to manufacturer
- Last 3 bytes: Network Interface Controller (NIC) specific
- Addressing types:
    - Unicast: Frames sent to a single destination
    - Multicast: Frames sent to a group of destinations
    - Broadcast: Frames sent to all devices on the network (FF:FF:FF:FF:FF:FF)

### Ethernet

- Most common LAN technology
- IEEE 802.3 standard
- CSMA/CD (Carrier Sense Multiple Access with Collision Detection) for media access
- Standard frame format:
    - Preamble (8 bytes)
    - Destination MAC address (6 bytes)
    - Source MAC address (6 bytes)
    - Type/Length (2 bytes)
    - Data payload (46-1500 bytes)
    - Frame Check Sequence (4 bytes)

### Data Link Layer Devices

|Device|Function|OSI Layer|
|---|---|---|
|Bridge|Connects network segments, filters traffic based on MAC addresses|Layer 2|
|Switch|Connects devices, forwards frames based on MAC addresses|Layer 2|
|Wireless Access Point|Connects wireless devices to a wired network|Layer 2|

## Network Layer

### IP Addressing

#### IPv4

- 32-bit address represented in dotted decimal notation (e.g., 192.168.1.1)
- Divided into network and host portions
- Address classes:
    - Class A: 0.0.0.0 - 127.255.255.255 (8-bit network prefix)
    - Class B: 128.0.0.0 - 191.255.255.255 (16-bit network prefix)
    - Class C: 192.0.0.0 - 223.255.255.255 (24-bit network prefix)
    - Class D: 224.0.0.0 - 239.255.255.255 (Multicast)
    - Class E: 240.0.0.0 - 255.255.255.255 (Reserved)
- Private address ranges:
    - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
    - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
    - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)

#### IPv6

- 128-bit address represented in hexadecimal notation (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
- Can be abbreviated by removing leading zeros and replacing consecutive zero blocks with ::
- Address types:
    - Global Unicast: Globally routable addresses (2000::/3)
    - Link-Local: Non-routable addresses for local communication (fe80::/10)
    - Unique Local: Private addresses similar to IPv4 private ranges (fc00::/7)
    - Multicast: One-to-many communication (ff00::/8)

### Subnetting

- Process of dividing a network into smaller subnetworks
- Uses CIDR (Classless Inter-Domain Routing) notation: IP address followed by /prefix (e.g., 192.168.1.0/24)
- Subnet mask: Binary mask identifying network vs. host portions (e.g., 255.255.255.0)

### Routing

- Process of forwarding packets from source to destination across multiple networks
- Routing tables contain:
    - Network destination
    - Subnet mask
    - Gateway (next hop)
    - Interface
    - Metric (route preference)
- Routing protocols:
    - Distance Vector: RIP, EIGRP
    - Link State: OSPF, IS-IS
    - Path Vector: BGP

### Network Layer Protocols

|Protocol|Purpose|Key Features|
|---|---|---|
|**IP**|Packet delivery|Connectionless, best-effort delivery|
|**ICMP**|Error reporting & diagnostics|Used by ping and traceroute|
|**ARP**|Maps IP addresses to MAC addresses|Caches mappings for efficiency|
|**OSPF**|Interior gateway routing protocol|Link-state, fast convergence|
|**BGP**|Exterior gateway routing protocol|Path vector, connects autonomous systems|

### Network Layer Devices

|Device|Function|OSI Layer|
|---|---|---|
|Router|Connects networks, routes packets based on IP addresses|Layer 3|
|Layer 3 Switch|Combines switching and routing functions|Layer 3|
|Multilayer Switch|Provides Layer 2-4 functions|Layers 2-4|

## Transport Layer

### Comparison of TCP and UDP

|Feature|TCP|UDP|
|---|---|---|
|**Connection**|Connection-oriented|Connectionless|
|**Reliability**|Guaranteed delivery|Best-effort delivery|
|**Order**|Maintains sequence|No sequence guarantee|
|**Error detection**|Yes|Optional (IPv4), Required (IPv6)|
|**Flow control**|Yes|No|
|**Congestion control**|Yes|No|
|**Overhead**|Higher|Lower|
|**Speed**|Slower|Faster|
|**Use cases**|Web, email, file transfers|Streaming, gaming, DNS|

### TCP Three-Way Handshake

Process for establishing a TCP connection:

1. **SYN**: Client sends SYN packet with initial sequence number (ISN)
2. **SYN-ACK**: Server responds with SYN-ACK packet, acknowledges client's ISN, sends own ISN
3. **ACK**: Client acknowledges server's ISN, connection established

### Flow Control and Congestion Avoidance

- **Flow Control**: Prevents sender from overwhelming receiver
    - Uses sliding window protocol
    - Window size dynamically adjusted based on receiver capacity
- **Congestion Avoidance**: Prevents overwhelming the network
    - Slow Start: Starts with small window, doubles until threshold
    - Congestion Avoidance: Linear growth after threshold
    - Fast Retransmit: Retransmits after duplicate ACKs
    - Fast Recovery: Avoids going back to slow start

### Port Numbers

- 16-bit values (0-65535) that identify specific applications/services
- Well-known ports: 0-1023 (require administrative privileges)
- Registered ports: 1024-49151 (registered with IANA)
- Dynamic/private ports: 49152-65535 (for temporary connections)

|Service|Port|Protocol|
|---|---|---|
|FTP Data|20|TCP|
|FTP Control|21|TCP|
|SSH|22|TCP|
|Telnet|23|TCP|
|SMTP|25|TCP|
|DNS|53|TCP/UDP|
|HTTP|80|TCP|
|POP3|110|TCP|
|IMAP|143|TCP|
|HTTPS|443|TCP|
|RDP|3389|TCP|

## Application Layer

### Domain Name System (DNS)

- Hierarchical, distributed database mapping domain names to IP addresses
- Components:
    - DNS Resolver: Client-side component that initiates queries
    - Root Servers: Top of the DNS hierarchy
    - TLD Servers: Manage top-level domains (.com, .org, etc.)
    - Authoritative Servers: Provide definitive answers for specific domains
- Record types:
    - A: Maps hostname to IPv4 address
    - AAAA: Maps hostname to IPv6 address
    - CNAME: Canonical name (alias)
    - MX: Mail exchange
    - NS: Name server
    - TXT: Text information
    - SOA: Start of authority

### Email Protocols

- **SMTP** (Simple Mail Transfer Protocol):
    - Used for sending email
    - Port 25 (unencrypted), 587 (with STARTTLS), or 465 (with SSL/TLS)
- **POP3** (Post Office Protocol):
    - Used for retrieving email
    - Downloads messages to client, typically removes from server
    - Port 110 (unencrypted) or 995 (with SSL/TLS)
- **IMAP** (Internet Message Access Protocol):
    - Used for retrieving email
    - Keeps messages on server, allowing access from multiple devices
    - Port 143 (unencrypted) or 993 (with SSL/TLS)

### File Transfer Protocols

- **FTP** (File Transfer Protocol):
    - Standard protocol for transferring files
    - Uses separate control (21) and data (20) connections
    - Supports authentication, but transmits credentials in cleartext
- **SFTP** (SSH File Transfer Protocol):
    - Secure file transfer using SSH
    - Encrypts commands and data
    - Port 22 (same as SSH)
- **FTPS** (FTP Secure):
    - FTP with SSL/TLS encryption
    - Port 990 (implicit) or 21 (explicit with STARTTLS)

### Other Application Layer Protocols

- **DHCP** (Dynamic Host Configuration Protocol):
    - Automatically assigns IP addresses to network devices
    - Uses ports 67 (server) and 68 (client), UDP
- **NTP** (Network Time Protocol):
    - Synchronizes clocks between computer systems
    - Port 123, UDP
- **SNMP** (Simple Network Management Protocol):
    - Monitors and manages network devices
    - Ports 161 (standard) and 162 (traps), UDP

## HTTP and the Web

### URL Structure

- **Scheme**: Protocol being used (http, https, ftp, etc.)
- **Host**: Domain name or IP address of the server
- **Port**: Port number (optional, defaults based on scheme)
- **Path**: Resource path on the server
- **Query string**: Additional parameters (after ?)
- **Fragment**: Reference to a section within the resource (after #)

Example: `https://www.example.com:443/products/search?category=books&author=smith#section2`

### HTTP Methods

|Method|Purpose|Idempotent|Safe|
|---|---|---|---|
|GET|Retrieve a resource|Yes|Yes|
|POST|Create a new resource|No|No|
|PUT|Update or replace a resource|Yes|No|
|DELETE|Remove a resource|Yes|No|
|HEAD|Retrieve headers only|Yes|Yes|
|OPTIONS|Get supported methods|Yes|Yes|
|PATCH|Partially update a resource|No|No|

### HTTP Status Codes

|Code Range|Category|Examples|
|---|---|---|
|1xx|Informational|100 Continue, 101 Switching Protocols|
|2xx|Success|200 OK, 201 Created, 204 No Content|
|3xx|Redirection|301 Moved Permanently, 302 Found, 304 Not Modified|
|4xx|Client Error|400 Bad Request, 401 Unauthorized, 404 Not Found|
|5xx|Server Error|500 Internal Server Error, 503 Service Unavailable|

### HTTP Headers

|Header|Type|Purpose|Example|
|---|---|---|---|
|User-Agent|Request|Identifies client software|`User-Agent: Mozilla/5.0...`|
|Host|Request|Specifies target host|`Host: www.example.com`|
|Cookie|Request|Sends stored cookies|`Cookie: sessionid=123`|
|Authorization|Request|Authentication credentials|`Authorization: Basic YWRtaW46cGFzc3dvcmQ=`|
|Content-Type|Both|Specifies media type|`Content-Type: application/json`|
|Content-Length|Both|Size of body in bytes|`Content-Length: 348`|
|Location|Response|URL for redirection|`Location: /new-url`|
|Set-Cookie|Response|Sets a cookie|`Set-Cookie: sessionid=123; Path=/`|
|Cache-Control|Both|Caching directives|`Cache-Control: max-age=3600`|

### HTTP State Management

HTTP is stateless by design, but several mechanisms can be used to maintain state:

- **Cookies**: Small pieces of data stored by the browser, sent with every request to the same server
- **Sessions**: Server-side storage that uses a session ID (often stored in a cookie) to identify the client
- **URL Parameters**: State information encoded in the URL
- **Hidden Form Fields**: Data embedded in forms that persist between page loads
- **Web Storage**: Client-side storage options: localStorage and sessionStorage
- **AJAX (Asynchronous JavaScript and XML)**: Allows asynchronous communication with the server

### HTTP/2 and HTTP/3

- **HTTP/2**:
    - Multiplexed connections (multiple requests over a single connection)
    - Header compression (HPACK)
    - Server push (preemptively sending resources)
    - Binary protocol (vs. text-based HTTP/1.1)
- **HTTP/3**:
    - Uses QUIC protocol instead of TCP
    - Improved performance for lossy connections
    - Built-in encryption (TLS 1.3)
    - Further reduced latency through improved multiplexing

## Network Security

### TLS/SSL

- Provides encryption, authentication, and integrity
- Key components:
    - **Handshake Protocol**: Negotiates cipher suite, exchanges keys
    - **Record Protocol**: Encrypts and transmits data
    - **Alert Protocol**: Signals errors and warnings
- TLS handshake process:
    1. Client Hello: Client sends supported cipher suites, random number
    2. Server Hello: Server selects cipher suite, sends certificate, random number
    3. Key Exchange: Client verifies certificate, sends premaster secret
    4. Finished: Both sides derive session keys, confirm secure connection

### Common Security Threats

|Threat|Description|Mitigation|
|---|---|---|
|**Packet Sniffing**|Capturing and analyzing network traffic|Encryption (TLS/SSL), VPN|
|**Man-in-the-Middle**|Intercepting and possibly altering communication|Certificate validation, HSTS|
|**DDoS**|Overwhelming a service with traffic|Traffic filtering, CDN, rate limiting|
|**DNS Poisoning**|Corrupting DNS cache to redirect traffic|DNSSEC, DNS monitoring|
|**ARP Spoofing**|Associating attacker's MAC with legitimate IP|Static ARP entries, ARP detection tools|
|**SQL Injection**|Inserting malicious SQL code|Parameterized queries, input validation|
|**Cross-Site Scripting (XSS)**|Injecting malicious scripts|Content Security Policy, input sanitization|
|**Cross-Site Request Forgery (CSRF)**|Tricking user into unwanted actions|Anti-CSRF tokens, SameSite cookies|

### Security Protocols and Technologies

|Technology|Purpose|Implementation|
|---|---|---|
|**Firewall**|Controls traffic based on rules|Network, host-based, application|
|**IDS/IPS**|Detects/prevents intrusions|Signature-based, anomaly-based|
|**VPN**|Secure tunnel over public network|IPsec, SSL/TLS, WireGuard|
|**NAT**|Maps private IPs to public IPs|One-to-one, one-to-many (PAT)|
|**ACL**|Controls access to resources|Packet filtering, user-based|
|**WAF**|Protects web applications|Rule-based, learning mode|
|**DNSSEC**|Secures DNS responses|Digital signatures, authenticated denial|
|**2FA/MFA**|Multiple authentication factors|Something you know/have/are|

## Network Diagnostics

### Common Network Problems

|Problem|Symptoms|Diagnostic Approach|
|---|---|---|
|**Connectivity Issues**|No network access|Check physical connections, ping gateway, trace route|
|**DNS Problems**|Cannot resolve domains|nslookup/dig, check DNS settings, flush DNS cache|
|**High Latency**|Slow response times|ping, traceroute, mtr, check for network congestion|
|**Packet Loss**|Intermittent connectivity|ping with count, mtr, analyze patterns|
|**DHCP Issues**|No IP address assignment|ipconfig/ifconfig, check DHCP server, verify lease time|
|**Routing Problems**|Cannot reach specific networks|traceroute, check routing tables, verify gateway|
|**Authentication Failures**|Cannot access secured resources|Check credentials, verify certificates, check logs|

### Diagnostic Tools

|Tool|Purpose|Basic Usage|
|---|---|---|
|**ping**|Tests reachability and response time|`ping example.com`|
|**traceroute/tracert**|Shows path packets take|`traceroute example.com`|
|**nslookup/dig**|Queries DNS records|`nslookup example.com`|
|**netstat**|Displays network connections|`netstat -an`|
|**tcpdump/Wireshark**|Captures and analyzes packets|`tcpdump -i eth0 port 80`|
|**ifconfig/ipconfig**|Shows network interface configuration|`ifconfig`|
|**mtr**|Combines ping and traceroute|`mtr example.com`|
|**nmap**|Scans ports and services|`nmap -p 1-1000 example.com`|
|**iperf**|Tests network bandwidth|`iperf -c server_ip`|
|**curl**|Tests HTTP connections|`curl -v example.com`|

### Common Error Messages

|Error Message|Likely Cause|Troubleshooting Steps|
|---|---|---|
|**Network Unreachable**|Routing problem|Check default gateway, routing tables|
|**Connection Refused**|Service not running or blocked|Verify service status, check firewall|
|**No Route to Host**|No valid path to destination|Check routing, firewall, host availability|
|**Host Unknown**|DNS resolution failure|Check DNS settings, host file|
|**Request Timed Out**|Host unreachable or too slow|Check connectivity, firewall, host load|
|**SSL Certificate Error**|Certificate validation issue|Verify date/time, certificate chain, CA trust|
|**Too Many Redirects**|Redirection loop|Check server configuration, cookies|

## Network Performance

### Performance Metrics

|Metric|Description|Typical Values|Measurement Tools|
|---|---|---|---|
|**Bandwidth**|Maximum data throughput|100 Mbps - 10 Gbps (LAN)|iperf, speedtest|
|**Throughput**|Actual data transfer rate|Variable based on network|iperf, file transfer test|
|**Latency**|Delay between sending and receiving|1-20 ms (LAN), 20-150 ms (WAN)|ping, traceroute|
|**Jitter**|Variation in packet delay|<30 ms for voice/video|iperf, specialized tools|
|**Packet Loss**|Percentage of lost packets|<1% acceptable, <0.1% ideal|ping, mtr|
|**Connection Time**|Time to establish connection|<200 ms|curl -w timing format|
|**DNS Resolution Time**|Time to resolve domain name|<100 ms|dig +stats, browser tools|

### Performance Optimization Techniques

|Technique|Description|Implementation|
|---|---|---|
|**Caching**|Store copies of data closer to users|CDN, proxy cache, browser cache|
|**Compression**|Reduce data size for transmission|HTTP compression, image optimization|
|**Load Balancing**|Distribute traffic across servers|Hardware/software load balancers, DNS rotation|
|**TCP Optimization**|Tune TCP parameters|Adjust window size, congestion control algorithms|
|**QoS (Quality of Service)**|Prioritize certain traffic types|Traffic shaping, packet marking|
|**Content Delivery Network**|Distributed server network|Use for static assets, global distribution|
|**Protocol Optimization**|Use efficient protocols|HTTP/2, QUIC, optimize DNS|

### Benchmarking

- **Website Performance**:
    - Page load time: <2 seconds preferred
    - Time to First Byte (TTFB): <200 ms
    - Tools: Lighthouse, WebPageTest, PageSpeed Insights
- **Network Performance**:
    - Baseline tests: Regular performance snapshots
    - Comparison testing: Before/after changes
    - Stress testing: Performance under load
    - Tools: iperf, netperf, ab (Apache Benchmark)

## Command Line Tools

### Basic Network Commands (Cross-Platform)

|Command|Purpose|Examples|
|---|---|---|
|ping|Test connectivity|`ping example.com`|
|traceroute/tracert|Show network path|`traceroute example.com`|
|nslookup|DNS lookup|`nslookup example.com`|
|netstat|Network statistics|`netstat -an` (connections), `netstat -r` (routing)|
|ipconfig/ifconfig|Interface configuration|`ifconfig` or `ip addr show`|
|arp|View/manipulate ARP cache|`arp -a`|
|route|View/manipulate routing table|`route print` or `ip route show`|

### Advanced Network Commands (Linux/macOS)

|Command|Purpose|Examples|
|---|---|---|
|ss|Socket statistics|`ss -tuln` (listening ports)|
|ip|IP networking|`ip link show`, `ip addr add 192.168.1.10/24 dev eth0`|
|iptables|Firewall management|`iptables -L`, `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`|
|tcpdump|Packet capture|`tcpdump -i eth0 port 80`|
|dig|DNS lookup tool|`dig example.com +short`, `dig example.com MX`|
|host|Simple DNS lookup|`host example.com`|
|curl|HTTP client|`curl -v https://example.com`|
|wget|Download files|`wget https://example.com/file.zip`|
|netcat (nc)|TCP/UDP connection tool|`nc -v example.com 80`, `nc -l 8080`|
|mtr|Network diagnostic|`mtr example.com`|

### Windows-Specific Commands

|Command|Purpose|Examples|
|---|---|---|
|ipconfig|Interface configuration|`ipconfig /all`|
|nbtstat|NetBIOS information|`nbtstat -n`|
|pathping|Enhanced traceroute|`pathping example.com`|
|netsh|Network shell|`netsh interface show interface`|
|route|Routing table management|`route print`|
|getmac|MAC address lookup|`getmac /v`|
|nslookup|DNS query tool|`nslookup -type=mx example.com`|
|tracert|Trace route|`tracert example.com`|

## Network Design Best Practices

### Network Design Principles

|Principle|Description|Implementation|
|---|---|---|
|**Hierarchy**|Organize network in layers|Core, distribution, access layers|
|**Redundancy**|Eliminate single points of failure|Redundant links, devices, power|
|**Scalability**|Ability to grow without redesign|Modular design, room for expansion|
|**Security**|Protection at multiple levels|Defense in depth, segmentation|
|**Manageability**|Ease of monitoring and maintenance|Standard configurations, documentation|
|**Simplicity**|Avoid unnecessary complexity|Standardize where possible|
|**Cost-effectiveness**|Balance performance and cost|Right-sizing, appropriate technology|

### Network Documentation

- **Network Diagrams**:
    - Physical topology
    - Logical topology
    - Rack diagrams
    - Wiring schemas
- **IP Address Management (IPAM)**:
    - Subnet allocation
    - IP address assignments
    - DHCP scopes
    - DNS zones
- **Configuration Documentation**:
    - Device configurations
    - Change logs
    - Standard operating procedures
    - Disaster recovery plans

### Common Network Architectures

| Architecture      | Description                          | Best For                               |
| ----------------- | ------------------------------------ | -------------------------------------- |
| **Three-Tier**    | Core, distribution, access layers    | Enterprise networks                    |
| **Spine-Leaf**    | Two-layer data center design         | Data centers, cloud infrastructure     |
| **Hub and Spoke** | Central hub with remote sites        | WAN, branch offices                    |
| **Full Mesh**     | All sites connect to all others      | Critical networks requiring redundancy |
| **SD-WAN**        | Software-defined WAN                 | Organizations with multiple locations  |
| **Zero Trust**    | No implicit trust, verify everything | High-security environments             |
| **Cloud-Centric** | Based around cloud services          | Cloud-first organizations              |