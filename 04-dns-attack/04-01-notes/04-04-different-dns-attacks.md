# 🌐 04-04: Different Types of DNS Attacks

## 🟢 1. Local DNS Cache Poisoning

### 📌 Definition
Local DNS Cache Poisoning is an attack where **false DNS records are inserted into a local DNS server’s cache**, causing users to be redirected to malicious websites.

### 🧠 Basic Attack Process
1. User sends a DNS query to a local DNS server  
2. Attacker sends a fake DNS response before the real one arrives  
3. DNS server accepts and stores the fake IP (cache poisoning)  
4. Future users requesting the same domain are redirected to attacker’s IP  

💥 Result: Users unknowingly visit malicious websites

---

## 🔵 2. The Kaminsky Attack

### 📌 Definition
The Kaminsky Attack is a **large-scale DNS cache poisoning attack** that targets DNS servers by guessing transaction IDs and injecting fake responses.

### 🧠 Basic Attack Process
1. Attacker sends many DNS queries for random subdomains (e.g., abc.example.com)  
2. DNS server asks authoritative server for answers  
3. Attacker floods fake responses with different transaction IDs  
4. If one matches → DNS server accepts fake response  
5. Fake authority (NS record) is inserted into cache  

💥 Result: Attacker gains control over the entire domain resolution

---

## 🟡 3. Fake Records in Legitimate DNS Response

### 📌 Definition
This attack involves **inserting malicious DNS records into an otherwise legitimate-looking DNS response**, tricking the DNS server into caching them.

### 🧠 Basic Attack Process
1. DNS server sends query to a legitimate server  
2. Attacker injects a response that looks valid  
3. Response contains:
   - Correct answer section  
   - Malicious authority/additional records  
4. DNS server trusts and caches all records  

💥 Result: Hidden malicious entries take control of future DNS queries

---

## 🟣 4. DNS Rebinding Attack

### 📌 Definition
DNS Rebinding is an attack that **bypasses browser security (Same Origin Policy)** by changing the IP address of a domain during communication.

### 🧠 Basic Attack Process
1. User visits attacker-controlled website  
2. Domain initially resolves to attacker’s server  
3. After a short time (TTL expires), DNS response changes  
4. Same domain now resolves to internal/local IP (e.g., 192.168.x.x)  
5. Browser trusts it (same domain) and sends requests  

💥 Result: Attacker can access internal network services

---

## 🔴 5. Denial-of-Service (DoS) Attacks on DNS

### 📌 Definition
A DoS attack on DNS is when attackers **overwhelm DNS servers with massive traffic**, making them unavailable to legitimate users.

### 🧠 Basic Attack Process
1. Attacker sends huge numbers of DNS requests  
2. DNS server resources become overloaded  
3. Server cannot respond to real users  
4. Legitimate requests fail or timeout  

💥 Result: Websites become unreachable

---

## 🎯 Final Summary

| Attack Type | Main Goal |
|------------|----------|
| Cache Poisoning | Redirect users |
| Kaminsky Attack | Take control of domain |
| Fake Records | Inject hidden malicious entries |
| DNS Rebinding | Access internal systems |
| DoS Attack | Disrupt DNS service |

---