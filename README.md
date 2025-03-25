# [Internet gateway](https://github.com/Mallick17/AWS-VPC/blob/Route_Tables,IGW-&-NAT_Gateway/README.md#1-what-is-an-internet-gateway-igw), [NAT gateway](https://github.com/Mallick17/AWS-VPC/blob/Route_Tables,IGW-&-NAT_Gateway/README.md#2-what-is-a-nat-gateway-ngw) & [Route tables]()
## Overview: AWS VPC Networking

Imagine your AWS VPC (Virtual Private Cloud) as a secure, fenced playground. Within this playground, you can have different areas (subnets) where your servers (like EC2 instances) live. Now, if you want to talk to the outside world (the Internet), you need special gateways or “doors.” There are two main types:

- **Internet Gateway (IGW):** This is like the main gate of the playground that allows open two-way communication with the Internet.
- **NAT Gateway (NGW):** This is like a one-way exit door for some areas of the playground. It lets servers go out to the Internet for updates or downloads but stops people from coming in directly.

---

## 1. What Is an Internet Gateway (IGW)?

### **Definition**
- An **Internet Gateway (IGW)** in AWS is a **logical component** that allows **communication between AWS resources inside a VPC (Virtual Private Cloud) and the internet**. 
- An **Internet Gateway** is a horizontally scaled, redundant, and highly available VPC component that:

- **Enables communication between your VPC and the Internet.**
- Supports **both inbound and outbound traffic** between the instances (like EC2 servers) and the outside world.
- Translates private IP addresses (inside your VPC) to public IP addresses for internet communication.

**Think of it like this:**
- It’s the **main door** of a house that connects the inside of your home (VPC) to the outside world (Internet).
- Without this door, you are completely **isolated** from the outside.

### **How It Works**
An **Internet Gateway (IGW)** works at the **VPC level**, providing **NAT (Network Address Translation)** for public IPs.  
It:
1. **Routes outbound traffic** from EC2 instances to the internet.
2. **Allows inbound traffic** from the internet to reach instances with a public IP.
3. **Performs NAT (if needed)** to map private IPs to public IPs.
- **Inbound:** When someone on the Internet requests access to your website (hosted on an EC2 instance), the request travels through the IGW to reach your instance.
- **Outbound:** When your server needs to fetch updates or connect to an external API, the traffic goes out through the IGW.

### **Why Do We Need an Internet Gateway?**
Imagine you have a **website running on an EC2 instance** inside AWS.  
- If your instance **doesn't have an Internet Gateway**, users **CANNOT** visit your website.
- The **Internet Gateway acts as a bridge** to let users access your application.

### **Real-World Example**
Imagine your VPC is a house. The Internet Gateway is the front door. If you want to invite guests (internet traffic) or leave the house (send out traffic), you need to use this door.

### **Key Points**
- **Public Subnet Requirement:** For an instance to be reachable from the Internet, it must be in a public subnet (a subnet that’s associated with a route pointing to the IGW).
- **Routing:** You must configure your VPC’s route table with a rule like `0.0.0.0/0 → IGW` to route all non-local traffic to the Internet.

---

## **Steps to Set Up an Internet Gateway in AWS**
Here’s how you set it up **step by step**:

### **Step 1: Create a VPC (if not already created)**
- Go to **AWS Console → VPC → Create VPC**.
- Give it a **name** and define an **IPv4 CIDR block** (e.g., `10.0.0.0/16`).

### **Step 2: Create an Internet Gateway**
- Go to **AWS Console → VPC → Internet Gateways**.
- Click **Create Internet Gateway** and give it a name.

### **Step 3: Attach Internet Gateway to VPC**
- Select your **newly created IGW**.
- Click **Attach to VPC** and select your VPC.

### **Step 4: Configure Route Table**
- Go to **VPC → Route Tables**.
- Select the **Main Route Table** (or create a new one).
- Click **Edit Routes** → **Add Route**:
  - **Destination:** `0.0.0.0/0`
  - **Target:** Select your **Internet Gateway (IGW)**.
- Click **Save Routes**.

