# Network Commands and Examples
By Joshua Hall, Edited by Claude.ai
## Part 1: Network Command Reference Charts

### Basic Network Diagnostics

|Command|Purpose|Basic Syntax|
|---|---|---|
|ping|Tests connectivity to a host|`ping hostname`|
|traceroute/tracert|Shows path to destination|`traceroute hostname`|
|nslookup|DNS lookup utility|`nslookup domain.com`|
|dig|DNS lookup with more details|`dig domain.com`|
|host|Simple DNS lookup|`host domain.com`|
|ifconfig/ipconfig|Shows network interface details|`ifconfig` or `ipconfig`|
|netstat|Network statistics|`netstat -an`|
|route|Show/manipulate routing table|`route -n`|
|arp|Address Resolution Protocol cache|`arp -a`|
|nmap|Network exploration and security auditing|`nmap target`|

### Network Performance Tools

|Command|Purpose|Basic Syntax|
|---|---|---|
|iperf/iperf3|Network bandwidth testing|`iperf -c server`|
|mtr|Combines ping and traceroute|`mtr hostname`|
|speedtest-cli|Tests internet bandwidth|`speedtest-cli`|
|tcpdump|Packet analyzer|`tcpdump -i interface`|
|netcat (nc)|TCP/IP swiss army knife|`nc -v host port`|
|ss|Socket statistics|`ss -tuln`|
|wireshark|Packet capture and analysis|`wireshark` (GUI)|
|curl|Client for URLs|`curl -v website.com`|
|wget|Retrieve files from web|`wget https://website.com/file`|

### Remote Access Commands

|Command|Purpose|Basic Syntax|
|---|---|---|
|ssh|Secure Shell|`ssh user@hostname`|
|scp|Secure copy (file transfer)|`scp file user@host:/path`|
|telnet|Unencrypted terminal emulation|`telnet host port`|
|ftp|File Transfer Protocol|`ftp hostname`|
|sftp|Secure File Transfer Protocol|`sftp user@hostname`|
|rsync|Fast, versatile file copying tool|`rsync -av source/ destination/`|

## Part 2: Network Exploration Examples

### Network Discovery and Mapping

```bash
# Discover active hosts on the local network
nmap -sP 192.168.1.0/24

# Port scanning a specific host
nmap -sS -sV 192.168.1.10

# OS detection
nmap -O 192.168.1.10

# Create a visual network map
# (requires zenmap, the GUI for nmap)
zenmap
```

### DNS Exploration

```bash
# Basic DNS lookup
nslookup example.com

# Detailed DNS information
dig example.com

# Reverse DNS lookup
host 8.8.8.8

# Check DNS records of specific type
dig example.com MX
dig example.com NS
dig example.com TXT

# Trace DNS delegation
dig +trace example.com
```

### Network Troubleshooting

```bash
# Test connectivity with increasing payload size
ping -s 1500 example.com

# Check path to destination with timing
traceroute -T example.com

# More detailed path analysis with both ICMP and TCP
mtr example.com

# Check if a specific port is open
nc -zv example.com 443

# Monitor network connections
netstat -tunap | grep ESTABLISHED

# Analyze TCP handshake
tcpdump -i eth0 -n "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0 and port 80"
```

### Network Performance Testing

```bash
# Run iperf server
iperf -s

# Run iperf client to test bandwidth
iperf -c server_ip -t 30

# Test download/upload speeds
speedtest-cli

# Measure latency over time
ping -c 100 example.com | grep time=
```

## Part 3: HTTP Request Examples

### Using curl for HTTP Requests

```bash
# Basic GET request
curl https://example.com

# GET request with verbose output
curl -v https://example.com

# POST request with data
curl -X POST https://example.com/api -d "name=value"

# PUT request with JSON data
curl -X PUT https://example.com/api/resource/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"New Name","value":42}'

# HEAD request (headers only)
curl -I https://example.com

# Follow redirects
curl -L https://example.com

# Save output to file
curl -o output.html https://example.com

# Use HTTP basic authentication
curl -u username:password https://example.com/protected
```

### Using wget for File Downloads

```bash
# Download a file
wget https://example.com/file.zip

# Download with retry
wget -t 3 -c https://example.com/largefile.iso

# Mirror a website
wget --mirror --convert-links --page-requisites --no-parent -P /target/directory https://example.com
```

### Using netcat for HTTP

```bash
# Manual HTTP request with netcat
echo -e "GET / HTTP/1.1\r\nHost: example.com\r\n\r\n" | nc example.com 80

# Start a simple HTTP server on port 8080
while true; do { echo -e "HTTP/1.1 200 OK\r\n"; echo -e "Content-Type: text/html\r\n\r\n"; echo -e "<html><body><h1>Hello World</h1></body></html>\r\n"; } | nc -l 8080; done
```

