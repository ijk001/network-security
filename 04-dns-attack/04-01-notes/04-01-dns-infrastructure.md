# DNS Infrastructure

---

## What is DNS?

**DNS (Domain Name System)** is a distributed system that translates:

- **Domain Names → IP Addresses**

### Example

- Human-friendly: `www.google.com`
- Machine-friendly: `142.250.xxx.xxx`

💡 Without DNS, users would need to remember IP addresses instead of domain names.

---

## DNS Infrastructure Overview

DNS uses a **hierarchical and distributed architecture**.

### Main Components:

1. **DNS Resolver**
2. **Root Name Server**
3. **Top-Level Domain (TLD) Server**
4. **Authoritative Name Server**

---

## DNS Domain Hierarchy

DNS namespace is organized as a **tree-like structure**.

- The top is the **ROOT**, denoted as `.`
- Each node is a **domain or subdomain**

### Hierarchy Example

```
.
├── (Root)
│   ├── .com
│   ├── .net
│   ├── .edu
│   ├── .gov
│   └── .fr
│        └── example.com
│             └── www.example.com
```

### Explanation

- **TLD:** `.com`  
- **Second-Level Domain:** `example`  
- **Subdomain:** `www`  

---

## 🧾 DNS Root Servers

- There are **13 logical root servers** (`a` to `m`)
- Example:
  - `a.root-servers.net`
  - `b.root-servers.net`
  - ...
  - `m.root-servers.net`

💡 Managed by:
- ICANN  
- Verisign  
- NASA  
- US DoD  

👉 Root servers **do NOT store full data**, they only **redirect to TLD servers**

---

## 🌐 Top-Level Domains (TLD)

### Types of TLDs:

- **Infrastructure TLD**
  - `.arpa`

- **Generic TLD (gTLD)**
  - `.com`, `.net`, `.org`

- **Sponsored TLD (sTLD)**
  - `.edu`, `.gov`, `.mil`, `.travel`, `.jobs`

- **Country Code TLD (ccTLD)**
  - `.au`, `.cn`, `.fr`

- **Reserved TLD**
  - `.example`, `.test`, `.localhost`, `.invalid`

---

## Subdomain

A **subdomain** is a subdivision of a main domain.

### Example

- Domain: `google.com`
- Subdomains:
  - `www.google.com`
  - `mail.google.com`
  - `maps.google.com`

💡 Subdomains are used to:
- Organize services
- Separate applications
- Structure large systems

---

## DNS Zones

A **DNS Zone** is a portion of the domain namespace managed by a DNS server.

### Example

- `example.com` → Main zone  
- `usa.example.com` → Delegated zone  
- `uk.example.com` → Delegated zone  

💡 Zones enable:
- Delegation  
- Distributed management  
- Scalability  

---

## ⚖️ Subdomain vs DNS Zone

| Feature        | Subdomain                          | DNS Zone                              |
|---------------|-----------------------------------|--------------------------------------|
| Definition    | Logical subdivision of a domain    | Administrative portion of DNS        |
| Purpose       | Organization                      | Management & delegation              |
| Example       | `blog.example.com`                | Zone for `example.com` or `usa.example.com` |
| Dependency    | Always part of a domain           | May be managed separately            |

💡 Key Idea:
- **Every DNS zone is a domain**
- **A subdomain becomes a separate zone only when delegated**

---

## DNS Server Types

### 1️⃣ Recursive Resolver
- First contact point (client → resolver)
- Performs full lookup process

---

### 2️⃣ Root Server
- Directs queries to TLD servers

---

### 3️⃣ TLD Server
- Directs queries to authoritative servers

---

### 4️⃣ Authoritative Server
- Stores DNS records
- Provides final IP address

---

## 🌍 Public DNS Servers

- Google DNS → `8.8.8.8`
- Cloudflare → `1.1.1.1`
- OpenDNS → `208.67.222.222`

---

## 📌 Key Takeaways

- DNS is a **hierarchical distributed system**  
- Root → TLD → Authoritative structure  
- Domains follow a **tree hierarchy**  
- Subdomains organize services logically  
- DNS Zones enable **delegation & scalability**  
- A subdomain becomes a zone only when delegated  
- Root servers guide queries globally  