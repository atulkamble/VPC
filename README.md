# AWS VPC (Virtual Private Cloud) Master Guide

## Section 1: Networking Fundamentals

* What is Networking?
* LAN (Local Area Network)
* WAN (Wide Area Network)
* Internet
* Router
* Switch
* Gateway
* Firewall
* DNS (Domain Name System)
* DHCP (Dynamic Host Configuration Protocol)

### Networking Analogy

```text
City
 ├── Roads
 ├── Buildings
 ├── Security Gates
 └── House Numbers

AWS
 ├── VPC
 ├── Route Tables
 ├── Security Groups
 └── IP Addresses
```

---

# Section 2: AWS Networking Full Forms

| Short Form | Full Form                      |
| ---------- | ------------------------------ |
| AWS        | Amazon Web Services            |
| VPC        | Virtual Private Cloud          |
| CIDR       | Classless Inter-Domain Routing |
| EC2        | Elastic Compute Cloud          |
| IGW        | Internet Gateway               |
| NAT        | Network Address Translation    |
| SG         | Security Group                 |
| NACL       | Network Access Control List    |
| ENI        | Elastic Network Interface      |
| EIP        | Elastic IP                     |
| ALB        | Application Load Balancer      |
| NLB        | Network Load Balancer          |
| TGW        | Transit Gateway                |
| VPN        | Virtual Private Network        |
| DX         | Direct Connect                 |
| AZ         | Availability Zone              |

---

# AWS VPC – IP Address Ranges and Limits

An AWS VPC (Virtual Private Cloud) uses CIDR (Classless Inter-Domain Routing) notation to define its IP address range.

## 1. AWS VPC CIDR range limits

Largest IPv4 VPC CIDR

# /16

65,536 total IP addresses

Smallest IPv4 VPC CIDR

# /28

16 total IP addresses

| CIDR | Subnet mask     | Total IPs |
| ---- | --------------- | --------- |
| /16  | 255.255.0.0     | 65,536    |
| /17  | 255.255.128.0   | 32,768    |
| /18  | 255.255.192.0   | 16,384    |
| /19  | 255.255.224.0   | 8,192     |
| /20  | 255.255.240.0   | 4,096     |
| /21  | 255.255.248.0   | 2,048     |
| /22  | 255.255.252.0   | 1,024     |
| /23  | 255.255.254.0   | 512       |
| /24  | 255.255.255.0   | 256       |
| /25  | 255.255.255.128 | 128       |
| /26  | 255.255.255.192 | 64        |
| /27  | 255.255.255.224 | 32        |
| /28  | 255.255.255.240 | 16        |

Important: AWS supports IPv4 VPC CIDR blocks from `/16` to `/28`. These limits apply to individual VPC CIDR blocks.

## 2. AWS subnet ranges and usable IPs

AWS reserves 5 IPv4 addresses in every subnet.

| Subnet CIDR | Total IPs | Usable IPs |
| ----------- | --------- | ---------- |
| /16         | 65,536    | 65,531     |
| /20         | 4,096     | 4,091      |
| /24         | 256       | 251        |
| /25         | 128       | 123        |
| /26         | 64        | 59         |
| /27         | 32        | 27         |
| /28         | 16        | 11         |

Example: `10.0.1.0/24`

| IP address          | AWS usage                             |
| ------------------- | ------------------------------------- |
| 10.0.1.0            | Network address                       |
| 10.0.1.1            | VPC router                            |
| 10.0.1.2            | DNS reservation                       |
| 10.0.1.3            | Reserved for future use               |
| 10.0.1.255          | Network broadcast address reservation |
| 10.0.1.4–10.0.1.254 | 251 usable IPs                        |

## 3. VPC CIDR and subnet architecture

AWS VPC: 10.0.0.0/16

65,536 IPv4 addresses

Public Subnet A

### 10.0.1.0/24

251 usable IPs

Private Subnet A

### 10.0.2.0/24

251 usable IPs

Public Subnet B

### 10.0.3.0/24

251 usable IPs

Private Subnet B

### 10.0.4.0/24

251 usable IPs

## 4. Important AWS VPC quotas

| Component                         | Default quota            |
| --------------------------------- | ------------------------ |
| VPCs per Region                   | 5                        |
| IPv4 CIDR blocks per VPC          | 5                        |
| Subnets per VPC                   | 200                      |
| Route tables per VPC              | 200                      |
| Routes per route table            | 50                       |
| Internet Gateways per Region      | 5                        |
| NAT Gateways per AZ               | 5                        |
| Security groups per VPC           | 2,500                    |
| Inbound rules per security group  | 60                       |
| Outbound rules per security group | 60                       |
| Network ACLs per VPC              | 200                      |
| Rules per Network ACL             | 20 inbound + 20 outbound |