### **Step 5: Modify the Subnet**
- Go to **VPC → Subnets**.
- Select your **public subnet**.
- Ensure it has the **Auto-assign Public IP** enabled.

### **Step 6: Security Groups and NACLs**
- Go to **EC2 → Security Groups**.
- Ensure you **allow inbound and outbound traffic** (e.g., allow **HTTP (80) and SSH (22)**).


---

## 2. What Is a NAT Gateway (NGW)?

### **Definition**
- A **NAT Gateway (Network Address Translation Gateway)** is a **managed AWS service** that allows instances in a **private subnet** to **access the internet** securely **without exposing them to inbound traffic from the internet.**
A **NAT Gateway** (Network Address Translation Gateway) allows instances in a **private subnet** (which has no direct Internet access) to access the Internet in a secure way:

- **Outbound Only:** It lets your instances reach the Internet (for software updates, API calls, etc.) but does not allow inbound traffic initiated from the Internet.
- **Hides Private IPs:** It translates private IP addresses to a public IP address when sending traffic out, and then routes the returning traffic back to the correct instance.

### **How It Works**
- **Outbound Traffic:** When an instance in a private subnet needs to download an update, its traffic is sent to the NAT Gateway. The NGW changes the source IP (private) to its own public IP and sends the request to the Internet.
- **Inbound Traffic:** The reply from the Internet returns to the NAT Gateway, which then forwards it back to the originating instance. However, no unsolicited inbound traffic is allowed.

### **Real-World Example**
Think of the NAT Gateway as a mailroom in an office building that collects outgoing mail and forwards it with a company address. People outside the building can’t just walk into the offices—they can only reply to the mail that was sent out.

### **Key Points**
- **Private Subnets:** NAT Gateways are typically used in private subnets where you do not want external traffic to directly reach your servers.
- **Security:** They provide an extra layer of security by preventing direct inbound Internet connections.

---

## 3. Comparing Internet Gateway and NAT Gateway

| **Feature**            | **Internet Gateway (IGW)**                                             | **NAT Gateway (NGW)**                                             |
|------------------------|------------------------------------------------------------------------|-------------------------------------------------------------------|
| **Usage Scenario**     | Public subnets: instances that need to be accessed directly from the Internet.  | Private subnets: instances that need outbound Internet access only. |
| **Traffic Direction**  | **Inbound and Outbound:** Allows two-way communication.                | **Outbound Only:** No unsolicited inbound connections allowed.    |
| **IP Address Requirement** | Instances require a public IP address to be reachable via IGW.         | Instances can use private IPs; NGW uses its own public IP for translation. |
| **Security**           | Less secure for sensitive data since they’re exposed to the Internet.  | More secure since instances remain hidden from direct Internet exposure.  |

---

## 4. Detailed Step-by-Step Setup Example

### **Setting Up an Internet Gateway**

1. **Create a VPC:**  
   - In the AWS console, go to VPC and create a new VPC (e.g., `10.0.0.0/16`).

2. **Create and Attach the IGW:**  
   - Create an Internet Gateway.
   - Attach it to your VPC.

3. **Configure the Route Table:**  
   - Edit your route table and add a rule:  
     **Destination:** `0.0.0.0/0`  
     **Target:** Your IGW.

4. **Assign Public IPs:**  
   - Ensure your public subnets auto-assign public IP addresses to instances.

### **Setting Up a NAT Gateway**

1. **Create a Private Subnet:**  
   - Define a private subnet in your VPC (e.g., `10.0.1.0/24`).

2. **Create a Public Subnet for the NAT Gateway:**  
   - You need a public subnet (with an IGW and route table entry for `0.0.0.0/0`) where the NAT Gateway will reside.

3. **Create the NAT Gateway:**  
   - Go to the NAT Gateways section in the VPC console.
   - Create a NAT Gateway in your public subnet and allocate an Elastic IP to it.

4. **Update the Private Subnet’s Route Table:**  
   - Edit the route table for the private subnet.
   - Add a route:  
     **Destination:** `0.0.0.0/0`  
     **Target:** The NAT Gateway.

