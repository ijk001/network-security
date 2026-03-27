# 🧪 04-05: The Kaminsky Attack

## 📌 Definition

The Kaminsky Attack is a type of DNS cache poisoning attack that targets the **authority (NS) record** of a domain.

👉 Instead of poisoning a single hostname, it allows the attacker to:

- Inject a **malicious nameserver**
- Take control of the **entire domain**

---

## 🧠 Basic Idea

Normal DNS:

User → DNS → example.com DNS server → correct answer

After attack:

User → DNS → poisoned cache → attacker-controlled DNS → fake answers

💥 Entire domain resolution is now controlled by attacker

---

## ⚠️ Key Problem

DNS uses:

- UDP (no connection)
- Transaction ID (only 16-bit)

👉 This makes it possible to **guess or brute-force responses**

---

## 🧩 The Core Weakness

DNS validates responses using:

- Transaction ID
- Source port
- Matching question

👉 If attacker guesses these correctly → response is accepted

---

## 🔥 Main Strategy

Instead of attacking:

www.example.com

👉 The attacker queries:

random123.example.com  
random456.example.com  
randomXYZ.example.com  

These **do not exist**

---

## 💡 Why Random Subdomains?

Because:

- DNS server must ask authoritative server every time
- Cache cannot help (new name every time)

👉 This creates many opportunities to inject fake replies

---

## 🧪 Attack Steps

1. Attacker sends query for random subdomain  
   Example:
   random1.example.com  

2. DNS server forwards request to real DNS  

3. Attacker floods fake responses:

   - Different transaction IDs  
   - Same question  
   - Fake authority section  

4. If one matches → accepted  

5. Cache stores:

   example.com → ns.attacker32.com  

💥 Entire domain hijacked

---

## 📦 Key Injection

The attacker inserts:

example.com → ns.attacker32.com  

👉 Now all future queries go to attacker DNS

---

## 🧠 Simple Flow

Attacker → many fake replies  
DNS server → accepts one valid-looking reply  
Cache → poisoned  
Users → redirected  

---

## 🎯 Why This Is Powerful

| Feature | Impact |
|--------|-------|
| Random subdomains | Unlimited attempts |
| UDP protocol | No handshake |
| Small ID space | Easy guessing |
| Authority poisoning | Full domain takeover |

---

## 💥 Result of Attack

After success:

example.com → ns.attacker32.com  

Now:

- www.example.com → attacker  
- mail.example.com → attacker  
- login.example.com → attacker  

👉 Entire domain compromised

---

## 🛡️ Defenses

To prevent Kaminsky attack:

- Random source ports (adds entropy)
- Random transaction IDs
- DNSSEC (strongest protection)
- Rate limiting DNS queries

---

## ✅ Key Takeaway

Kaminsky attack works by:

- Forcing repeated DNS queries using random subdomains
- Flooding fake responses
- Winning the race once
- Poisoning the authority section

🔥 Once successful → attacker controls the entire domain