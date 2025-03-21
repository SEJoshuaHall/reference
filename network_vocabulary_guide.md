# Network Vocabulary Guide
By Joshua Hall, Edited by Claude.ai
## Fundamental Concepts

| Term                   | Definition                                                                                                  |
| ---------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Network**            | A collection of computing devices connected together for the purpose of sharing resources and communicating |
| **Protocol**           | A set of rules that governs how data is transmitted between devices on a network                            |
| **Packet**             | A unit of data transmitted over a network, containing control information and user data                     |
| **Bandwidth**          | The maximum data transfer capacity of a network, measured in bits per second (bps)                          |
| **Latency**            | The delay between sending and receiving data, measured in milliseconds (ms)                                 |
| **Jitter**             | Variation in latency/packet delay over time, which can impact real-time applications                        |
| **Throughput**         | The actual amount of data successfully transferred over a network in a given time period                    |
| **Hop**                | Each pass through an intermediary device (typically a router) as data travels from source to destination    |
| **Network Topology**   | The physical or logical arrangement of devices in a network                                                 |
| **Node**               | Any device connected to a network that can send, receive, or forward information                            |
| **Host**               | A computer or other device connected to a network that has a network address                                |
| **Client**             | A device or application that requests services or resources from a server                                   |
| **Server**             | A device or application that provides services or resources to clients                                      |
| **Peer-to-Peer (P2P)** | A network model where computers can act as both clients and servers to each other                           |
| **OSI Model**          | A conceptual framework used to understand network interactions across seven distinct layers                 |
| **TCP/IP Model**       | A practical, implementation-focused model with four layers used for internet communications                 |

## Network Types and Topologies

|Term|Definition|
|---|---|
|**LAN (Local Area Network)**|A network covering a small geographic area, typically a home, office, or building|
|**WAN (Wide Area Network)**|A network spanning a large geographical area, often connecting multiple LANs|
|**MAN (Metropolitan Area Network)**|A network spanning a city or large campus|
|**PAN (Personal Area Network)**|A network for communicating among personal devices within a very small area|
|**WLAN (Wireless LAN)**|A LAN that uses wireless communication instead of wired connections|
|**VPN (Virtual Private Network)**|A secure connection over a public network (usually the internet) that provides privacy and anonymity|
|**VLAN (Virtual LAN)**|A logical subdivision of a physical network that allows groups of devices to communicate as if they were on the same physical LAN|
|**Bus Topology**|Network arrangement where all devices connect to a central cable|
|**Star Topology**|Network arrangement where all devices connect to a central hub or switch|
|**Ring Topology**|Network arrangement where devices form a closed loop|
|**Mesh Topology**|Network arrangement where devices are interconnected with multiple paths between nodes|
|**Tree Topology**|Hierarchical network arrangement with a root node and branches|
|**Hybrid Topology**|Network that combines two or more different topology structures|

## Physical Layer Concepts

|Term|Definition|
|---|---|
|**Ethernet**|A family of wired computer networking technologies commonly used in LANs|
|**Wi-Fi**|Wireless networking technology that uses radio waves to provide wireless network connections|
|**Bluetooth**|Short-range wireless technology standard for exchanging data between fixed and mobile devices|
|**NFC (Near Field Communication)**|Short-range wireless technology that enables simple and secure communication between electronic devices|
|**Cat5/Cat5e/Cat6**|Categories of twisted pair cables used for carrying Ethernet signals|
|**Fiber Optic Cable**|Cable containing one or more optical fibers that uses light pulses to transmit data|
|**Coaxial Cable**|Cable with a central conductor surrounded by an insulating layer and a conductive shield|
|**RJ45 Connector**|Standard connector for Ethernet network cables|
|**Transceiver**|Device that can both transmit and receive signals|
|**NIC (Network Interface Card)**|Hardware component that connects a computer to a network|
|**Hub**|Device that connects multiple network segments together, broadcasting data to all ports|
|**Repeater**|Device that receives, amplifies, and retransmits signals to extend the range of a network|
|**Modem**|Device that modulates and demodulates signals to enable data transmission over communication lines|
|**ISP (Internet Service Provider)**|Company that provides internet access to customers|
|**Bandwidth**|The maximum rate at which data can be transferred over a network connection|
|**Duplex**|Communication channel capable of transmitting in both directions (half-duplex vs. full-duplex)|