---

## 5. Tips and Tricks

- **Double-Check Routes:**  
  Always verify that your route tables correctly direct traffic. A missing or incorrect route can block internet access.

- **Security Groups & NACLs:**  
  Ensure that your Security Groups and Network ACLs allow the desired inbound and outbound traffic. Sometimes the IGW or NAT Gateway works perfectly, but a security rule blocks the connection.

- **Cost Considerations:**  
  NAT Gateways incur additional costs based on usage. Plan accordingly if you have a lot of outbound traffic from private subnets.

- **High Availability:**  
  For production, consider deploying NAT Gateways in multiple Availability Zones (AZs) to ensure high availability and fault tolerance.

---

## Final Summary

- **Internet Gateway (IGW):**  
  - **Purpose:** Directly connect public subnets to the Internet.  
  - **Traffic:** Two-way (inbound and outbound).  
  - **Example:** Hosting a website accessible to everyone.

- **NAT Gateway (NGW):**  
  - **Purpose:** Provide secure outbound Internet access for instances in private subnets without exposing them.  
  - **Traffic:** Outbound only.  
  - **Example:** Allowing servers to download software updates while keeping them hidden from the Internet.

---

<details>
  <summary>Internet Gateway Vs NAT Gateway</summary>

## **Internet Gateway (IGW) vs. NAT Gateway in AWS VPC**
Both **Internet Gateway (IGW)** and **NAT Gateway** help AWS instances connect to the internet, but they serve **different purposes**.  

| Feature  | **Internet Gateway (IGW)** | **NAT Gateway** |
|----------|--------------------------|----------------|
| **Purpose** | Connects VPC to the **internet** | Allows **private subnets** to access the internet |
| **Used In** | Public Subnets | Private Subnets |
| **Inbound Traffic (From Internet)** | Allowed (if public IP assigned) | ❌ Not Allowed |
| **Outbound Traffic (To Internet)** | Allowed | Allowed |
| **Public IP Needed?** | Yes | No |
| **Route Table Entry** | `0.0.0.0/0 → IGW` | `0.0.0.0/0 → NAT Gateway` |

---

## **What is an Internet Gateway (IGW)?**
A **NAT Gateway (Network Address Translation Gateway)** is a **managed AWS service** that allows instances in a **private subnet** to **access the internet** securely **without exposing them to inbound traffic from the internet.**
- An **Internet Gateway (IGW)** is a **bridge** that connects an AWS **VPC to the internet**.  

💡 **Think of it like this:**  
- Your **VPC is a private neighborhood**.  
- The **Internet Gateway is the main gate** to enter or exit the neighborhood.  
- Without an IGW, no one inside the neighborhood can access the outside world (internet), and no one from the outside can come in.  

### **How It Works**
1. Your EC2 instance **must be in a public subnet**.  
2. It **must have a public IP**.  
3. The **route table** must have a rule:  
   ```
   Destination: 0.0.0.0/0 → Target: Internet Gateway
   ```
4. Security groups and NACLs must allow traffic.  

### **Example: Hosting a Website**
Let’s say you launch a **web server (EC2 instance)** and want users to visit your website.  
**Without an IGW** ❌ → No one can access it.  
**With an IGW** ✅ → Users can visit your site.

### **Setting Up an Internet Gateway**
1. **Create an IGW** (AWS Console → VPC → Internet Gateways → Create IGW).  
2. **Attach it to your VPC**.  
3. **Modify the Route Table** (`0.0.0.0/0 → IGW`).  
4. **Ensure EC2 has a Public IP**.  
5. **Check Security Group Rules** (allow HTTP/HTTPS).  

---

## **What is a NAT Gateway?**
A **NAT (Network Address Translation) Gateway** allows **instances in private subnets** to access the **internet**, but **prevents incoming traffic**.  

💡 **Think of it like this:**  
- You live in a **secured apartment** (private subnet).  
- You **can order food online** (access the internet).  
- But **delivery people CANNOT enter your building** (no inbound traffic).  

