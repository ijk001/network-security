# 08-03: Exercises

## Question

An IPv4 host has the following configuration:

- IP address: **192.168.89.130**  
- Subnet mask: **255.255.255.224 (/27)**  

Based on this information, answer the following:

1. What is the magic number (block size)?  
2. What is the network ID of the first subnet?  
3. What are the network IDs of:
   - the second subnet  
   - the last subnet  
4. What is the network ID of the subnet to which the given host belongs?  
5. What is the broadcast address of that subnet?  
6. What is the usable host range of that subnet?  

---

## Solution

### Step 1: Magic number (Block Size)

Subnet mask = 255.255.255.224  

Magic number = 256 − 224 = **32**

👉 **(1) Magic number = 32**

---

### Step 2: Network ID of the first subnet

This is a Class C network:

- Default network portion = first 3 octets → 192.168.89  

Thus, first subnet starts from: 192.168.89.0

👉 **(2) First subnet network ID = 192.168.89.0**

---

### Step 3: Network IDs of subnets

Using block size 32, subnet IDs increase by 32:

- 1st → 192.168.89.0  
- 2nd → 192.168.89.32  
- 3rd → 192.168.89.64  
- 4th → 192.168.89.96  
- 5th → 192.168.89.128  
- 6th → 192.168.89.160  
- 7th → 192.168.89.192  
- 8th → 192.168.89.224  

👉 **(3) Network IDs:**
- Second subnet → 192.168.89.32  
- Last subnet → 192.168.89.224  

---

### Step 4: Network ID of the host’s subnet

Host IP = 192.168.89.130  

Since block size = 32,
subnet boundaries occur at: 0, 32, 64, 96, 128, 160, ...

The largest value ≤ 130 is **128**.

Thus:

👉 **(4) Network ID = 192.168.89.128**

---

### Step 5: Broadcast address of the host’s subnet

The host belongs to the subnet that starts at **192.168.89.128**.

Since block size = 32,

- Next subnet starts from = 192.168.89.128 + 32 = **192.168.89.160**  

So, the last address of the current subnet = 192.168.89.160 − 1 = **192.168.89.159**

The last address of this subnet is the broadcast address.

👉 **(5) Broadcast address = 192.168.89.159**

---

### Step 6: Host range of the subnet

- Network ID = 192.168.89.128  
- Broadcast ID = 192.168.89.159  

Usable host range: 192.168.89.129 to 192.168.89.158

👉 **(6) Host range = 192.168.89.129 to 192.168.89.158**

---

## Final Summary

1. Magic number = 32  

2. First subnet network ID = 192.168.89.0  

3. Network IDs:
   - Second subnet → 192.168.89.32  
   - Last subnet → 192.168.89.224  

4. Host’s subnet network ID = 192.168.89.128  

5. Host’s Broadcast address = 192.168.89.159  

6. Host range = 192.168.89.129 to 192.168.89.158  