# 🧪 DNS Rebinding Attack

---

## 📌 Definition

DNS Rebinding is an attack that **bypasses the Same Origin Policy (SOP)** by changing the IP address of a domain over time.

👉 The browser trusts the domain  
👉 But the IP behind that domain changes  

💥 Result: attacker gains access to internal/private services

---

## 🧠 Basic Idea

Normal behavior:

User → visits attacker website  
Browser → enforces Same Origin Policy  
👉 Cannot access internal network (e.g., 192.168.x.x)

---

Attack behavior:

1. User visits: attacker32.com  
2. DNS returns attacker’s server IP  
3. Browser loads malicious JavaScript  
4. DNS changes IP → points to internal device  
5. Same domain, different IP → SOP bypassed  

💥 Browser now talks to internal device!

---

## 🌐 Scenario

- User VM → 192.168.60.1  
- IoT Device → 192.168.60.80  
- Attacker Server → 10.9.0.180  

👉 IoT device is password protected  
👉 But inside local network  

---

## 🔒 Same Origin Policy (SOP)

SOP allows requests only if:

- Same protocol  
- Same domain  
- Same port  

👉 It does NOT check IP changes  

⚠️ That is the weakness exploited

---

## 💥 Attack Flow

1. User visits attacker website  
2. Attacker serves JavaScript  
3. JavaScript sends requests to attacker domain  
4. DNS changes mapping:

```
attacker32.com → 10.9.0.180   (initial)
attacker32.com → 192.168.60.80 (after rebinding)
```

5. Browser still trusts the domain  
6. Requests now go to IoT device  

---

## 🧾 Example HTTP Traffic

```
GET /password HTTP/1.1

POST /temperature?value=88&password=8xk...
```

👉 Sensitive data is exposed

---

## ⚙️ DNS Rebinding Setup (Initial Zone File)

```
$TTL 1

@   IN  SOA ns.attacker32.com. admin.attacker32.com. (
        2008111001
        8H
        2H
        4W
        1D )

@   IN  NS  ns.attacker32.com.

@   IN  A   10.9.0.180
www IN  A   10.9.0.180
ns  IN  A   10.9.0.153
*   IN  A   10.9.0.100
```

👉 TTL is very low → allows fast IP switching

---

## 🔄 DNS Rebinding Modification

```
@   IN  NS  ns.attacker32.com.

@   IN  A   10.9.0.180
www IN  A   192.168.60.80   ← switched to internal device
ns  IN  A   10.9.0.153
*   IN  A   10.9.0.100
```

👉 Now same domain points to internal IP

---

## 🔁 Apply Changes

```
# service named restart
```

👉 Restart DNS server to apply rebinding

---

## 🎯 Why This Works

| Component | Weakness |
|----------|---------|
| DNS TTL  | Allows fast IP change |
| Browser  | Trusts domain, not IP |
| SOP      | Does not verify IP change |

---

## 💥 Final Outcome

```
Before:
attacker32.com → attacker server

After:
attacker32.com → internal IoT device
```

👉 Browser sends requests to internal network  
👉 Attacker controls communication  

---

## 🚨 Impact

- Access internal services  
- Steal credentials  
- Control IoT devices  
- Bypass network isolation  

---

## 🧠 Final Intuition

👉 DNS controls **where** traffic goes  
👉 Browser trusts **who** (domain name)  
👉 Attacker manipulates DNS  

💥 Trust is broken → attack succeeds

---