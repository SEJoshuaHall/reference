# Network Supplemental Guide
By Joshua Hall, Edited by Claude.ai

This guide compiles essential network-related information from the provided materials, focusing on practical examples, tools, and concepts not fully covered in the main reference documents.

## Table of Contents

1. [The Internet and OSI Model](https://claude.ai/chat/a818297f-40a9-42b2-a587-b3f2c7198399#the-internet-and-osi-model)
2. [Network Diagnostic Tools](https://claude.ai/chat/a818297f-40a9-42b2-a587-b3f2c7198399#network-diagnostic-tools)
3. [HTTP and Web Communication](https://claude.ai/chat/a818297f-40a9-42b2-a587-b3f2c7198399#http-and-web-communication)
4. [Security Concepts](https://claude.ai/chat/a818297f-40a9-42b2-a587-b3f2c7198399#security-concepts)
5. [Command Line Networking](https://claude.ai/chat/a818297f-40a9-42b2-a587-b3f2c7198399#command-line-networking)
6. [Server-Side Development](https://claude.ai/chat/a818297f-40a9-42b2-a587-b3f2c7198399#server-side-development)
7. [Deployment and Cloud Services](https://claude.ai/chat/a818297f-40a9-42b2-a587-b3f2c7198399#deployment-and-cloud-services)

## The Internet and OSI Model

### Internet vs. TCP/IP Model Comparison

![Diagram comparing OSI model layers with TCP/IP layers](https://da77jsbdz4r05.cloudfront.net/images/ls170/layered-system-osi-tcp-ip-comparison.png)

The TCP/IP model and the OSI model are two different frameworks for understanding network communications. While the TCP/IP model is more practical and widely used for internet communications, the OSI model provides a more detailed conceptual framework.

### Key Internet Characteristics

- The internet is a vast _network of networks_ comprising both the physical infrastructure (devices, routers, cables) and the protocols enabling it to function.
- Physical network limitations include:
    - **Latency**: The delay between sending and receiving data, measured in milliseconds (ms)
    - **Bandwidth**: The maximum data transfer capacity of a network connection

### Summary of TCP/IP Layers

1. **Link Layer**
    
    - Responsible for physical addressing using MAC addresses
    - Primarily uses Ethernet for local network communication
    - Devices include switches and network interface cards
2. **Internet Layer**
    
    - Uses IP addressing for logical device identification
    - Handles routing between networks
    - IPv4 and IPv6 provide the addressing schemes used
3. **Transport Layer**
    
    - TCP provides reliable, connection-oriented communication
    - UDP provides fast, connectionless communication
    - Multiplexing allows multiple applications to share the network
4. **Application Layer**
    
    - Provides protocols that directly serve end-user applications
    - Examples include HTTP, DNS, FTP, SMTP

## Network Diagnostic Tools

### Command Line Tools

#### Curl

```bash
# Basic GET request
curl www.google.com

# GET request with verbose output
curl -X GET "https://www.reddit.com/" -m 30 -v

# POST request
curl -X POST "https://echo.epa.gov" -m 30 -v
```

The `-v` flag provides verbose output, showing the request and response headers.

#### Telnet

Telnet can be used to explore HTTP by establishing direct connections to web servers:

```bash
telnet example.com 80
GET / HTTP/1.1
Host: example.com
[press Enter twice to send]
```

#### Netcat (nc)

Netcat is a versatile networking utility that can be used as both a client and server:

```bash
# Client mode (connect to a server)
nc -v google.com 80

# Server mode (listen on a port)
nc -lvp 2345
```

The `-l` flag puts netcat in listen mode, `-v` enables verbose output, and `-p` specifies the port (required on macOS).

### Status Codes

|Status Code|Status Text|Meaning|
|---|---|---|
|200|OK|The request was handled successfully.|
|302|Found|The requested resource has changed temporarily. Usually results in a redirect to another URL.|
|404|Not Found|The requested resource cannot be found.|
|500|Internal Server Error|The server has encountered a generic error.|

### HTTP Response Headers

|Header Name|Description|Example|
|---|---|---|
|Content-Encoding|The type of encoding used on the data.|`Content-Encoding: gzip`|
|Server|Name of the server.|`Server:thin 1.5.0 codename Knife`|
|Location|Notify client of new resource location.|`Location: https://www.github.com/login`|
|Content-Type|The type of data the response contains.|`Content-Type:text/html; charset=UTF-8`|

## HTTP and Web Communication

### HTTP Request/Response Cycle

HTTP is the protocol that enables web communication through a request-response model:

1. Client sends a request to a server
2. Server processes the request
3. Server returns a response to the client
4. Client processes the response (e.g., renders a webpage)

### URL Components

A URL (Uniform Resource Locator) consists of several components:

- **Scheme**: The protocol (e.g., http, https)
- **Host**: The domain name or IP address
- **Port**: The port number (optional, defaults depend on scheme)
- **Path**: The resource path on the server
- **Query string**: Additional parameters (after ?)
- **Fragment**: Reference to a section within a resource (after #)

Example: `https://www.example.com:443/products/search?category=books&author=smith#section2`

### HTTP State Management

HTTP is a stateless protocol, but several mechanisms can simulate state:

- **Sessions**: Server-side storage identified by a session ID
- **Cookies**: Small data pieces stored by browsers
- **AJAX (Asynchronous JavaScript and XML)**: Allows asynchronous communication

### HTML Elements Reference

HTML5 element selection flowchart for understanding semantic element usage:

![HTML5 Element Flowchart](https://raw.githubusercontent.com/html5doctor/html5-semantics-flowchart/gh-pages/html5-semantic-flowchart-v1.5.png)

## Security Concepts

### TLS/SSL

Transport Layer Security (TLS) provides security for HTTP communications through:

- **Encryption**: Encodes data to prevent unauthorized access
- **Authentication**: Verifies server identity through certificates
- **Integrity**: Ensures data hasn't been altered in transit

The TLS handshake process is used to:

- Agree on the TLS version
- Select cryptographic algorithms (cipher suite)
- Exchange and validate keys

### Common Security Threats

- **Packet Sniffing**: Capturing and analyzing unencrypted network traffic
- **Man-in-the-Middle Attacks**: Intercepting and potentially altering communications
- **Session Hijacking**: Stealing session tokens to impersonate legitimate users
- **Cross-site Scripting (XSS)**: Injecting malicious scripts into trusted websites

### Security Mitigation Techniques

- **HTTPS**: Using TLS/SSL encryption for HTTP communications
- **Same-origin Policy**: Restricting how documents or scripts from one origin interact with resources from another origin
- **Content Security Policy**: Restricting which resources can be loaded
- **Input Sanitization**: Cleaning user inputs to prevent code injection

## Command Line Networking

### Bash Commands for Networking

```bash
# Display network interfaces
ifconfig
ip addr show

# Test connectivity
ping example.com

# Trace route to host
traceroute example.com

# View active network connections
netstat -an
ss -tuln

# DNS lookup
nslookup example.com
dig example.com

# Check for open ports
nmap -p 80,443 example.com
```

### Bash Scripting for Network Tasks

Bash supports creating scripts to automate networking tasks:

```bash
#!/bin/bash

# Simple script to check if a website is up
echo "Checking website availability..."
read -p "Enter website URL: " website

if ping -c 1 $website &> /dev/null
then
  echo "$website is reachable!"
else
  echo "$website is down or unreachable."
fi
```

Save this as a `.sh` file, make it executable with `chmod +x filename.sh`, and run it with `./filename.sh`.

### Bash Control Structures

Conditional statements and loops can be used for network tasks:

```bash
# Check if port is open
if nc -z example.com 80; then
  echo "Port 80 is open"
else
  echo "Port 80 is closed"
fi

# Monitor website availability
counter=0
while [ $counter -lt 5 ]; do
  if curl -s --head example.com > /dev/null; then
    echo "Website is up"
  else
    echo "Website is down"
  fi
  sleep 60
  ((counter++))
done
```

## Server-Side Development

### Basic Server in Ruby

```ruby
# server.rb
require "socket"

def parse_request(request_line)
  http_method, path_and_params, http = request_line.split(" ")
  path, params = path_and_params.split("?")

  params = (params || '').split("&").each_with_object({}) do |pair, hash|
    key, value = pair.split("=")
    hash[key] = value
  end

  [http_method, path, params]
end

server = TCPServer.new("localhost", 3003)

loop do
  client = server.accept

  request_line = client.gets
  next if !request_line || request_line =~ /favicon/
  puts request_line
  
  next unless request_line

  http_method, path, params = parse_request(request_line)

  client.puts "HTTP/1.1 200 OK"
  client.puts "Content-Type: text/html\r\n\r\n"
  client.puts
  client.puts "<html>"
  client.puts "<body>"
  client.puts "<pre>"
  client.puts http_method
  client.puts path
  client.puts params
  client.puts "</pre>"

  # Additional handling code would go here

  client.puts "</body>"
  client.puts "</html>"

  client.close
end
```

To run this server:

```bash
ruby server.rb
```

Then access it in a browser at `localhost:3003`

### Rack Framework

Rack provides an interface between Ruby web applications and web servers:

```ruby
# config.ru
require_relative 'app'
run App.new

# app.rb
class App
  def call(env)
    [200, {"Content-Type" => "text/html"}, ["Hello World"]]
  end
end
```

Start a Rack application with:

```bash
bundle exec rackup config.ru -p 9595
```

Test it with:

```bash
curl -X GET localhost:9595 -m 30 -v
```

Or in a browser: http://localhost:9595/

### Sinatra Framework

Sinatra is a simple DSL for creating web applications in Ruby:

```ruby
require "sinatra"
require "sinatra/reloader"

get "/" do 
  File.read "public/template.html" 
end
```

The `reloader` extension causes the application to reload on each page load during development.

### View Templates with ERB

ERB (Embedded Ruby) allows embedding Ruby code in HTML:

```erb
<!-- example.erb -->
<% names = ['bob', 'joe', 'kim', 'jill'] %>
<html>  
  <body>  
    <h4>Hello, my name is <%= names.sample %></h4>  
  </body>  
</html>
```

Used in Ruby with:

```ruby
require 'erb'
template_file = File.read('example.erb')  
erb = ERB.new(template_file)  
erb.result
```

## Deployment and Cloud Services

### Heroku Deployment

Deploy a Ruby application to Heroku:

1. Create a Heroku account
    
2. Install the Heroku CLI
    
3. Prepare your application:
    
    - Set up a Git repository
    - Remove debugging code
    - Configure for production
4. Create necessary configuration files:
    
    ```ruby
    # Gemfile (specify Ruby version)
    source "https://rubygems.org"
    ruby "3.1.0"
    
    # ... your gems ...
    
    group :production do
      gem "puma"
    end
    ```
    
    ```ruby
    # config.ru
    require "./your_app_name"
    run Sinatra::Application
    ```
    
    ```
    # Procfile
    web: bundle exec puma -t 5:5 -p ${PORT:-3000} -e ${RACK_ENV:-development}
    ```
    
5. Test locally:
    
    ```bash
    heroku local
    ```
    
6. Deploy:
    
    ```bash
    heroku create app_name
    bundle lock --add-platform x86_64-linux
    git push heroku main
    ```
    

### Security for Web Applications

HTML input sanitization is crucial for web security:

```ruby
# Manual sanitization in Rack
helpers do
  def h(content)
    Rack::Utils.escape_html(content)
  end
end

# In a template
<h3><%=h todo[:name] %></h3>

# Automatic sanitization in Sinatra
configure do
  set :erb, :escape_html => true
end
```

When automatic sanitization is enabled, you need to use `<%==` instead of `<%=` when you want to output HTML that shouldn't be escaped.

## Additional Resources

For more information about networking concepts and tools, you can refer to:

- [Internet Protocol Suite on Wikipedia](https://en.wikipedia.org/wiki/Internet_protocol_suite)
- [HTTP Documentation on MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP)
- [WebPlatform](https://webplatform.github.io/) for web development standards
- [Ruby Documentation](https://ruby-doc.org/) for Ruby programming
- [Heroku Dev Center](https://devcenter.heroku.com/) for Heroku deployment