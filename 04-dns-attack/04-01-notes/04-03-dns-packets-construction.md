# 🌐 04-03: DNS Packets Construction

---

## 📌 Introduction

DNS packets are the core units of communication in the Domain Name System.  
They carry queries and responses between clients and DNS servers.

Understanding DNS packets helps in:
- Debugging DNS issues
- Analyzing network traffic
- Building custom DNS tools
- Understanding security attacks (e.g., spoofing)

---

## 🧱 DNS Packet Structure

A DNS packet follows a layered structure:

IP Header → UDP Header → DNS Header → DNS Data

- IP Header → handles routing  
- UDP Header → uses port 53  
- DNS Header → controls behavior  
- DNS Data → contains actual query/response  

---

## 🧩 DNS Header Fields

| Field | Purpose |
|------|--------|
| id | Unique transaction ID |
| flags | Query/response control |
| qdcount | Number of questions |
| ancount | Number of answers |
| nscount | Authority records |
| arcount | Additional records |

---

### ⚙️ Important Flags

- qr → 0 (query), 1 (response)  
- aa → authoritative answer  
- rd → recursion desired  
- ra → recursion available  

---

## 📦 DNS Sections

| Section | Description |
|--------|-------------|
| qd | Question |
| an | Answer |
| ns | Authority |
| ar | Additional |

---

## 📄 DNS Records (Concept)

Example:

Query: www.example.com  

Answer: www.example.com → 1.2.3.4  
Authority: example.com → ns.example.com  
Additional: ns.example.com → 10.0.0.1  

---

## 🛠️ DNS Structure in Scapy

Use the following command to inspect DNS structure:

ls(DNS)

Key components:
- qd → question
- an → answer
- ns → authority
- ar → additional

---

## 🧪 DNS Query Record (DNSQR)

Fields:
- qname → domain name
- qtype → record type (A = 1)
- qclass → class (IN = 1)

---

## 🧪 DNS Resource Record (DNSRR)

Fields:
- rrname → domain name
- type → record type
- rclass → class
- ttl → time to live
- rdata → actual data (IP / NS)

---

## 📡 Example: Sending a DNS Query

Python (Scapy):

from scapy.all import *

IPpkt  = IP(dst='8.8.8.8')
UDPpkt = UDP(dport=53)

Qdsec  = DNSQR(qname='www.example.com')

DNSpkt = DNS(
    id=100,
    qr=0,
    rd=1,
    qd=Qdsec
)

packet = IPpkt / UDPpkt / DNSpkt

response = sr1(packet, timeout=2)

if response:
    print(response[DNS].summary())

---

## 🖥️ Simple DNS Server (Concept)

### Step 1: Receive Query

from scapy.all import *
from socket import socket, AF_INET, SOCK_DGRAM

sock = socket(AF_INET, SOCK_DGRAM)
sock.bind(("0.0.0.0", 1053))

while True:
    data, addr = sock.recvfrom(4096)

    dns_req = DNS(data)
    query_name = dns_req.qd.qname.decode()

    print("Query:", query_name)

---

### Step 2: Build Response

    answer = DNSRR(
        rrname=dns_req.qd.qname,
        type="A",
        rdata="10.2.3.6",
        ttl=300
    )

---

### Step 3: Send Response

    dns_resp = DNS(
        id=dns_req.id,
        qr=1,
        aa=1,
        qd=dns_req.qd,

        qdcount=1,
        ancount=1,

        an=answer
    )

    sock.sendto(bytes(dns_resp), addr)

---

## 🔄 Communication Flow

Client → DNS Query → Server  
Server → DNS Response → Client  

---

## 🧠 Key Takeaways

- DNS uses layered design (IP → UDP → DNS)
- DNSQR is used for queries
- DNSRR is used for answers
- Header fields control packet behavior
- Scapy enables full packet manipulation

---

## 🚀 Why This Matters

- Network debugging
- Packet analysis (Wireshark)
- Security research
- Building custom DNS systems

---

## ✅ Final Flow

IP → UDP → DNS Header → Question → Answer → Authority → Additional

---