# 08-02: Exercises

## Question

A Class B network with network ID **172.20.0.0** is given.  
The network needs to be divided into **at least 15 subnets**.

Based on this requirement, answer the following:

1. How many bits have to be borrowed from the host portion?  
2. How many total bits will be used to identify the subnet?  
3. What is the new subnet mask in dotted decimal notation?  
4. What is the magic number (block size)?  
5. What are the network IDs of:
   - the first subnet  
   - the second subnet  
   - the last required (15th) subnet  
   - the last created (16th) subnet  
6. How many usable host addresses are available per subnet?  
7. What are the usable host ranges of:
   - the first subnet  
   - the second subnet  
   - the last required (15th) subnet  

---

## Solution

### Step 1: Number of bits to be borrowed

Given:
- Class B network → default prefix = /16  
- Required subnets = 15  

Checking powers of 2:

- 2¹ = 2  
- 2² = 4  
- 2³ = 8  
- 2⁴ = 16  

Thus, 4 bits have to be borrowed (since 2⁴ = 16 ≥ 15).

👉 **(1) Bits borrowed = 4**

---

### Step 2: Total subnet bits

👉 **(2) Total subnet bits = 16 + 4 = 20 bits (/20)**  

---

### Step 3: New subnet mask

Default mask (binary):  
11111111.11111111.00000000.00000000  

After borrowing 4 bits:  
11111111.11111111.11110000.00000000  

Convert to decimal:

👉 **(3) Subnet mask = 255.255.240.0 (/20)**  

---

### Step 4: Magic number (Block Size)

The change occurs in the **3rd octet**.

Magic number = 256 − 240 = **16**

👉 **(4) Magic number = 16**

(Subnet IDs increase by 16 in the 3rd octet)

---

### Step 5: Subnet network IDs

Using block size 16:

- 1st → 172.20.0.0/20  
- 2nd → 172.20.16.0/20  
- 3rd → 172.20.32.0/20  
- ...  
- 15th → 172.20.224.0/20  
- 16th → 172.20.240.0/20  

👉 **(5) Network IDs:**
- First subnet → 172.20.0.0  
- Second subnet → 172.20.16.0  
- Last required (15th) → 172.20.224.0  
- Last created (16th) → 172.20.240.0  

---

### Step 6: Number of hosts per subnet

Remaining host bits: 32 − 20 = 12  

Number of usable hosts: 2¹² − 2 = 4096 − 2 = **4094**

👉 **(6) Usable hosts per subnet = 4094**

---

### Step 7: Host ranges

#### First subnet (172.20.0.0/20)

- Network ID = 172.20.0.0  
- Broadcast ID = 172.20.15.255  
- Usable host range = 172.20.0.1 to 172.20.15.254  

---

#### Second subnet (172.20.16.0/20)

- Network ID = 172.20.16.0  
- Broadcast ID = 172.20.31.255  
- Usable host range = 172.20.16.1 to 172.20.31.254  

---

#### Last required (15th) subnet (172.20.224.0/20)

- Network ID = 172.20.224.0  
- Broadcast ID = 172.20.239.255  
- Usable host range = 172.20.224.1 to 172.20.239.254  

---

👉 **(7) Host ranges:**
- First subnet → 172.20.0.1 to 172.20.15.254  
- Second subnet → 172.20.16.1 to 172.20.31.254  
- Last required subnet → 172.20.224.1 to 172.20.239.254  

---

## Final Summary

1. Bits borrowed from host portion = 4  

2. Total subnet bits = 20 bits (/20)  

3. Subnet mask = 255.255.240.0  

4. Magic number (block size) = 16  

5. Subnet network IDs:
   - First subnet → 172.20.0.0  
   - Second subnet → 172.20.16.0  
   - Last required (15th) → 172.20.224.0  
   - Last created (16th) → 172.20.240.0  

6. Usable hosts per subnet = 4094  

7. Host ranges:
   - First subnet → 172.20.0.1 to 172.20.15.254  
   - Second subnet → 172.20.16.1 to 172.20.31.254  
   - Last required subnet → 172.20.224.1 to 172.20.239.254   