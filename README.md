# Classless Inter-Domain Routing
### **What is CIDR?**
CIDR is a method for allocating IP addresses and routing data on the internet. It replaced the older class-based system (Class A, B, C) to make IP address allocation more efficient and flexible. CIDR uses a **subnet mask** to define how an IP address is split between the **network portion** and the **host portion**.

- **Notation**: CIDR is written as an IP address followed by a slash and a number (e.g., `192.168.1.0/24`). The number after the slash (called the **prefix length**) indicates how many bits are used for the network portion.
- **Subnet Mask**: This is a 32-bit number (for IPv4) that masks the network part of an IP address. It’s often written in dotted-decimal form (e.g., `255.255.255.0`) or as a prefix length (e.g., `/24`).

---

### **Understanding Subnet Masks**
A subnet mask determines which part of an IP address identifies the network and which part identifies the host. It’s a binary concept:
- **1s** in the subnet mask represent the **network bits**.
- **0s** represent the **host bits**.

For example:
- IP: `192.168.1.10`
- Subnet Mask: `255.255.255.0` (or `/24`)
  - Binary: `11111111.11111111.11111111.00000000`
  - 24 bits are for the network (`192.168.1`), and 8 bits are for the host (`.10`).

---

### **CIDR Breakdown: Subnet Mask Table**
Here’s a detailed breakdown of common CIDR notations, their subnet masks, and what they mean in terms of network and host capacity. I’ll include the number of usable IP addresses (excluding the network and broadcast addresses).

| **CIDR** | **Subnet Mask**      | **Network Bits** | **Host Bits** | **Total IPs** | **Usable IPs** | **Wildcard Mask**   |
|----------|----------------------|------------------|---------------|---------------|----------------|---------------------|
| `/0`     | `0.0.0.0`           | 0                | 32            | 4,294,967,296 | 4,294,967,294  | `255.255.255.255`  |
| `/8`     | `255.0.0.0`         | 8                | 24            | 16,777,216    | 16,777,214     | `0.255.255.255`    |
| `/16`    | `255.255.0.0`       | 16               | 20            | 65,536        | 65,534         | `0.0.255.255`      |
| `/24`    | `255.255.255.0`     | 24               | 8             | 256           | 254            | `0.0.0.255`        |
| `/25`    | `255.255.255.128`   | 25               | 7             | 128           | 126            | `0.0.0.127`        |
| `/26`    | `255.255.255.192`   | 26               | 6             | 64            | 62             | `0.0.0.63`         |
| `/27`    | `255.255.255.224`   | 27               | 5             | 32            | 30             | `0.0.0.31`         |
| `/28`    | `255.255.255.240`   | 28               | 4             | 16            | 14             | `0.0.0.15`         |
| `/29`    | `255.255.255.248`   | 29               | 3             | 8             | 6              | `0.0.0.7`          |
| `/30`    | `255.255.255.252`   | 30               | 2             | 4             | 2              | `0.0.0.3`          |
| `/31`    | `255.255.255.254`   | 31               | 1             | 2             | 0 (point-to-point)| `0.0.0.1`       |
| `/32`    | `255.255.255.255`   | 32               | 0             | 1             | 1 (single host)| `0.0.0.0`          |

---

### **Key Concepts in the Table**
1. **Total IPs**: Calculated as `2^(host bits)`. This includes all possible addresses in the subnet.
2. **Usable IPs**: Total IPs minus 2 (the **network address** and **broadcast address**), except:
   - `/31`: Used for point-to-point links, no broadcast/network addresses (RFC 3021).
   - `/32`: A single host, no subnetting.
3. **Wildcard Mask**: The inverse of the subnet mask (e.g., `255.255.255.0` → `0.0.0.255`). Used in access control lists (ACLs) to specify ranges.

---

### **How Subnetting Works**
Subnetting divides a larger network into smaller subnetworks. Each subnet has its own range of IP addresses, defined by the subnet mask.

#### **Example: Subnetting `192.168.1.0/24`**
- **Original Network**: 
  - CIDR: `/24`
  - Subnet Mask: `255.255.255.0`
  - Range: `192.168.1.0 - 192.168.1.255`
  - Usable IPs: 254

- **Subnet to `/26`**:
  - New Subnet Mask: `255.255.255.192`
  - Host Bits: 6 → `2^6 = 64 IPs per subnet`
  - Usable IPs: 62 per subnet
  - Number of Subnets: `2^(26-24) = 2^2 = 4 subnets`
  - Subnet Ranges:
    1. `192.168.1.0 - 192.168.1.63` (Network: `.0`, Broadcast: `.63`)
    2. `192.168.1.64 - 192.168.1.127` (Network: `.64`, Broadcast: `.127`)
    3. `192.168.1.128 - 192.168.1.191` (Network: `.128`, Broadcast: `.191`)
    4. `192.168.1.192 - 192.168.1.255` (Network: `.192`, Broadcast: `.255`)

Each subnet can support 62 devices, and you’ve created 4 separate networks from the original `/24`.

---

### **Step-by-Step: Calculating Subnets**
Let’s subnet `10.0.0.0/8` into `/10`:
1. **Original**:
   - `/8` → `255.0.0.0`
   - 24 host bits → `2^24 = 16,777,216 IPs`
   - Usable: 16,777,214
2. **New Mask**:
   - `/10` → `255.192.0.0` (Binary: `11111111.11000000.00000000.00000000`)
   - 22 host bits → `2^22 = 4,194,304 IPs per subnet`
   - Usable: 4,194,302
3. **Number of Subnets**:
   - Borrowed bits: `10 - 8 = 2`
   - `2^2 = 4 subnets`
4. **Ranges**:
   - `10.0.0.0 - 10.63.255.255`
   - `10.64.0.0 - 10.127.255.255`
   - `10.128.0.0 - 10.191.255.255`
   - `10.192.0.0 - 10.255.255.255`

---

### **CIDR and IPv6**
CIDR applies to IPv6 too, but the 128-bit address space changes the math:
- Example: `2001:db8::/32`
  - 32 bits for the network, 96 bits for hosts.
  - Subnetting to `/48` creates `2^(48-32) = 65,536 subnets`, each with `2^80` addresses.

IPv6 subnetting is less common at the consumer level due to the vast address space, but it follows the same principles.

---

### **Practical Use Cases**
- **Home Networks**: `/24` (e.g., `192.168.1.0/24`) is standard, supporting 254 devices.
- **Small Offices**: `/26` or `/27` for smaller subnets with 62 or 30 devices.
- **ISPs**: Larger blocks like `/16` or `/8` for customer allocations.

---

### **Quick Tips**
- **Block Size**: Each step in CIDR (e.g., `/24` to `/25`) halves the number of IPs and doubles the number of subnets.
- **Binary Trick**: To find the subnet increment, look at the rightmost `1` in the subnet mask (e.g., `255.255.255.192` → increment of 64).