## Data Link Layer Concepts

|Term|Definition|
|---|---|
|**MAC Address**|Media Access Control address; a unique identifier assigned to a network interface controller (NIC)|
|**Frame**|A data unit at the Data Link layer containing address and error-detection fields|
|**Switch**|Network device that connects devices within a network and uses MAC addresses to forward data to the correct destination|
|**Bridge**|Device that connects multiple network segments at the Data Link layer|
|**ARP (Address Resolution Protocol)**|Protocol used to find the MAC address of a device from its IP address|
|**LLC (Logical Link Control)**|Upper sublayer of the Data Link layer that provides flow control and error notification|
|**MAC (Media Access Control)**|Lower sublayer of the Data Link layer that controls how devices access a shared medium|
|**CSMA/CD (Carrier Sense Multiple Access with Collision Detection)**|Protocol used in Ethernet to determine when devices can transmit data on a shared medium|
|**CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)**|Protocol used in wireless networks to avoid collisions during transmission|
|**MTU (Maximum Transmission Unit)**|The largest size of a protocol data unit that can be transmitted in a single network layer transaction|
|**Broadcast Domain**|A logical division of a network in which all devices can reach each other by broadcast at the Data Link layer|
|**Collision Domain**|A network segment where data collisions can occur, typically within a shared medium|
|**STP (Spanning Tree Protocol)**|Protocol that prevents loops in networks with redundant paths|
|**VLAN (Virtual LAN) Trunking**|Method for carrying multiple VLANs over a single physical link|

## Network Layer Concepts

|Term|Definition|
|---|---|
|**IP (Internet Protocol)**|The primary network layer protocol in the Internet Protocol Suite for relaying datagrams across network boundaries|
|**IP Address**|A numerical label assigned to each device connected to a computer network that uses the Internet Protocol|
|**IPv4**|Fourth version of the Internet Protocol using 32-bit addresses (e.g., 192.168.1.1)|
|**IPv6**|Sixth version of the Internet Protocol using 128-bit addresses (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)|
|**Subnet**|A logical subdivision of an IP network|
|**Subnet Mask**|A 32-bit number that divides an IP address into network and host portions|
|**CIDR (Classless Inter-Domain Routing)**|A method for allocating IP addresses and IP routing|
|**Router**|Network device that forwards data packets between computer networks|
|**Default Gateway**|Node that serves as the forwarding host to other networks when no other route is specified|
|**NAT (Network Address Translation)**|Method of remapping one IP address space into another by modifying network address information|
|**ICMP (Internet Control Message Protocol)**|Protocol used by network devices to diagnose network communication issues|
|**TTL (Time to Live)**|A mechanism that limits the lifespan of data in a network to prevent packets from circulating indefinitely|
|**Routing**|Process of selecting paths in a network along which to send data|
|**Routing Table**|Data table stored in a router or network host that lists routes to particular network destinations|
|**Static Routing**|Routing configuration that uses manually-configured routes rather than learning them via a dynamic routing protocol|
|**Dynamic Routing**|Routing that automatically adjusts paths based on network changes|
|**OSPF (Open Shortest Path First)**|Link-state routing protocol that uses a link state algorithm to find the shortest path|
|**BGP (Border Gateway Protocol)**|A standardized exterior gateway protocol designed to exchange routing information between autonomous systems|
|**RIP (Routing Information Protocol)**|Distance-vector routing protocol that selects the route with the lowest hop count|
|**AS (Autonomous System)**|A collection of connected Internet Protocol routing prefixes under the control of one or more network operators|

## Transport Layer Concepts

