# 🌐 04-03: DNS Packets Construction

---

## 📌 Introduction

DNS packets are the core units of communication in the Domain Name System.  
They carry queries (requests) and responses (answers) between clients and DNS servers.

---

## 🧱 DNS Packet Structure

A DNS packet follows a layered structure:

IP Header → UDP Header → DNS Header → DNS Data

- IP Header → handles routing  
- UDP Header → uses port 53  
- DNS Header → controls behavior  
- DNS Data → contains actual query/response  

![DNS Structure](image-2.png)

---

## 🧩 DNS Header Fields

| Field | Purpose |
|------|--------|
| id | Unique transaction ID (matches request & response) |
| flags | Query/response control information |
| qdcount | Number of Question Records |
| ancount | Number of Answer Records |
| nscount | Number of Authority Records |
| arcount | Number of Additional Records |

---

### ⚙️ Important Flags

- qr → 0 = query, 1 = response  
- aa → authoritative answer  
- rd → recursion desired  
- ra → recursion available  

---

## 📦 DNS Data Field

| Section | Description |
|--------|-------------|
| qd | Question (what we are asking) |
| an | Answer (final result, e.g., IP address) |
| ns | Authority (which server is responsible) |
| ar | Additional (extra info like IP of nameserver) |

---

## 📄 DNS Records (Concept — Clarified)

Query:
www.example.com

DNS Response (concept):

- Question (qd) → What is IP of www.example.com?
- Answer (an) → www.example.com → 1.2.3.4
- Authority (ns) → example.com → ns.example.com
- Additional (ar) → ns.example.com → 10.0.0.1

Meaning:
- Answer = final result  
- Authority = who knows  
- Additional = helpful shortcut  

---

## 🛠️ DNS Structure in Scapy

Command:
ls(DNS)

Key components:
- qd → question section  
- an → answer section  
- ns → authority section  
- ar → additional section  

---

## 🧪 DNS Query Record (DNSQR)

DNSQR represents the **question part of a DNS packet**.

Fields:
- qname → domain name  
- qtype → record type (A = IPv4)  
- qclass → class (IN = Internet)  

---

## 🧪 DNS Resource Record (DNSRR)

DNSRR represents **actual data in responses**.

Used in:
- Answer section  
- Authority section  
- Additional section  

Fields:
- rrname → domain name  
- type → record type  
- rclass → class  
- ttl → cache time  
- rdata → actual data (IP or nameserver)  

---

## 📡 Example: Sending a DNS Query

from scapy.all import *

IPpkt  = IP(dst='8.8.8.8')        # Google DNS

UDPpkt = UDP(dport=53)            # DNS port

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

Explanation:
- IP → destination server  
- UDP → port 53  
- DNSQR → question  
- DNS → full packet  
- "/" → stack layers  
- sr1() → send + receive  

---

## 🖥️ Simple DNS Server

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

Explanation:
- Creates UDP server  
- Receives DNS packet  
- Extracts domain name  

---

### Step 2: Build Response

answer = DNSRR(
    rrname=dns_req.qd.qname,
    type="A",
    rdata="10.2.3.6",
    ttl=300
)

Explanation:
- Creates answer record  
- Maps domain → IP  

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

Explanation:
- id must match request  
- qr=1 → response  
- aa=1 → authoritative  
- an → attach answer  

---

## 🔄 Communication Flow

Client → DNS Query → Server  
Server → DNS Response → Client  

---

## 🧠 Key Takeaways

- DNS uses layered design (IP → UDP → DNS)
- DNSQR = question
- DNSRR = actual data
- Header controls behavior
- Scapy allows manual packet creation  

---

## 🚀 Why This Matters

- Network debugging  
- Packet analysis (Wireshark)  
- Security research  
- Building DNS systems  

---

## ✅ Final Flow

IP → UDP → DNS Header → Question → Answer → Authority → Additional

---