These are common default quotas, not necessarily hard maximums. Many can be increased through AWS Service Quotas.

## 5. Private IP address ranges for VPCs

| Private IP range              | CIDR           | Total IPs  |
| ----------------------------- | -------------- | ---------- |
| 10.0.0.0 – 10.255.255.255     | 10.0.0.0/8     | 16,777,216 |
| 172.16.0.0 – 172.31.255.255   | 172.16.0.0/12  | 1,048,576  |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | 65,536     |

These are RFC 1918 private address ranges. A single IPv4 VPC CIDR block cannot exceed `/16`, even when the overall private address range is larger.

## 6. Quick CIDR calculator

Select CIDR prefix

## /24

Total IPv4 addresses

# 256

Usable subnet IPs

# 251

Formula: Total IPs = 2^(32 − CIDR prefix). AWS reserves 5 addresses per subnet.

Recommended for AWS practical labs: Use `10.0.0.0/16` for the VPC and `/24` for individual public and private subnets. This provides ample space for EC2, ALB, RDS, NAT Gateway, and EKS experiments.


# Section 3: IP Addressing Concepts

## What is an IP Address?

```text
10.0.1.10
192.168.1.10
172.16.1.10
```

Think:

```text
IP Address = House Number
```

---

## Private IP Ranges

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

AWS VPC uses these ranges.

---

## Public IP

Internet-routable IP.

Examples:

```text
54.x.x.x
3.x.x.x
13.x.x.x
```

---

# Section 4: CIDR & Subnetting

## CIDR

CIDR = Classless Inter-Domain Routing

Example:

```text
10.0.0.0/16
```

Meaning:

```text
Network Bits = 16
Host Bits = 16
```

---

## CIDR Size Table

| CIDR | Total IPs | AWS Usable |
| ---- | --------- | ---------- |
| /16  | 65536     | 65531      |
| /20  | 4096      | 4091       |
| /24  | 256       | 251        |
| /26  | 64        | 59         |
| /28  | 16        | 11         |

---

## CIDR Logic

```text
Smaller CIDR
=
Larger Network

/16
Large

/24
Medium

/28
Small
```

---

## AWS Reserved IPs

For subnet:

```text
10.0.1.0/24
```

Reserved:

```text
10.0.1.0
10.0.1.1
10.0.1.2
10.0.1.3
10.0.1.255
```

Usable:

```text
251
```

---

# Section 5: AWS Global Infrastructure

## Region

Examples:

```text
ap-south-1
us-east-1
eu-west-1
```

---

## Availability Zone

Examples:

```text
ap-south-1a
ap-south-1b
ap-south-1c
```

---

## Region Architecture

```text
Region
 |
 +--- AZ-A
 |
 +--- AZ-B
 |
 +--- AZ-C
```

---

# Section 6: VPC Fundamentals

## What is a VPC?

VPC = Virtual Private Cloud

Logical isolated network inside AWS.

Example:

```text
10.0.0.0/16
```

---

## VPC Architecture

```text
Internet
   |
  IGW
   |
VPC
10.0.0.0/16
```

---

# Section 7: Subnets

## Public Subnet

Requirements:

```text
Route to IGW
+
Public IP
```

Examples:

```text
Web Servers
ALB
Bastion Host
NAT Gateway
```

---

## Private Subnet

Requirements:

```text
No Public IP
No Direct IGW Route
```

Examples:

```text
Application Servers
Databases
Redis
EKS Nodes
```

---

# Section 8: Route Tables

Purpose:

```text
Control Traffic Routing
```

Example:

| Destination | Target |
| ----------- | ------ |
| 10.0.0.0/16 | Local  |
| 0.0.0.0/0   | IGW    |

Think:

```text
AWS GPS System
```

---

# Section 9: Internet Gateway

IGW = Internet Gateway

Purpose:

```text
VPC ↔ Internet
```

Architecture:

```text
Internet
   |
  IGW
   |
  VPC
```

---

# Section 10: Security Groups

Security Group = Instance Firewall

Properties:

```text
Stateful

Allow Only
```

Example:

```text
SSH 22
HTTP 80
HTTPS 443
```

---

# Section 11: Network ACL

NACL = Network Access Control List

Subnet Firewall

Properties:

```text
Stateless

Allow
Deny
```

---

# Security Group vs NACL

| Feature    | SG       | NACL   |
| ---------- | -------- | ------ |
| Level      | Instance | Subnet |
| Stateful   | Yes      | No     |
| Deny Rules | No       | Yes    |

---

# Section 12: NAT Gateway

NAT = Network Address Translation