|Term|Definition|
|---|---|
|**TCP (Transmission Control Protocol)**|Connection-oriented protocol providing reliable, ordered, and error-checked delivery of data|
|**UDP (User Datagram Protocol)**|Connectionless protocol with minimal overhead but no guarantees of delivery, ordering, or duplicate protection|
|**Port Number**|A 16-bit number used to identify specific applications/services|
|**Socket**|The combination of an IP address and a port number that uniquely identifies an endpoint|
|**Handshake**|Process by which two devices establish a connection before data transmission|
|**Three-Way Handshake**|Process used by TCP to establish a connection using SYN, SYN-ACK, and ACK messages|
|**Flow Control**|Mechanism to prevent a sender from overwhelming a receiver|
|**Congestion Control**|Mechanism to prevent overwhelming the network|
|**Sliding Window**|Flow control method that allows the sender to transmit multiple packets before requiring acknowledgment|
|**Acknowledgment (ACK)**|Message sent by the receiver to indicate successful receipt of data|
|**Negative Acknowledgment (NACK)**|Message sent by the receiver to indicate failed receipt of data|
|**Segment**|A data unit at the Transport layer in TCP|
|**Datagram**|A data unit at the Transport layer in UDP|
|**Multiplexing**|Process of combining multiple signals into one signal for transmission|
|**Demultiplexing**|Process of separating a combined signal into its component signals|

## Application Layer Concepts

|Term|Definition|
|---|---|
|**HTTP (Hypertext Transfer Protocol)**|Application protocol for distributed, collaborative, hypermedia information systems|
|**HTTPS (HTTP Secure)**|Secure version of HTTP using TLS/SSL for encryption|
|**FTP (File Transfer Protocol)**|Protocol for transferring files between a client and a server|
|**SMTP (Simple Mail Transfer Protocol)**|Protocol for sending email messages between servers|
|**POP3 (Post Office Protocol)**|Protocol used by email clients to retrieve email from a server|
|**IMAP (Internet Message Access Protocol)**|Protocol for email retrieval with features beyond POP3|
|**DNS (Domain Name System)**|System that translates domain names into IP addresses|
|**DHCP (Dynamic Host Configuration Protocol)**|Protocol that dynamically assigns IP addresses to devices|
|**SSH (Secure Shell)**|Cryptographic network protocol for secure data communication|
|**Telnet**|Network protocol for bidirectional interactive text-oriented communication|
|**SNMP (Simple Network Management Protocol)**|Protocol for collecting and organizing information about managed devices on networks|
|**API (Application Programming Interface)**|Set of rules and protocols for building and interacting with software applications|
|**REST (Representational State Transfer)**|Architectural style for designing networked applications|
|**SOAP (Simple Object Access Protocol)**|Protocol for exchanging structured information in web services|
|**URL (Uniform Resource Locator)**|Reference to a web resource specifying its location and the mechanism for retrieving it|
|**URI (Uniform Resource Identifier)**|String of characters used to identify a resource|
|**Cookies**|Small pieces of data stored by the browser and sent to the server with each request|
|**WebSocket**|Protocol providing full-duplex communication channels over a single TCP connection|

## Web Concepts

|Term|Definition|
|---|---|
|**HTTP Methods**|Commands indicating the desired action to be performed on a resource (GET, POST, PUT, DELETE, etc.)|
|**HTTP Status Codes**|Three-digit codes returned by a server in response to an HTTP request|
|**HTTP Headers**|Additional information sent with HTTP requests and responses|
|**Query String**|Part of a URL that assigns values to specified parameters|
|**REST API**|API that conforms to the constraints of REST architectural style|
|**AJAX (Asynchronous JavaScript and XML)**|Technique for creating asynchronous web applications|
|**JSON (JavaScript Object Notation)**|Lightweight data-interchange format|
|**XML (Extensible Markup Language)**|Markup language that defines rules for encoding documents|
|**CDN (Content Delivery Network)**|Distributed network of servers that delivers web content to users based on geographic location|
|**Web Cache**|Temporary storage of web documents to reduce server load and bandwidth usage|
|**Proxy Server**|Server that acts as an intermediary for requests from clients seeking resources from other servers|
|**Load Balancer**|Device that distributes network traffic across multiple servers|
|**Reverse Proxy**|Server that sits between client devices and a web server, forwarding client requests to the web server|
|**API Gateway**|Server that acts as an API front-end, receiving API requests and routing them to the appropriate backend service|
|**OAuth**|Open standard for access delegation, commonly used for user authentication|
|**JWT (JSON Web Token)**|Compact, URL-safe means of representing claims to be transferred between two parties|

## Security Concepts