## Part 4: Advanced Network Analysis

### Packet Capture and Analysis

```bash
# Capture TCP traffic on port 80
tcpdump -i eth0 tcp port 80 -w capture.pcap

# Capture DNS traffic
tcpdump -i eth0 udp port 53 -n

# Capture HTTP headers only
tcpdump -i eth0 -s 0 -A 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'

# Analyze captured packets
tcpdump -r capture.pcap -n

# Filter packets for a specific host
tcpdump -i eth0 host 192.168.1.10
```

### Network Socket Analysis

```bash
# List all listening TCP ports
ss -tlnp

# List established connections
ss -tp

# Show socket statistics
ss -s

# Show detailed socket information
ss -ei

# Monitor socket changes
watch -n 1 'ss -t4'
```

### Network Performance Diagnostics

```bash
# Check route to destination
ip route get 8.8.8.8

# Show detailed interface statistics
ip -s link show eth0

# Monitor network activity in real-time
iftop -i eth0

# Check network bandwidth usage by process
nethogs eth0

# Analyze network latency issues
hping3 -c 10 -S -p 80 example.com
```

### Secure Network Communication

```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096

# Copy SSH key to remote server
ssh-copy-id user@hostname

# Create an SSH tunnel
ssh -L 8080:localhost:80 user@remote_host

# Create a reverse SSH tunnel
ssh -R 8080:localhost:80 user@remote_host

# Use SSH as a SOCKS proxy
ssh -D 1080 user@remote_host
```

## Part 5: Network Configuration Examples

### Interface Configuration

```bash
# Show interface configuration
ip addr show

# Add IP address to interface
ip addr add 192.168.1.10/24 dev eth0

# Bring interface up/down
ip link set eth0 up
ip link set eth0 down

# Configure interface with DHCP
dhclient eth0

# Set static IP in /etc/network/interfaces (Debian/Ubuntu)
auto eth0
iface eth0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

### Routing Configuration

```bash
# Show routing table
ip route show

# Add a static route
ip route add 10.0.0.0/24 via 192.168.1.1

# Add a default route
ip route add default via 192.168.1.1

# Delete a route
ip route del 10.0.0.0/24

# Save routing table permanently (Debian/Ubuntu)
# Add to /etc/network/interfaces:
post-up ip route add 10.0.0.0/24 via 192.168.1.1
```

### Firewall Configuration (iptables)

```bash
# List all firewall rules
iptables -L -v

# Allow SSH traffic
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS traffic
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Block a specific IP address
iptables -A INPUT -s 10.0.0.5 -j DROP

# Save iptables rules (Debian/Ubuntu)
iptables-save > /etc/iptables/rules.v4
```

### DNS Configuration

```bash
# Edit resolv.conf for DNS servers
# /etc/resolv.conf
nameserver 8.8.8.8
nameserver 8.8.4.4
search example.com

# Flush DNS cache (Linux)
systemd-resolve --flush-caches

# Flush DNS cache (macOS)
dscacheutil -flushcache

# Flush DNS cache (Windows)
ipconfig /flushdns
```

## Part 6: Monitoring and Debugging Examples

### Real-time Network Monitoring

```bash
# Monitor network connections
watch -n 1 'netstat -tulanp'

# Visualize network traffic with iftop
iftop -i eth0

# Monitor bandwidth by process
nethogs eth0

# Monitor DNS queries
tcpdump -i eth0 -n udp port 53

# Monitor HTTP requests
tcpdump -i eth0 -A -s 0 'tcp port 80'
```

### Bandwidth Testing and Quality of Service

```bash
# Server side
iperf3 -s

# Client side with multiple streams
iperf3 -c server_ip -P 4 -t 30

# Bidirectional test
iperf3 -c server_ip -d

# Test with specified bandwidth
iperf3 -c server_ip -b 10M

# Test UDP performance
iperf3 -c server_ip -u -b 100M
```

### Network Path Analysis

```bash
# Basic traceroute
traceroute example.com

# TCP traceroute (bypass firewalls)
traceroute -T -p 80 example.com

# UDP traceroute
traceroute -U example.com

# Combine ping and traceroute for continuous monitoring
mtr example.com

# Detailed hop analysis
tracepath example.com
```

### Scanning and Security Assessment

```bash
# Scan network for open ports
nmap -p 1-1000 192.168.1.0/24

# Identify services running on ports
nmap -sV 192.168.1.10

# Check for common vulnerabilities
nmap --script vuln 192.168.1.10

# Scan for specific ports across a network
nmap -p 22,80,443 192.168.1.0/24

# Aggressive scan with OS detection
nmap -A 192.168.1.10
```