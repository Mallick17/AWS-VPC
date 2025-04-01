# VPC in AWS Console
## Creating VPC Manually
#### Step 1: Create a Custom VPC
1. **Navigate to VPC**
   - In the AWS Management Console, search for "VPC" and select it.

2. **Create VPC**
   - Click **Create VPC**.
   - **Name**: `php-mallow-vpc`.
   - **IPv4 CIDR block**: `10.0.0.0/16`.
   - **IPv6 CIDR block**: None.
   - **Tenancy**: Default.
   - **Enable DNS hostnames**: Yes.
   - Click **Create VPC**.

**Reason**:
- A custom VPC provides a logically isolated network environment for your resources, allowing you to define your own IP address range and subnet structure. The CIDR block `10.0.0.0/16` ensures enough IP addresses for multiple subnets. Enabling DNS hostnames allows instances in the VPC to resolve domain names (e.g., the RDS endpoint) without additional configuration.

---

#### Step 2: Create Subnets
1. **Navigate to Subnets**
   - In the VPC dashboard, select **Subnets** from the left sidebar.

2. **Create Public Subnets**
   - Click **Create subnet**.
   - **VPC**: `php-mallow-vpc`.
   - **Subnet 1**:
     - Name: `Public-1a`.
     - Availability Zone: `ap-south-1a`.
     - IPv4 CIDR block: `10.0.1.0/24`.
   - **Subnet 2**:
     - Name: `Public-1b`.
     - Availability Zone: `ap-south-1b`.
     - IPv4 CIDR block: `10.0.2.0/24`.
   - Click **Create subnet**.
   - Enable auto-assign public IP for both subnets:
     - Select `Public-1a`, click **Actions** > **Edit subnet settings**, enable **Auto-assign public IPv4 address**, and save.
     - Repeat for `Public-1b`.

3. **Create Private Subnets**
   - Click **Create subnet**.
   - **VPC**: `php-mallow-vpc`.
   - **Subnet 3**:
     - Name: `Private-1a`.
     - Availability Zone: `ap-south-1a`.
     - IPv4 CIDR block: `10.0.3.0/24`.
   - **Subnet 4**:
     - Name: `Private-1b`.
     - Availability Zone: `ap-south-1b`.
     - IPv4 CIDR block: `10.0.4.0/24`.
   - Click **Create subnet**.

**Reason**:
- Subnets divide the VPC’s IP range into smaller segments, allowing you to place resources in different Availability Zones (AZs) for high availability. Public subnets (`Public-1a`, `Public-1b`) are for resources that need internet access (e.g., the EC2 instance hosting the PHP app). Private subnets (`Private-1a`, `Private-1b`) are for resources that should not be publicly accessible (e.g., the RDS database). Enabling auto-assign public IP for public subnets ensures EC2 instances in these subnets can communicate with the internet.

---

#### Step 3: Create an Internet Gateway
1. **Navigate to Internet Gateways**
   - In the VPC dashboard, select **Internet Gateways** from the left sidebar.

2. **Create Internet Gateway**
   - Click **Create internet gateway**.
   - **Name**: `php-mallow-IGW`.
   - Click **Create**.

3. **Attach to VPC**
   - Select `php-mallow-IGW`, click **Actions** > **Attach to VPC**.
   - **VPC**: `php-mallow-vpc`.
   - Click **Attach**.

**Reason**:
- An Internet Gateway allows resources in the VPC (e.g., EC2 instances in public subnets) to connect to the internet and be accessible from the internet. This is necessary for the PHP application server to serve web traffic and for you to SSH into the EC2 instance.

---

#### Step 4: Create Route Tables
1. **Navigate to Route Tables**
   - In the VPC dashboard, select **Route Tables** from the left sidebar.

2. **Create Public Route Table**
   - Click **Create route table**.
   - **Name**: `php-mallow-public-RT`.
   - **VPC**: `php-mallow-vpc`.
   - Click **Create**.
   - Add route:
     - Select `php-mallow-public-RT`, go to **Routes** tab, click **Edit routes**.
     - Add: Destination `0.0.0.0/0`, Target `php-mallow-IGW`.
     - Click **Save routes**.
   - Associate subnets:
     - Go to **Subnet associations** tab, click **Edit subnet associations**.
     - Select `Public-1a` and `Public-1b`.
     - Click **Save associations**.

3. **Create Private Route Table**
   - Click **Create route table**.
   - **Name**: `php-mallow-private-RT`.
   - **VPC**: `php-mallow-vpc`.
   - Click **Create**.
   - Associate subnets:
     - Go to **Subnet associations** tab, click **Edit subnet associations**.
     - Select `Private-1a` and `Private-1b`.
     - Click **Save associations**.

**Reason**:
- Route tables control the traffic flow within the VPC. The public route table routes internet-bound traffic (`0.0.0.0/0`) to the Internet Gateway, allowing instances in public subnets to access the internet. The private route table does not have an internet route, ensuring instances in private subnets (e.g., RDS) are isolated from the internet. Associating subnets with the correct route tables ensures proper network behavior.

---

#### Step 5: Create a NAT Gateway
1. **Navigate to NAT Gateways**
   - In the VPC dashboard, select **NAT Gateways** from the left sidebar.

2. **Create NAT Gateway**
   - Click **Create NAT gateway**.
   - **Name**: `php-mallow-NATGW`.
   - **Subnet**: `Public-1a`.
   - **Elastic IP**: Allocate a new Elastic IP.
   - Click **Create NAT gateway**.

3. **Update Private Route Table**
   - Go to **Route Tables**, select `php-mallow-private-RT`.
   - Go to **Routes** tab, click **Edit routes**.
   - Add: Destination `0.0.0.0/0`, Target `php-mallow-NATGW`.
   - Click **Save routes**.

**Reason**:
- A NAT Gateway allows instances in private subnets (e.g., RDS) to access the internet for updates or backups without being publicly accessible. Placing the NAT Gateway in a public subnet ensures it can route traffic to the internet via the Internet Gateway. Updating the private route table ensures outbound internet traffic from private subnets goes through the NAT Gateway.

---
