# VPC Peering Connection

A **VPC Peering Connection** is a **networking connection between two VPCs** that enables communication using **private IPv4 or IPv6 addresses**. 

- It allows resources in either VPC (such as EC2 or Lambda) to **communicate as if they were in the same network**.
- You can peer:
  - VPCs in **the same AWS account**
  - VPCs in **different AWS accounts**
  - VPCs in **different AWS Regions** (called **inter-Region VPC peering**)

![image](https://github.com/user-attachments/assets/a5c02881-1506-41af-8045-d8efb74f1f5e)

---

### **How VPC Peering Works**

- **No gateway, VPN, or physical hardware is required.**
- **AWS uses the existing VPC infrastructure**, ensuring:
  - High availability
  - No single point of failure
  - No bandwidth bottlenecks
- The traffic remains **within the AWS global network backbone**, avoiding the public internet entirely.

---

### **Key Benefits of VPC Peering**

| Benefit | Description |
|--------|-------------|
| **Private Communication** | Resources in peered VPCs can communicate privately using IP addresses. |
| **High Availability** | No single point of failure; traffic uses AWS’s resilient infrastructure. |
| **Secure & Encrypted** | Inter-Region traffic is encrypted and remains within AWS’s private network. |
| **Cost-Effective** | No need for costly VPNs or third-party appliances. |
| **Scalable File Sharing** | Easily share data across accounts or business units. |
| **Geographic Redundancy** | Replicate data between Regions for disaster recovery. |

---

### **Inter-Region VPC Peering**

- Allows VPCs in **different AWS Regions** to communicate using **private IP addresses**.
- Traffic is:
  - **Encrypted**
  - **Private** (does not traverse the public internet)
  - **Resilient** (built on AWS backbone)

This is useful for:
- Cross-region applications
- Global file sharing
- Multi-region architecture
- Data replication for backup and disaster recovery

---

### **Use Case Examples**

-  **File Sharing Network:** Connect multiple VPCs across accounts to share files securely.
-  **Cross-Account Access:** Provide another account access to your services (e.g., EC2, RDS).
-  **Multi-Region Redundancy:** Replicate critical data and applications across regions.

---

## Pricing for a VPC peering connection
- There is no charge to create a VPC peering connection. All data transfer over a VPC peering connection that stays within an Availability Zone is free, even if it's between different accounts. Charges apply for data transfer over VPC peering connections that cross Availability Zones and Regions.