|Term|Definition|
|---|---|
|**Firewall**|Network security system that monitors and controls incoming and outgoing network traffic|
|**IDS (Intrusion Detection System)**|Device or software that monitors a network for malicious activity or policy violations|
|**IPS (Intrusion Prevention System)**|Network security appliance that monitors network traffic for malicious activity and can block or prevent it|
|**DMZ (Demilitarized Zone)**|Physical or logical subnetwork that contains and exposes an organization's external-facing services|
|**SSL/TLS (Secure Sockets Layer/Transport Layer Security)**|Cryptographic protocols designed to provide secure communication over a network|
|**Certificate**|Digital document that verifies the identity of a server and contains the server's public key|
|**PKI (Public Key Infrastructure)**|Set of roles, policies, and procedures needed to create, manage, distribute, use, store, and revoke digital certificates|
|**Encryption**|Process of encoding information so that only authorized parties can access it|
|**Hashing**|Process of converting an input of any length into a fixed-size string of text|
|**VPN (Virtual Private Network)**|Secure connection over a public network|
|**IPsec (Internet Protocol Security)**|Protocol suite for secure Internet Protocol communication|
|**AES (Advanced Encryption Standard)**|Specification for the encryption of electronic data|
|**DDoS (Distributed Denial of Service)**|Attack where multiple systems flood a target with traffic to overload it|
|**Phishing**|Attempt to obtain sensitive information by disguising as a trustworthy entity|
|**Man-in-the-Middle Attack**|Attack where an attacker secretly relays and alters communication between two parties|
|**Ransomware**|Malware that threatens to publish or block access to data unless a ransom is paid|
|**Zero-day Vulnerability**|Software vulnerability unknown to those who should be interested in mitigating it|
|**Two-Factor Authentication (2FA)**|Security process requiring two different forms of identification|

## Network Performance and Monitoring

|Term|Definition|
|---|---|
|**QoS (Quality of Service)**|Technologies and techniques for managing network resources to provide different priority levels to different applications|
|**Throughput**|The actual amount of data successfully transferred over a connection in a given time period|
|**Latency**|The time it takes for a packet to travel from source to destination|
|**Jitter**|Variation in the delay of received packets|
|**Packet Loss**|When packets of data traveling across a network fail to reach their destination|
|**Network Monitoring**|System that constantly monitors a network for problems or performance issues|
|**SNMP (Simple Network Management Protocol)**|Protocol for collecting and organizing information about managed devices on networks|
|**MIB (Management Information Base)**|Database used for managing entities in a communication network|
|**Network Analyzer**|Tool that captures network traffic and helps diagnose network problems|
|**Syslog**|Standard for message logging, allowing separation of software that generates messages from systems that store them|
|**NetFlow**|Network protocol developed by Cisco for collecting IP traffic information|
|**Wireshark**|Network protocol analyzer that captures and displays packet data in detail|
|**Ping**|Network utility used to test the reachability of a host and measure round-trip time|
|**Traceroute/Tracert**|Network diagnostic tool for displaying the route and measuring transit delays|
|**MTTR (Mean Time to Repair)**|Average time required to repair a failed component or system|
|**MTTF (Mean Time to Failure)**|Average time expected until the first failure of a system|
|**SLA (Service Level Agreement)**|Contract between a service provider and a customer defining the expected level of service|

## Network Infrastructure

|Term|Definition|
|---|---|
|**Router**|Network device that forwards data packets between computer networks|
|**Switch**|Network device that connects devices within a LAN and forwards data to specific destinations|
|**Hub**|Basic networking device that connects multiple Ethernet devices to create a network segment|
|**Modem**|Device that modulates and demodulates signals to enable data transmission over communication lines|
|**Access Point**|Device that allows wireless devices to connect to a wired network using Wi-Fi|
|**Gateway**|Node that connects disparate networks by translating between different network protocols|
|**Repeater**|Device that amplifies or regenerates a signal to extend the range of a network|
|**Bridge**|Device that connects and filters traffic between two network segments at the Data Link layer|
|**Patch Panel**|Hardware assembly containing ports for connecting and managing network cables|
|**Rack**|Frame for mounting multiple network devices, servers, and other computing equipment|
|**UPS (Uninterruptible Power Supply)**|Device that provides emergency power when the main power fails|
|**PDU (Power Distribution Unit)**|Device that distributes electric power to multiple network devices in a rack|
|**IDF (Intermediate Distribution Frame)**|Telecommunication framework that connects to the MDF|
|**MDF (Main Distribution Frame)**|Central telecommunications connection point|
|**Backbone**|Part of a network that connects various pieces of the network and provides a path for exchanging information|
|**Load Balancer**|Device that distributes network traffic across multiple servers|

