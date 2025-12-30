## 🌐 1. What is a Network?

A **network** is a system where multiple computers/devices are connected to exchange data or share resources (internet, files, printers).

---

## 🏠 2. Types of Networks

| Type    | Description                            |
| ------- | -------------------------------------- |
| **LAN** | Local Area Network (home/office)       |
| **WAN** | Wide Area Network (Internet)           |
| **MAN** | Metropolitan Area Network (city)       |
| **PAN** | Personal Area Network (Bluetooth, USB) |

---

## 🧱 3. OSI Model (7 Layers)

diff
`+-------------------+ Layer 7 – Application    (e.g., HTTP, FTP, DNS)
`+-------------------+ Layer 6 – Presentation   (e.g., Encryption, compression)
`+-------------------+ Layer 5 – Session        (e.g., Open/close sessions)
`+-------------------+ Layer 4 – Transport      (e.g., TCP, UDP)
`+-------------------+ Layer 3 – Network        (e.g., IP addressing, routing)
`+-------------------+ Layer 2 – Data Link      (e.g., MAC, Ethernet)
`+-------------------+ Layer 1 – Physical       (e.g., Cables, Wi-Fi signals)

---

## 📡 4. Network Devices

| Device            | Function                              |
| ----------------- | ------------------------------------- |
| Router            | Connects LAN to Internet, assigns IPs |
| Switch            | Connects devices within a LAN         |
| Modem             | Connects to your ISP                  |
| Firewall          | Blocks/filters traffic                |
| AP (Access Point) | Provides wireless (Wi-Fi) access      |

---

## 📊 5. Network Performance Metrics

|Term|Meaning|
|---|---|
|**Bandwidth**|Max data transfer rate (measured in Mbps or Gbps)|
|**Latency**|Delay between sending and receiving (measured in ms)|
|**Jitter**|Variation in latency (important for voice/video)|
|**Packet Loss**|% of lost packets in transmission (causes buffering or lag)|

---

## 📦 6. TCP (Transmission Control Protocol)

|Feature|Value|
|---|---|
|Reliable|✅ Yes|
|Ordered|✅ Yes|
|Connection-based|✅ Yes|
|Slower|🐢 Yes|

### 🔄 TCP 3-Way Handshake
`Client               Server`
  `| ---- SYN ----->   |  "Start connection"`
  `| <--- SYN-ACK ---  |  "Okay, I’m ready"`
  `| ---- ACK ----->   |  "Confirmed"``
### TCP Flags

|Flag|Description|
|---|---|
|SYN|Start a connection|
|ACK|Acknowledge data|
|FIN|Finish connection|
|RST|Reset connection|
|PSH|Push data immediately|
|URG|Urgent data present|

---

## 🚀 7. UDP (User Datagram Protocol)

|Feature|Value|
|---|---|
|Reliable|❌ No|
|Ordered|❌ No|
|Fast|✅ Yes|
|Connectionless|✅ Yes|

**Used for**: DNS, online games, VoIP, video streaming

---

## 🧪 8. ICMP (Internet Control Message Protocol)

- Used for diagnostics, not for actual data transfer
    
- Helps identify **network issues**
    

**Examples:**

- `ping` — checks if a host is reachable
    
- `traceroute` — shows the path packets take
    

---

## 🌍 9. IP Addressing

- **IPv4**: `192.168.1.1`
    
- **IPv6**: `2001:db8::1`
    
- **Private IPs**: Used in LANs (`192.168.x.x`, `10.x.x.x`)
    
- **Public IPs**: Reachable over the internet
    

---

## 🧠 10. Subnetting

Subnetting divides a network into smaller networks for:

- Better performance
    
- Security
    
- IP management
    

**CIDR Notation:**

- `/24` → 255.255.255.0 → 256 IPs
    
- `/16` → 255.255.0.0 → 65,536 IPs
    

---

## 🔁 11. NAT (Network Address Translation)

Translates **private IPs** (inside LAN) into **public IPs** for internet communication.

scss



`192.168.1.10  →  [Router NAT]`
`→  Public IP (e.g., 100.25.78.5)`

Types:

- **SNAT** – Source NAT (outbound)
    
- **DNAT** – Destination NAT (inbound)
    
- **PAT** – Port Address Translation (many-to-one)
    

---

## 🛠️ 12. DHCP (Dynamic Host Configuration Protocol)

Automatically assigns:

