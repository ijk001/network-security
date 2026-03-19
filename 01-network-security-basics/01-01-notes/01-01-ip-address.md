# 🌐 01-01: IP Address Basics

---

## 📌 What is an IP Address?

An **IP Address (Internet Protocol Address)** uniquely identifies a device on a network.

- Works like a **home address** for devices  
- Used for **communication between devices**  
- Example: `192.168.1.1`  

---

## 🔹 Types of IP Address

- **IPv4** (most commonly used)  
- **IPv6** (newer version, larger address space)  

### IPv6 Address (Short Idea)

- 128-bit address  
- Written in 8 hexadecimal blocks  

Example:  
`2001:0db8:85a3:0000:0000:8a2e:0370:7334`

💡 Created to solve IPv4 address exhaustion  

---

## 🔹 IPv4 Address Structure

An IPv4 address consists of:

- **32 bits**  
- Divided into **4 octets (8 bits each)**  
- Written in **decimal format**  

Example:  
`192.168.60.5`

Binary representation:  
`11000000.10101000.00111100.00000101`

---

## 🔹 Network ID vs Host ID

An IP address has two parts:

| Part       | Description |
|------------|------------|
| Network ID | Identifies the network |
| Host ID    | Identifies the device |

Example: `192.168.60.5/24`

- `/24` → first 24 bits = Network  
- Remaining 8 bits = Host  

---

## 🔹 Subnet Mask

A **subnet mask** defines which part of an IP is:

- Network portion  
- Host portion  

Example:  
`11111111.11111111.11111100.00000000 → 255.255.252.0`

- `1` → network bits  
- `0` → host bits  

💡 Trick:
- Network ID → all host bits = 0  
- Broadcast ID → all host bits = 1  

---

## 🔹 Original Class-Based Addressing

Before CIDR, IP addresses were divided into classes.

### IP Classes Overview

| Class | Range | Default Mask | Use |
|------|------|-------------|-----|
| A | 1.0.0.0 – 126.255.255.255 | /8 | Large networks |
| B | 128.0.0.0 – 191.255.255.255 | /16 | Medium networks |
| C | 192.0.0.0 – 223.255.255.255 | /24 | Small networks |
| D | 224.0.0.0 – 239.255.255.255 | Multicast | Special |
| E | 240.0.0.0 – 255.255.255.255 | Reserved | Experimental |

💡 Note: `127.x.x.x` is reserved for loopback  

💡 Limitation: Wasted many IP addresses → replaced by CIDR  

---

## 🔹 CIDR (Classless Inter-Domain Routing)

CIDR removes class limitation and allows flexible subnetting.

Format:
```
IP_Address/Prefix_Length
```

Example:  
`192.168.60.3/26`

- `/26 → 26 bits for network, 6 bits for host`  
- Binary mask:  
  `11111111.11111111.11111111.11000000`  

💡 More flexible and efficient  

---

### 🔹 Important Terms

- **Network ID** → First address of the network  
- **Broadcast ID** → Last address (used to send to all hosts)  
- **Address Range** → All addresses from network to broadcast  
- **Host Range** → Usable IPs for devices  

---

### Question

Find Network ID, Broadcast ID, Address Range, Host Range for: 192.168.192.0/19

---

### Solution

### Method 1: Binary (Conceptual)

- `/19` → Subnet mask: `11111111.11111111.11100000.00000000`  
- IP : 192.168.192.0 → 11000000.10101000.11000000.00000000

Thus, 1st Address: 11000000.10101000.11000000.00000000 → 192.168.192.0

And, Last Address: 11000000.10101000.11011111.11111111 → 192.168.223.255

---

### Method 2: Block Size

- `/19` → Subnet mask: `255.255.224.0`  
- Block size = **256 - 224 = 32**  

So subnet ranges increase by 32 in the 3rd Octet: 0, 32, 64, 96, 128, 160, 192, 224

Given network starts at 192:

192 → next block = 224

So, the Last Address: 192.168.223.255

---

### ✔ Final Answer

- **Network ID:** 192.168.192.0  
- **Broadcast ID:** 192.168.223.255  
- **Address Range:** 192.168.192.0 – 192.168.223.255  
- **Host Range:** 192.168.192.1 – 192.168.223.254  

---

## 🔹 Special IP Addresses

### Private IP Addresses

| Range | CIDR |
|------|------|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

---

### Public vs Private IP

| Type | Description |
|------|------------|
| Public IP | Globally unique, routable on internet |
| Private IP | Used inside local networks, not routable |

💡 Private IPs require **NAT** to access the internet  

---

### Loopback Address

Loopback refers to the system itself (**localhost**).

- Range: `127.0.0.0/8`  
- Common: `127.0.0.1`  
- Used for testing and internal communication  
- Packets never leave the device  

---

## 🔹 Static vs Dynamic IP

| Type | Description |
|------|------------|
| Static IP | Manually assigned, does not change |
| Dynamic IP | Assigned by DHCP, may change |

💡 Most home networks use dynamic IPs  

---

## 🔍 Viewing IP Address (Linux)

```bash
ip address
```

or, 

```bash
ip -br addr
```

---

## ⚙️ Assigning IP Address

### Manual

```bash
sudo ip addr add 192.168.60.6/24 dev enp0s3
```

---

### Automatic (DHCP)

**DHCP = Dynamic Host Configuration Protocol**

Automatically assigns:
- IP address  
- Subnet mask  
- Default gateway  

---

## 🌍 DNS (Short Idea)

DNS converts:

- Domain names → IP addresses  

Example:
- `www.google.com → 142.250.xxx.xxx`

---

## 🔎 Check IP of a Host

```bash
dig www.example.com
```

Output:
```
93.184.216.34
```

---

## 📌 Key Takeaways

- IP address uniquely identifies a device  
- IPv4 = 32-bit system  
- Network + Host division  
- CIDR = flexible addressing  
- Private IPs require NAT  
- DHCP automates configuration  
- DNS maps names to IPs  

---