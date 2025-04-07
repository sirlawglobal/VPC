
# 🚀 AWS VPC Setup - KCVPC

## 📘 Overview

This project outlines the setup of a **Virtual Private Cloud (VPC)** in the **EU-West-1 (Ireland)** region on **Amazon Web Services (AWS)**. It includes the creation of a custom VPC with public and private subnets, routing via Internet and NAT Gateways, security configurations, and EC2 instance deployments.  

---

## 🏗️ Architecture Diagram  
![VPC Architecture](https://github.com/user-attachments/assets/399456a9-d79f-40d0-9c50-04e3ee56a4bf)

---

## 🧱 Components & Configuration

### 1. **VPC**
- **Name:** `KCVPC`
- **CIDR Block:** `10.0.0.0/16`

### 2. **Subnets**
- **Public Subnet:** `10.0.1.0/24` (Availability Zone: `eu-west-1a`)
- **Private Subnet:** `10.0.2.0/24` (Availability Zone: `eu-west-1a`)

### 3. **Internet Gateway (IGW)**
- Name: `KCVPC-IGW`
- Attached to the VPC to enable internet access for public subnet resources.

### 4. **Route Tables**
- **Public Route Table**
  - Associated with Public Subnet.
  - Route: `0.0.0.0/0 → Internet Gateway`
- **Private Route Table**
  - Associated with Private Subnet.
  - Route: `0.0.0.0/0 → NAT Gateway`

### 5. **NAT Gateway**
- Deployed in the Public Subnet.
- Attached to an Elastic IP to allow outbound internet access from private subnet resources.

### 6. **Security Groups**
#### ✅ `PublicSG`
- Allow HTTP (port 80) and HTTPS (port 443) from **anywhere**
- Allow SSH (port 22) from a **specific IP**

#### 🔐 `PrivateSG`
- Allow MySQL (port 3306) traffic **only from Public Subnet**

### 7. **Network ACLs (NACLs)**
- **Public NACL**
  - Inbound & Outbound: Allow HTTP, HTTPS, SSH
- **Private NACL**
  - Allow traffic from the Public Subnet and enable outbound internet access

### 8. **EC2 Instances**
- **Public EC2 Instance**
  - Launched in the Public Subnet
  - Accessible via SSH and HTTP/HTTPS
- **Private EC2 Instance**
  - Launched in the Private Subnet
  - Has outbound internet access via NAT Gateway

---

## 📸 Deployment Steps with Screenshots

### 🔧 Step 1: Create the VPC  
Go to **VPC Dashboard → Create VPC**  
- Name: `KCVPC`  
- CIDR Block: `10.0.0.0/16`  
![VPC](https://github.com/user-attachments/assets/2deaa0ec-2ae3-48c7-a2bf-e4a3bec10df2)

---

### 🌐 Step 2: Create Subnets  
- Create **PublicSubnet**: `10.0.1.0/24`  
- Create **PrivateSubnet**: `10.0.2.0/24`  
![Private Subnet](https://github.com/user-attachments/assets/83a5413e-f0e3-4adc-b235-e1a38e05bd60)  
![Subnets](https://github.com/user-attachments/assets/8163748b-6cb8-4576-9ed9-a9dd890dc702)

---

### 🌍 Step 3: Attach Internet Gateway  
- Create and attach **KCVPC-IGW** to `KCVPC`  
![IGW](https://github.com/user-attachments/assets/2b208c25-e834-42e2-89ec-f5b9869e1633)

---

### 📡 Step 4: Configure Route Tables  
- **Public Route Table**: Add `0.0.0.0/0 → IGW`, associate with Public Subnet  
- **Private Route Table**: Add `0.0.0.0/0 → NAT Gateway`, associate with Private Subnet  
![Route Table](https://github.com/user-attachments/assets/53ae02db-824c-4d53-8ecf-6dd479387c46)

---

### 💡 Step 5: Create NAT Gateway  
- Allocate Elastic IP  
- Launch NAT Gateway in Public Subnet  
- Assign the Elastic IP to the NAT Gateway  
![NAT Gateway](https://github.com/user-attachments/assets/21da9b9e-7498-4a16-aba6-d3d462f3848d)

---

### 🔒 Step 6: Configure Security Groups  
- **PublicSG**: Allow HTTP, HTTPS (from all), SSH (from your IP)  
- **PrivateSG**: Allow MySQL from Public Subnet  
![PrivateSG](https://github.com/user-attachments/assets/4c5a966c-05f5-4d09-9529-d5f9894d3712)  
![PublicSG](https://github.com/user-attachments/assets/0de72e46-12bf-432f-911e-1df4fb567e7e)

---

### ⚙️ Step 7: Configure NACLs  
- **PublicNACL**: Allow HTTP, HTTPS, SSH  
- **PrivateNACL**: Allow traffic from PublicSubnet and internet access  
![Elastic IP](https://github.com/user-attachments/assets/02bcd335-79b3-4ce1-affd-b1b9ed9fee40)  
![Route to IGW](https://github.com/user-attachments/assets/97305f2d-0770-4821-9b2e-cf03af6c59e0)

---

### 💻 Step 8: Launch EC2 Instances  
- **Public Instance**:  
  - Subnet: PublicSubnet  
  - SG: PublicSG  
  - Test: SSH + HTTP Access  
- **Private Instance**:  
  - Subnet: PrivateSubnet  
  - SG: PrivateSG  
  - Test: Internet access via NAT  
![Private EC2](https://github.com/user-attachments/assets/99063075-758b-4a3f-b20b-00174703e304)  
![Public EC2](https://github.com/user-attachments/assets/7df69054-1a80-44ba-bb9b-c001a44a9275)

---

## ✅ Conclusion

This setup demonstrates a secure and scalable AWS network architecture following best practices. The `KCVPC` VPC provides isolated environments for both public-facing and internal resources, ensuring proper routing, access control, and security group segregation. Ideal for applications requiring internet-facing services with backend services shielded in private subnets.
