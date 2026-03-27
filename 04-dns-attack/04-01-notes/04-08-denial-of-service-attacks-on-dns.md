# 💥 04-08: Denial-of-Service (DoS) Attacks on DNS

## 📌 Definition

A Denial-of-Service (DoS) attack on DNS is an attack that **overloads DNS servers with massive traffic**, making them unable to respond to legitimate user requests.

👉 Users cannot resolve domain names  
👉 Websites become unreachable  

💥 Internet services appear "down"

---

## 🧠 Basic Idea

Normal DNS flow:

User → DNS query → DNS server → IP → Website loads ✅

---

During DoS attack:

Attacker → floods DNS server with requests 🚀  
DNS server → overloaded ❌  
Legitimate users → no response  

💥 Result: service disruption

---

## ⚙️ How Attack Works

1. Attacker sends a huge number of DNS queries  
2. DNS server resources get exhausted (CPU, memory, bandwidth)  
3. Server cannot process real user requests  
4. Requests time out  

---

## 🔥 Types of DNS DoS Attacks

### 1. Direct Flood Attack
```
Attacker → DNS Server (millions of requests)
```
👉 Simple overload

---

### 2. Distributed DoS (DDoS)
```
Botnet (thousands of machines) → DNS Server
```
👉 Hard to block (many sources)

---

### 3. Amplification Attack (Most Powerful)

```
Attacker → Spoofed request → Open DNS resolver
Resolver → huge response → Victim
```

👉 Small request → large response  
👉 Victim gets flooded  

💥 Example: DNS amplification using UDP

---

## 🚨 Why DNS is Vulnerable

- Uses UDP (no connection setup)  
- No strong authentication  
- Easy to spoof source IP  
- Open resolvers amplify traffic  

---

## 📊 Real-World Case Studies

### 🟥 Root DNS Servers
- Multiple DoS attacks targeted root servers  
- Could disrupt global DNS resolution  

---

### 🟥 August 25, 2013 (.cn Attack)
- Attack on China’s top-level domain (.cn)  
- DNS servers went down for **2–4 hours**  
- Large-scale internet disruption  

---

### 🟥 December 24, 2009 (UltraDNS)
- DoS attack on UltraDNS provider  
- Affected **thousands of online shoppers**  
- Major e-commerce disruption  

---

### 🟥 October 21, 2016 (Dyn Attack)

- Massive DDoS attack on Dyn DNS provider  
- Used IoT botnet (Mirai malware)

👉 Affected:
- Amazon  
- Twitter  
- GitHub  
- CNN  
- Airbnb  

💥 Large parts of the internet went down

---

## 🎯 Impact

- Websites become unreachable  
- Business losses 💸  
- Service downtime  
- Global internet instability  

---

## 🛡️ Defense Mechanisms

- Rate limiting (limit requests per second)  
- Anycast DNS (distribute load globally)  
- DNS caching  
- Traffic filtering / firewalls  
- DDoS protection services (Cloudflare, Akamai)  

---

## 🧠 Final Intuition

👉 DNS = "Internet phonebook"  
👉 If DNS is down → everything breaks  

💥 Attack DNS → break the internet experience

---