# 08-01: Exercises

## Question

Find the **address range**, **usable host range**, **network ID**, and **broadcast ID** of the following subnet:

172.21.40.0/22  

---

## Solution

## Method 1: Binary Method

### Step 1: Write the Given IP Address in Binary

Subnet IP:  
172.21.40.0 → 10101100.00010101.00101000.00000000

Subnet Mask:  
/22 → first 22 bits are network bits, last 10 bits are host bits

---

### Step 2: Determine the First and Last Address

- First address: keep all host bits as `0`
- Last address: keep all host bits as `1`

So,

- First address:  
  10101100.00010101.00101000.00000000 → 172.21.40.0

- Last address:  
  10101100.00010101.00101011.11111111 → 172.21.43.255

---

### Step 3: Identify Network ID and Broadcast ID

- Network ID = first address = 172.21.40.0
- Broadcast ID = last address = 172.21.43.255

---

### Step 4: Determine the Address Range

Address range:  
172.21.40.0 to 172.21.43.255

---

### Step 5: Determine the Usable Host Range

- First usable host: 172.21.40.1
- Last usable host: 172.21.43.254

Usable host range:  
172.21.40.1 to 172.21.43.254

---

## Method 2: Block Size Method

### Step 1: Find the Subnet Mask and Block Size

Subnet Mask:  
/22 → 11111111.11111111.11111100.00000000 → 255.255.252.0

Block size = 256 − 252 = 4  
The block size applies in the **3rd octet**.

---

### Step 2: Find the Subnet Range

Subnets increase by 4 in the 3rd octet:

..., 36, 40, 44, ...

Given subnet:  
172.21.40.0

So:
- This subnet starts at 172.21.40.0
- The next subnet starts at 172.21.44.0

Therefore, the current subnet ends at:  
172.21.43.255

---

### Step 3: Identify Network ID and Broadcast ID

- Network ID = 172.21.40.0
- Broadcast ID = 172.21.43.255

---

### Step 4: Determine the Address Range

Address range:  
172.21.40.0 to 172.21.43.255

---

### Step 5: Determine the Usable Host Range

- First usable host: 172.21.40.1
- Last usable host: 172.21.43.254

Usable host range:  
172.21.40.1 to 172.21.43.254

---

## Final Answer

- Network ID: 172.21.40.0
- Broadcast ID: 172.21.43.255
- Address range: 172.21.40.0 to 172.21.43.255
- Usable host range: 172.21.40.1 to 172.21.43.254