- IP address
    
- Subnet mask
    
- Gateway
    
- DNS servers
    

**Uses UDP ports 67 (server) and 68 (client)**

---

## 🔐 13. Security: Firewall, Proxy, VPN

### 🔥 Firewall

- Filters incoming/outgoing traffic based on rules
    

### 🧰 Proxy Server

- Acts as a **middleman** between your device and the internet
    
- Caches web content and hides your IP
    

### 🛡️ VPN (Virtual Private Network)

- Encrypts your traffic
    
- Hides your IP
    
- Bypasses geo-blocks
    

---

## ⚙️ 14. Quality of Service (QoS)

Used to **prioritize traffic**:

- Voice > Downloads
    
- Games > Background apps
    

Useful for:

- Reducing **jitter** and **lag**
    
- Managing **bandwidth usage**
    

---

## 🌐 15. DNS (Domain Name System)

**Converts domain names → IP addresses**

`google.com → 142.250.182.206`

DNS Query Flow:

1. Client → Resolver
    
2. Resolver → Root Server
    
3. Root → TLD Server
    
4. TLD → Authoritative DNS
    
5. IP returned to client
    



### 1. What is TLD?

- **TLD (Top-Level Domain):** The highest level of the DNS hierarchy after the root.
    
- Examples:
    
    - **gTLDs (Generic):** `.com`, `.org`, `.net`, `.xyz`
        
    - **ccTLDs (Country Code):** `.uk`, `.jp`, `.in`, `.us`
        

---

### 2. DNS Hierarchy

1. **Root Zone (`.`)**
    
    - Managed by **IANA/ICANN**.
        
    - Points to TLD servers.
        
2. **TLD Zone (e.g., `.com`, `.org`)**
    
    - Managed by **registry operators** (e.g., Verisign for `.com`).
        
    - Holds records of which authoritative servers manage each domain.
        
3. **Second-Level Domain (SLD)**
    
    - Directly under TLD.
        
    - Example: `example.com` → `example` is SLD.
        
4. **Subdomains**
    
    - Under SLD.
        
    - Example: `mail.example.com`, `www.example.com`.
        

---

### 3. DNS Resolution Process

When you type **www.example.com**

1. Resolver → **Root Server** → referral to `.com` TLD servers.
    
2. Resolver → **TLD Server** → referral to `example.com` authoritative servers.
    
3. Resolver → **Authoritative Server** → IP address of `www.example.com`.
    
4. Browser → connects to IP → website loads.
    

---

### 4. TLD DNS Servers

- Each TLD has multiple **authoritative name servers**.
    
- Example for `.com`:
    
    `a.gtld-servers.net b.gtld-servers.net ...`
    
- Ensure global redundancy and reliability.
    

---

### 5. Key Points

- TLDs are managed by **registries**.
    
- Registrars sell domains under TLDs.
    
- Root → TLD → SLD → Subdomain.
    
- TLD DNS ensures scalability and delegation in the internet naming system.
---

## 📏 16. MTU (Maximum Transmission Unit)

- Max size of a packet = usually **1500 bytes**
    
- Too high = packet dropped
    
- Too low = inefficient
    

> Use `ping -f -l` to test for optimal MTU

---

## 📊 17. Common Protocols and Ports

|Protocol|Port|Type|Use|
|---|---|---|---|
|HTTP|80|TCP|Web traffic|
|HTTPS|443|TCP|Secure web|
|FTP|21|TCP|File transfer|
|DNS|53|UDP|Domain name|
|DHCP|67/68|UDP|IP assignment|
|SSH|22|TCP|Secure remote access|
|SNMP|161|UDP|Network monitoring|

---

## 🔍 18. Network Troubleshooting Tools

|Tool|Description|
|---|---|
|`ping`|Checks connectivity|
|`tracert` / `traceroute`|Shows route to host|
|`ipconfig` / `ifconfig`|IP settings|
|`netstat`|Show open ports|
|`nslookup`|DNS lookup|
|`Wireshark`|Packet analysis|

---

## 📊 19. TCP vs UDP Summary

|Feature|TCP|UDP|
|---|---|---|
|Reliable|✅ Yes|❌ No|
|Ordered|✅ Yes|❌ No|
|Speed|🐢 Slower|⚡ Faster|
|Handshake|✅ Yes (3-way)|❌ No|
|Use Cases|Web, email|Games, VoIP|



[[index]]