## Cloud and Virtualization

|Term|Definition|
|---|---|
|**Cloud Computing**|Delivery of computing services over the internet|
|**IaaS (Infrastructure as a Service)**|Cloud computing service providing virtualized computing resources|
|**PaaS (Platform as a Service)**|Cloud computing service providing a platform allowing customers to develop, run, and manage applications|
|**SaaS (Software as a Service)**|Cloud computing service providing software applications over the internet|
|**Serverless Computing**|Cloud computing execution model where the cloud provider manages server allocation dynamically|
|**Virtual Machine (VM)**|Software emulation of a computer system|
|**Hypervisor**|Software, firmware, or hardware that creates and runs virtual machines|
|**Container**|Lightweight, standalone executable package that includes everything needed to run an application|
|**Docker**|Platform for developing, shipping, and running applications in containers|
|**Kubernetes**|Container orchestration system for automating deployment, scaling, and management of containerized applications|
|**Microservices**|Architectural style that structures an application as a collection of loosely coupled services|
|**Virtualization**|Creation of a virtual version of something like a server, storage device, network, or operating system|
|**Private Cloud**|Cloud infrastructure operated solely for a single organization|
|**Public Cloud**|Cloud services offered over the public internet and available to anyone who wants to purchase them|
|**Hybrid Cloud**|Computing environment that combines a private cloud with a public cloud|
|**Multi-Cloud**|Strategy of using multiple cloud computing services from different providers|
|**Edge Computing**|Distributed computing paradigm that brings computation and data storage closer to the location where it is needed|

## Internet of Things (IoT)

|Term|Definition|
|---|---|
|**IoT (Internet of Things)**|Network of physical objects embedded with sensors, software, and connectivity to exchange data with other systems|
|**M2M (Machine to Machine)**|Direct communication between devices using any communications channel|
|**Sensor**|Device that detects and responds to some type of input from the physical environment|
|**Actuator**|Component of a machine that is responsible for moving or controlling a mechanism|
|**MQTT (Message Queuing Telemetry Transport)**|Lightweight messaging protocol for small sensors and mobile devices|
|**CoAP (Constrained Application Protocol)**|Internet application protocol for constrained devices|
|**Zigbee**|Wireless technology developed for low-power, low-data-rate, close-proximity communication|
|**Z-Wave**|Wireless communications protocol used primarily for home automation|
|**Bluetooth Low Energy (BLE)**|Wireless personal area network technology designed for reduced power consumption|
|**RFID (Radio-Frequency Identification)**|Technology that uses electromagnetic fields to automatically identify and track tags attached to objects|
|**NFC (Near Field Communication)**|Set of communication protocols enabling two electronic devices to establish communication when within 4 cm|
|**Gateway**|Device that connects IoT endpoints to the broader network infrastructure|
|**Smart Home**|Home equipped with connected devices that can be remotely monitored and controlled|
|**Wearable**|Electronic device that can be worn on the body as an accessory or implant|
|**Fleet Management**|Management of commercial vehicles such as cars, vans, trucks, and construction equipment|
|**Industrial IoT (IIoT)**|Application of IoT technology in industrial settings|

## Software-Defined Networking (SDN)

|Term|Definition|
|---|---|
|**SDN (Software-Defined Networking)**|Approach to network management that enables dynamic, programmatically efficient network configuration|
|**Control Plane**|Part of the network that carries signaling traffic and is responsible for routing|
|**Data Plane**|Part of the network that carries user traffic|
|**NFV (Network Functions Virtualization)**|Concept of replacing dedicated network appliances with software running on standard servers|
|**Controller**|Central software that manages flow control to enable intelligent networking|
|**Southbound API**|Interface between the SDN controller and the network devices|
|**Northbound API**|Interface between the SDN controller and the applications and business logic|
|**OpenFlow**|Communication protocol that gives access to the forwarding plane of a network switch or router|
|**Network Slicing**|Technology that enables the creation of multiple virtual networks atop a shared physical infrastructure|
|**Intent-Based Networking**|Network administration that uses AI and machine learning to automate administrative tasks|
|**SD-WAN (Software-Defined Wide Area Network)**|Virtual WAN architecture that allows enterprises to leverage any combination of transport services|
|**Overlay Network**|Virtual network built on top of another network|
|**Underlay Network**|Physical network infrastructure over which an overlay network operates|
|**White Box Switch**|Network switch built from commodity hardware with an open operating system|
|**Network Automation**|Process of automating the configuration, management, and operations of a network|

