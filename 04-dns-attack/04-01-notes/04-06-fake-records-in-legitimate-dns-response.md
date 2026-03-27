## 🎭 04-06: Fake Records in Legitimate DNS Response

### 📌 Definition

A **Fake Record in a Legitimate Response** is when an attacker inserts malicious DNS records inside an otherwise valid-looking DNS reply.

👉 The packet appears completely normal, but contains hidden malicious data.

---

### 🧠 Key Idea

A DNS response has multiple sections:

- Question → what was asked  
- Answer → direct IP response  
- Authority → who controls the domain  
- Additional → extra helpful info  

💥 Attackers inject fake data into **Authority** or **Additional** sections.

---

### 🧾 Example: Legitimate DNS Response

```
;; QUESTION SECTION:
;www.example.com.        IN  A

;; ANSWER SECTION:
www.example.com.  86370  IN  A  93.184.216.34

;; AUTHORITY SECTION:
example.com.      172769 IN  NS b.iana-servers.net.
example.com.      172769 IN  NS a.iana-servers.net.

;; ADDITIONAL SECTION:
a.iana-servers.net. 1770 IN A 199.43.135.53
b.iana-servers.net. 1770 IN A 199.43.133.53
```

👉 This is a **normal DNS reply**

---

### ⚠️ Malicious Version (Poisoned Response)

```
;; QUESTION SECTION:
;www.example.com.        IN  A

;; ANSWER SECTION:
www.example.com.  259200  IN  A  1.2.3.4   ← fake IP

;; AUTHORITY SECTION:
example.com.      259200  IN  NS ns.attacker32.com  ← injected!

;; ADDITIONAL SECTION:
ns.attacker32.com. 259200 IN A 10.9.0.153
```

---

### 🔥 What Changed?

| Section      | Change |
|-------------|--------|
| Answer      | Fake IP (redirect user) |
| Authority   | Attacker controls domain |
| Additional  | IP of attacker DNS |

---

### 🎯 Why This Works

- DNS accepts **extra records** in response  
- Resolver caches **Authority + Additional data**  
- Packet looks legitimate → not rejected  

💥 Result:

```
example.com → ns.attacker32.com
```

👉 Now attacker controls ALL subdomains

---

### 🔗 Connection to Kaminsky Attack

- Kaminsky attack sends many spoofed responses  
- Each response includes:
  - Correct-looking answer  
  - Malicious authority record  

- If one response matches:
  - DNS cache stores fake NS record  
  - Entire domain gets hijacked  

---

### 💡 Final Intuition

| Component   | Role |
|------------|------|
| Answer     | Redirect victim |
| Authority  | Take domain control |
| Additional | Help resolver trust attacker |
| UDP        | No verification → easy spoof |

---

### 🚀 Final Outcome

```
Before attack:
example.com → real DNS servers

After attack:
example.com → ns.attacker32.com
```

👉 Full domain hijack achieved

---