Purpose:

```text
Private EC2
      |
Internet Access
```

Architecture:

```text
Private EC2
      |
 NAT Gateway
      |
 Public Subnet
      |
 IGW
      |
Internet
```

---

# Section 13: Bastion Host

Purpose:

Secure SSH Access.

Architecture:

```text
Internet
     |
 Bastion
     |
 Private EC2
```

---

# Section 14: VPC Endpoints

Purpose:

Private connectivity to AWS services.

Example:

```text
EC2
 |
VPC Endpoint
 |
S3
```

Benefits:

* No Internet
* No NAT
* Secure Access

---

# Section 15: Load Balancers

## ALB

Application Load Balancer

Layer 7

```text
HTTP
HTTPS
```

---

## NLB

Network Load Balancer

Layer 4

```text
TCP
UDP
```

---

Architecture:

```text
Internet
    |
   ALB
    |
EC2
EC2
EC2
```

---

# Section 16: Connectivity Flow

```text
Client
   |
Internet
   |
IGW
   |
Route Table
   |
NACL
   |
Security Group
   |
EC2
```

Most important interview diagram.

---

# Section 17: VPC Peering

Connect two VPCs.

Rule:

```text
No Overlapping CIDR
```

Architecture:

```text
VPC-A
10.0.0.0/16
     |
Peering
     |
VPC-B
10.1.0.0/16
```

---

# Section 18: Transit Gateway

TGW = Transit Gateway

Hub and Spoke Network.

```text
      TGW
   /   |   \
VPC  VPC  VPN
```

---

# Section 19: VPN

VPN = Virtual Private Network

```text
On-Prem
    |
 VPN
    |
 AWS
```

Encrypted connection.

---

# Section 20: Direct Connect

DX = Direct Connect

Dedicated private connection.

```text
Datacenter
      |
Direct Connect
      |
AWS
```

---

# Section 21: Production 3-Tier Architecture

```text
                 Internet
                     |
                  Route53
                     |
                    ALB
                     |
      ------------------------------
      |                            |
    Web-1                       Web-2
      |                            |
      ------------------------------
                     |
      ------------------------------
      |                            |
    App-1                       App-2
      |                            |
      ------------------------------
                     |
                    RDS
```

---

# Section 22: Connectivity Logic Charts

### Is EC2 Public?

```text
Public IP?
     |
    Yes
     |
Route to IGW?
     |
    Yes
     |
Public EC2
```

---

### Private EC2 Internet Access?

```text
Private EC2
      |
NAT Gateway?
      |
     Yes
      |
Internet Access
```

---

# Section 23: Troubleshooting Flow

```text
Cannot Access EC2
       |
Instance Running?
       |
Public IP?
       |
IGW Attached?
       |
Route Table Correct?
       |
SG Allows Traffic?
       |
NACL Allows Traffic?
       |
OS Firewall?
       |
Success
```

---

# Section 24: Hands-On Labs

1. Create Custom VPC
2. Public & Private Subnets
3. Route Tables
4. Internet Gateway
5. NAT Gateway
6. Bastion Host
7. VPC Endpoint
8. ALB Setup
9. VPC Peering
10. Transit Gateway
11. Site-to-Site VPN
12. Multi-AZ Architecture

---

# Section 25: Architecture Design Best Practices

### Recommended CIDR

```text
VPC
10.0.0.0/16
```

### Recommended Subnets

```text
Public-A    10.0.1.0/24
Public-B    10.0.2.0/24

App-A       10.0.10.0/24
App-B       10.0.11.0/24

DB-A        10.0.20.0/24
DB-B        10.0.21.0/24
```

### Best Practices

* Minimum 2 AZs
* ALB in Public Subnets
* Applications in Private Subnets
* Databases in Private Subnets
* NAT Gateway per AZ
* Avoid Public Databases
* Use Security Groups with least privilege
* Enable VPC Flow Logs
* Use VPC Endpoints for AWS services

---

# Final Quick Revision

```text
VPC = Network

Subnet = Network Segment

CIDR = Network Size

IGW = Internet Access

NAT = Private Internet Access

SG = Stateful Firewall

NACL = Stateless Firewall

ALB = Layer 7

NLB = Layer 4

Peering = VPC-to-VPC

TGW = Multi-VPC Hub

VPN = Encrypted Tunnel

DX = Dedicated Connection

Public Subnet = IGW Route + Public IP

Private Subnet = No Direct IGW Route

Connectivity = Route + NACL + SG
```

This structure is suitable for AWS Cloud Practitioner, AWS Solutions Architect Associate (SAA-C03), AWS Advanced Networking, AWS DevOps Engineer, Linux/Cloud training, and interview preparation.