## Mobile and Wireless Networks

|Term|Definition|
|---|---|
|**Cellular Network**|Communication network where the last link is wireless, distributed over land areas called cells|
|**GSM (Global System for Mobile Communications)**|Standard developed to describe the protocols for second-generation digital cellular networks|
|**CDMA (Code Division Multiple Access)**|Channel access method used by various radio communication technologies|
|**LTE (Long-Term Evolution)**|Standard for wireless broadband communication for mobile devices|
|**5G**|Fifth generation technology standard for cellular networks|
|**Wi-Fi**|Technology for local area wireless computer networking|
|**Wi-Fi 6 (802.11ax)**|Latest generation of Wi-Fi technology, providing faster speeds and better performance in congested areas|
|**SSID (Service Set Identifier)**|Name assigned to a wireless network|
|**WEP (Wired Equivalent Privacy)**|Security algorithm for IEEE 802.11 wireless networks (obsolete due to security weaknesses)|
|**WPA/WPA2/WPA3 (Wi-Fi Protected Access)**|Security certification programs developed by the Wi-Fi Alliance|
|**Roaming**|Ability for a wireless device to move from one access point's coverage area to another without disconnecting|
|**Handover/Handoff**|Process of transferring an ongoing call or data session from one channel to another|
|**Wireless Mesh Network**|Network topology where each node can relay data for the network|
|**MIMO (Multiple Input Multiple Output)**|Method for multiplying the capacity of a radio link using multiple transmit and receive antennas|
|**Beamforming**|Signal processing technique used to direct wireless signals toward a specific receiving device|
|**Channel Bonding**|Combining multiple channels to increase throughput|

## Advanced Security Concepts

|Term|Definition|
|---|---|
|**Zero Trust**|Security model that requires strict identity verification for every person and device trying to access resources|
|**MFA (Multi-Factor Authentication)**|Authentication method requiring users to provide two or more verification factors|
|**Biometrics**|Metrics related to human characteristics used for authentication|
|**SIEM (Security Information and Event Management)**|System providing real-time analysis of security alerts generated by applications and network hardware|
|**SOC (Security Operations Center)**|Facility where information security events are monitored, assessed, and defended|
|**Penetration Testing**|Authorized simulated cyberattack on a computer system to evaluate security|
|**Vulnerability Assessment**|Process of identifying, quantifying, and prioritizing vulnerabilities in systems|
|**SOAR (Security Orchestration, Automation and Response)**|Stack of solutions that allows organizations to collect security data and respond to security events|
|**EDR (Endpoint Detection and Response)**|Cybersecurity technology that addresses and monitors endpoint threats|
|**XDR (Extended Detection and Response)**|Security technology that provides extended visibility across networks, clouds, and endpoints|
|**Threat Intelligence**|Evidence-based knowledge about existing or emerging threats to assets|
|**Encryption at Rest**|Data encryption when stored on a device|
|**Encryption in Transit**|Data encryption when it's being transmitted over a network|
|**Key Management**|Administration of cryptographic keys in a cryptosystem|
|**Sandbox**|Security mechanism for separating running programs, often used to execute untested code|
|**CASB (Cloud Access Security Broker)**|Security policy enforcement point placed between cloud service consumers and providers|

## Network Protocols and Standards

