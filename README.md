# Virtual Private Cloud (VPC)
### **Amazon Virtual Private Cloud (Amazon VPC)**
**Amazon Virtual Private Cloud (Amazon VPC)** enables you to launch AWS resources into a **logically isolated virtual network** that you define. This network is similar to a traditional on-premises data center but leverages the scalability and flexibility of AWS infrastructure.

Think of it as your **own private section of the AWS Cloud**, where you control the network configuration, including IP ranges, subnets, route tables, gateways, and more.

An **Amazon Virtual Private Cloud (VPC)** is a **virtual network dedicated to your AWS account**, providing **logical isolation** from other virtual networks in the AWS Cloud. Within a VPC, you can **launch AWS resources** such as **Amazon EC2 instances**, databases, and more, in a secure and customizable network environment.

---

## Example Architecture Diagram

A typical VPC setup may include:
- Multiple subnets (across multiple **Availability Zones** for high availability).
- EC2 instances launched in each subnet.
- An **Internet Gateway** for public connectivity.

```
+----------------------------+
|        Amazon VPC         |
|  +---------+   +---------+|
|  | Subnet A|   | Subnet B||
|  |  (AZ-1) |   |  (AZ-2) ||   --> Connected via Internet Gateway
|  | EC2     |   | EC2     ||        for internet access.
|  +---------+   +---------+|
+----------------------------+
```

---

## Key Features of Amazon VPC

### 1. **Virtual Private Clouds (VPCs)**
- A customizable, isolated virtual network.
- You define the **IP address range**, **subnets**, **route tables**, and more.
- Can connect to on-premises networks using **VPN or Direct Connect**.

### 2. **Subnets**
- Logical subdivisions of a VPC’s IP address range.
- Each subnet resides in **one Availability Zone**.
- Can be **public** (internet-facing) or **private** (internal use only).

### 3. **IP Addressing**
- Supports both **IPv4** and **IPv6**.
- You can use **AWS-provided** addresses or **Bring Your Own IP (BYOIP)**.
- Commonly used for resources like EC2, NAT Gateways, and Load Balancers.

### 4. **Routing**
- **Route Tables** define how traffic moves within the VPC.
- You can direct traffic between subnets, to the internet, or on-premises.

### 5. **Gateways & Endpoints**
- **Internet Gateway**: Provides internet access to public subnets.
- **NAT Gateway**: Allows private subnets to access the internet securely.
- **VPC Endpoints**: Connect privately to AWS services without internet access.

### 6. **VPC Peering Connections**
- Connect two VPCs to route traffic between them using **private IPs**.

### 7. **Traffic Mirroring**
- Traffic Mirroring is a powerful feature in Amazon VPC that enables you to capture and analyze network traffic from Elastic Network Interfaces (ENIs). It allows you to copy incoming and outgoing packets at the network level and send them to security or monitoring appliances for deeper inspection.

### 8. **Transit Gateway**
- A **centralized hub** for connecting multiple VPCs and on-premises networks.

### 9. **VPC Flow Logs**
- Capture logs of all network traffic **into and out of network interfaces**.
- Useful for **security auditing and troubleshooting**.

### 10. **VPN Connections**
- Connect your VPC to **on-premises infrastructure securely** via IPsec VPN.

---

## Getting Started with Amazon VPC

### Default VPC
- Every AWS account includes a **default VPC in each region**.
- It’s preconfigured to launch EC2 instances immediately.

### Custom VPC
- You can create your own VPC with **custom subnets, IP ranges, gateways**, and routing configurations.

---

## Ways to Work with Amazon VPC

| Interface | Description |
|----------|-------------|
| **AWS Management Console** | Easy-to-use graphical interface |
| **AWS CLI** | Command-line access on Windows, Mac, Linux |
| **AWS SDKs** | Language-specific APIs for automation |
| **Query API** | Direct API calls using HTTP requests |

---

## Pricing Overview

There is **no cost for creating a VPC**, but certain **components are billable**, such as:

- **NAT Gateways**
- **Elastic IP addresses**
- **Traffic Mirroring**
- **Reachability Analyzer**
- **IP Address Manager (IPAM)**

---

## Public vs Private IP Addressing

### Public IPv4 Addresses
- Needed for **internet-accessible resources**.
- **Charged based on usage**, unless covered by Free Tier.

#### Types of Public IPv4 Addresses:
- **Elastic IP (EIP)** – Static IP, reassociable
- **EC2 Public IPv4** – Automatically assigned on launch
- **BYOIP** – Bring your own IPv4 address range
- **Service-managed IPs** – Assigned by AWS services like ECS, RDS, etc.

### Private IPv4 Addresses
- **RFC 1918 compliant**, internal communication only.
- **No additional cost**.

#### Common Services Using Public IPv4:
- Amazon EC2, RDS, ECS, EKS, EMR, Redshift
- AWS Global Accelerator
- NAT Gateway, Elastic Load Balancer
- Amazon WorkSpaces, AppStream 2.0

---


