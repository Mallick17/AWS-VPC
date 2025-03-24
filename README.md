# IP Addressing for VPCs and Subnets
## 1. Role of IP Addresses in Your VPC

- **Communication:**  
  - IP addresses allow the resources (like EC2 instances) within your VPC to talk to each other.
  - They also enable these resources to communicate with external systems over the internet.

- **Types of IP Addressing:**  
  - Both IPv4 and IPv6 are used, with each having its own representation and rules.

---

## 2. Understanding CIDR Notation

- **What is CIDR?**  
  - CIDR (Classless Inter-Domain Routing) notation represents an IP address along with its associated network mask.
  - It defines the range of IP addresses available within a block.

- **IPv4 in CIDR Notation:**  
  - **Individual IPv4 Address:**  
    - Composed of 32 bits divided into 4 groups (e.g., 10.0.1.0).
  - **IPv4 CIDR Block:**  
    - Written as four groups of numbers (0-255) separated by periods, followed by a slash and a number (0-32).  
    - **Example:** 10.0.0.0/16 indicates the network and how many bits are used for the network part.

- **IPv6 in CIDR Notation:**  
  - **Individual IPv6 Address:**  
    - Consists of 128 bits in 8 groups of 4 hexadecimal digits (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334).
  - **IPv6 CIDR Block:**  
    - Similar concept as IPv4 but uses hexadecimal and can include a double colon (::) for abbreviation.  
    - **Example:** 2001:db8:1234:1a00::/56

- **Reference:**  
  - For further details on CIDR, check the “What is CIDR?” documentation.

---

## 3. Private IPv4 Addresses

- **Definition:**  
  - Private IPv4 addresses are used within your VPC for internal communication; they are not reachable from the internet.
  
- **Key Points:**
  - When you launch an instance in a VPC, it gets a **primary private IP address** from the subnet’s IPv4 range.
  - An instance is also given an internal DNS hostname that resolves to its private IP address.
  - **Additional IPs:**  
    - You can assign secondary private IP addresses that can be moved between network interfaces.
  - **Usage:**  
    - These addresses are typically from the RFC 1918 ranges (reserved for private networks), but you can also use public ranges if needed (with proper connectivity set up).

---

## 4. Public IPv4 Addresses

- **How They Work:**
  - Public IP addresses are allocated from AWS’s pool.
  - When an instance is launched in a subnet with auto-assigned public IPs enabled, it receives a public IP mapped via Network Address Translation (NAT) to its private IP.

- **Control and Management:**
  - **Subnet Attribute:**  
    - You can control whether instances in a subnet automatically get a public IP address.
  - **Instance Launch Setting:**  
    - The setting during instance launch can override the subnet's default public IP assignment.
  - **Post-Launch Management:**  
    - Public IPs can be disassociated from an instance if needed.

- **Billing and Elastic IPs:**
  - AWS charges for public IPv4 addresses, including those associated with running instances and Elastic IP addresses.
  - If you need a persistent public IP, use an **Elastic IP address** which can be reassigned as required.

- **DNS and Access:**
  - If DNS hostnames are enabled in your VPC, a public DNS hostname will be provided that resolves to the public IP from outside the VPC.

---

## 5. IPv6 Addresses

- **Transition to IPv6:**  
  - IPv6 was introduced to overcome the limitations of IPv4, offering a much larger address space.

- **Public IPv6 Addresses:**
  - Can be configured either as private or accessible over the internet.
  - Preparation involves:
    - Using Amazon VPC IP Address Manager (IPAM) to provision a public IPv6 address range.
    - Assigning these addresses to instances or VPCs as needed.
  - **Key Process:**
    - Allocate an IPv6 CIDR block to your VPC and associate it with your subnets.

- **Private IPv6 Addresses:**
  - Not advertised or routed on the internet.
  - **Types:**  
    - **ULA (Unique Local Addresses):**  
      - Typically start with “fc” or “fd” (e.g., addresses in the fd00::/8 range).
    - **GUA (Global Unicast Addresses):**  
      - Can also be used as private addresses but require specific enablement.
  - **Usage Consideration:**  
    - Private IPv6 addresses are only available via IPAM.
    - AWS reserves some addresses (first 4 and last one) in each subnet’s IPv6 range.
  - **Restrictions:**
    - Traffic from a private IPv6 address cannot exit directly to the internet even if a gateway is attached.

---

## 6. Use Your Own IP Addresses

- **Bring Your Own IP (BYOIP):**
  - You have the option to bring your own public IPv4 or IPv6 address ranges to AWS.
  - **Ownership:**  
    - You retain ownership, but AWS advertises these addresses on your behalf.
  - **Usage:**  
    - Create Elastic IPs from your IPv4 pool.
    - Associate your IPv6 ranges with a VPC as needed.

---

## 7. Amazon VPC IP Address Manager (IPAM)

- **Purpose of IPAM:**
  - IPAM helps in planning, tracking, and monitoring your IP addresses across your AWS resources.
  - **Benefits:**
    - Simplifies the allocation of CIDR blocks to VPCs.
    - Reduces management overhead by organizing IP address allocation based on business rules.
  - **Usage:**
    - Use IPAM to provision both public and private IPv6 and IPv4 address pools.
    - It assists in managing overlaps and ensuring efficient use of address space.

---

## 8. Summary Comparison: IPv4 vs. IPv6

- **IPv4:**
  - Uses 32 bits.
  - Written in decimal with four groups.
  - Limited address space leading to the need for private IP ranges and NAT.
  
- **IPv6:**
  - Uses 128 bits.
  - Written in hexadecimal, supporting a vastly larger address space.
  - Designed to eventually replace IPv4 by alleviating address exhaustion issues.
  
- **Communication:**
  - Both can be used in VPCs, but they are managed differently, particularly when considering public versus private accessibility.

---