|Term|Definition|
|---|---|
|**IEEE 802.3 (Ethernet)**|Technical standard defining the physical layer and data link layer's MAC of wired Ethernet|
|**IEEE 802.11 (Wi-Fi)**|Set of standards for implementing wireless local area network computer communication|
|**IEEE 802.1Q (VLAN Tagging)**|Networking standard that supports VLANs on an IEEE 802.3 Ethernet network|
|**IEEE 802.1X**|Standard for port-based network access control|
|**SIP (Session Initiation Protocol)**|Signaling protocol for initiating, maintaining, and terminating real-time sessions|
|**RTP (Real-time Transport Protocol)**|Network protocol for delivering audio and video over IP networks|
|**H.323**|Standard providing audio-visual communication sessions on any packet network|
|**LDAP (Lightweight Directory Access Protocol)**|Protocol for accessing and maintaining distributed directory information services|
|**Kerberos**|Network authentication protocol designed to provide strong authentication for client/server applications|
|**RADIUS (Remote Authentication Dial-In User Service)**|Networking protocol providing centralized authentication, authorization, and accounting|
|**TACACS+ (Terminal Access Controller Access-Control System Plus)**|Protocol that handles authentication, authorization, and accounting for network devices|
|**IPAM (IP Address Management)**|Methodology of planning, tracking, and managing IP address space in a network|
|**BGP (Border Gateway Protocol)**|Standardized exterior gateway protocol designed to exchange routing information between autonomous systems|
|**MPLS (Multiprotocol Label Switching)**|Routing technique in telecommunications networks that directs data based on short path labels|
|**IPX/SPX (Internetwork Packet Exchange/Sequenced Packet Exchange)**|Networking protocol used by Novell NetWare systems|
|**AppleTalk**|Proprietary suite of networking protocols developed by Apple for their Macintosh computers|

## Troubleshooting and Diagnostics

|Term|Definition|
|---|---|
|**Ping**|Network utility used to test the reachability of a host and measure round-trip time|
|**Traceroute/Tracert**|Network diagnostic tool for displaying the route and measuring transit delays|
|**Netstat**|Command-line tool that displays network connections, routing tables, and interface statistics|
|**Nslookup/Dig**|Network administration tools for querying DNS servers|
|**Packet Sniffing**|Process of capturing and logging traffic that passes over a network|
|**Wireshark**|Network protocol analyzer that captures and displays packet data in detail|
|**tcpdump**|Command-line packet analyzer|
|**Bandwidth Testing**|Process of determining the maximum throughput of a network link or path|
|**Loopback Test**|Test in which a signal is sent from a communications device and returned to the same device|
|**MTR (My Traceroute)**|Network diagnostic tool that combines the functionality of traceroute and ping|
|**Network Baseline**|Documentation of network performance under normal conditions|
|**MTBF (Mean Time Between Failures)**|Average time between system breakdowns|
|**MTTR (Mean Time to Repair)**|Average time required to repair a failed component or system|
|**Root Cause Analysis**|Method of problem solving used for identifying the primary source of faults or problems|
|**Escalation Procedure**|Process for increasing the level of support when resolving difficult issues|
|**Trouble Ticket**|Documentation of reported issues and their resolution|

## Emerging Technologies

| Term                                                  | Definition                                                                                               |
| ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **5G**                                                | Fifth generation technology standard for cellular networks                                               |
| **Wi-Fi 6/6E**                                        | Latest generation of Wi-Fi technology, providing faster speeds and better performance in congested areas |
| **Quantum Networking**                                | Application of quantum physics to networking infrastructure                                              |
| **Network Automation**                                | Process of automating the configuration, management, and operations of a network                         |
| **AIOps (Artificial Intelligence for IT Operations)** | Application of AI to enhance IT operations                                                               |
| **Intent-Based Networking**                           | Network administration that uses AI and machine learning to automate administrative tasks                |
| **Blockchain**                                        | Distributed ledger technology that records transactions across multiple computers                        |
| **SASE (Secure Access Service Edge)**                 | Network architecture that combines VPN and SD-WAN capabilities with cloud-native security functions      |
| **Low Earth Orbit (LEO) Satellite Internet**          | Internet connectivity provided by a constellation of satellites in low Earth orbit                       |
| **Private 5G**                                        | 5G network deployed for a specific enterprise or location                                                |
| **Network as Code**                                   | Approach to network automation in which networking engineers define configuration as code                |
| **Digital Twin**                                      | Virtual representation of a physical object or system                                                    |
| **Li-Fi (Light Fidelity)**                            | Wireless communication technology that uses light to transmit data                                       |
| **Quantum Encryption**                                | Encryption methods that use principles of quantum mechanics to secure data                               |
| **6G**                                                | Sixth generation cellular network technology (still in research and development)                         |
| **Network Slicing**                                   | Technology that enables the creation of multiple virtual networks atop a shared physical infrastructure  |