### **Why Do We Need a NAT Gateway?**
- Private EC2 instances (like **databases, backend servers**) **should NOT** have public IPs.  
- But they might still need to **download updates, software**, or **reach APIs on the internet**.  
- A **NAT Gateway acts as a middleman**:  
  - It **hides the private IPs**.  
  - It **lets private instances reach the internet**.  
  - It **blocks any incoming traffic** from the internet.  

### **How It Works**
1. **Private EC2 sends a request** (e.g., "Download software update").  
2. **NAT Gateway receives the request**, replaces the private IP with its own **public IP**, and forwards it to the internet.  
3. **Response comes back** to the NAT Gateway, which sends it to the EC2 instance.  

---

## **Example: Updating a Private Server**
- You have a **database server in a private subnet**.  
- It needs to **install security updates** from the internet.  
- **Without a NAT Gateway** ❌ → No internet access.  
- **With a NAT Gateway** ✅ → Can download updates, but remains **hidden from external access**.  

---

## **Setting Up a NAT Gateway**
1. **Launch a NAT Gateway** in a **public subnet**.  
2. **Assign an Elastic IP** (EIP) to the NAT Gateway.  
3. **Modify the Private Subnet’s Route Table**:  
   ```
   Destination: 0.0.0.0/0 → NAT Gateway
   ```
4. **Check Security Groups & NACLs** to allow outbound traffic.  

---

## **Tricks and Tips**
### **🚀 Internet Gateway Tips**
✔ **For public-facing applications** (websites, APIs).  
✔ **Instances must have a public IP**.  
✔ **Check security groups & NACLs** to allow inbound traffic.  

### **🔒 NAT Gateway Tips**
✔ **For private instances that need outbound internet access**.  
✔ **Place NAT Gateway in a public subnet**.  
✔ **Assign an Elastic IP** to ensure **consistent outbound traffic**.  
✔ **Private subnet route table must point to NAT Gateway**.  

---

## **Logical Questions & Answers**
### **Q1: Can an instance in a private subnet access the internet without a NAT Gateway?**
❌ **No.** Private instances need a **NAT Gateway** to reach the internet.  

---

### **Q2: Can an Internet Gateway be used in a private subnet?**
❌ **No.** An IGW is used for public subnets only.  

---

### **Q3: What’s the key difference between IGW and NAT Gateway?**
| Feature | **Internet Gateway (IGW)** | **NAT Gateway** |
|---------|----------------|--------------|
| **Purpose** | Full internet access | Only outbound access |
| **Inbound Traffic?** | Allowed | ❌ No inbound |
| **Used In** | Public subnets | Private subnets |
| **Public IP Needed?** | Yes | No |
| **Route Table Rule** | `0.0.0.0/0 → IGW` | `0.0.0.0/0 → NAT Gateway` |

---

### **Q4: Can a NAT Gateway allow inbound internet traffic?**
❌ **No.** It only allows **outbound** traffic.  

---

### **Q5: How do private instances send traffic to the internet?**
They send it **through a NAT Gateway** placed in a **public subnet**.  

---

## **Final Summary**
🚀 **Internet Gateway (IGW)**  
✔ Connects **public subnets** to the internet.  
✔ Required for **web servers, APIs, etc.**  
✔ Instances **must have a public IP**.  
✔ Needs **Route Table entry** (`0.0.0.0/0 → IGW`).  

🔒 **NAT Gateway**  
✔ Allows **private subnets** to access the internet.  
✔ Used for **databases, backend servers**.  
✔ **No inbound traffic** is allowed.  
✔ Needs **Route Table entry** (`0.0.0.0/0 → NAT Gateway`).  

🔥 **Common Mistakes & Fixes**
- ❌ **Forgetting to attach IGW/NAT** → No internet access.  
- ❌ **Not updating route tables** → Instances stuck.  
- ❌ **Security groups block traffic** → Allow HTTP, HTTPS, SSH.  

Now you know **everything** about **Internet Gateway and NAT Gateway in AWS VPC**! 🎯 🚀
  
</details>

---

## Route Tables
