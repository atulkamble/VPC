## AWS Networking Fundamentals – Before Learning VPC


| #  | Concept           | Significance / Why Important                          | Short Example                 |
| -- | ----------------- | ----------------------------------------------------- | ----------------------------- |
| 1  | Computer Network  | Connects devices to communicate                       | Two computers sharing files   |
| 2  | LAN               | Connects devices within a small area                  | Office network                |
| 3  | WAN               | Connects networks across large distances              | Internet                      |
| 4  | IP Address        | Unique logical address of a device                    | `192.168.1.10`                |
| 5  | IPv4              | 32-bit IP addressing                                  | `10.0.0.1`                    |
| 6  | IPv6              | 128-bit IP addressing                                 | `2001:db8::1`                 |
| 7  | Public IP         | Enables internet communication                        | EC2 public IP                 |
| 8  | Private IP        | Communication within private networks                 | `10.0.1.10`                   |
| 9  | Static IP         | IP address that remains fixed                         | AWS Elastic IP                |
| 10 | Dynamic IP        | IP assigned automatically                             | DHCP-assigned IP              |
| 11 | Subnet            | Divides a network into smaller networks               | `10.0.1.0/24`                 |
| 12 | Subnet Mask       | Identifies network and host portions                  | `255.255.255.0`               |
| 13 | CIDR              | Defines IP address ranges                             | `10.0.0.0/16`                 |
| 14 | Router            | Connects different networks                           | Home Wi-Fi router             |
| 15 | Routing Table     | Determines where network traffic goes                 | `0.0.0.0/0 → IGW`             |
| 16 | Default Gateway   | Route used when no specific route matches             | Router `192.168.1.1`          |
| 17 | DNS               | Converts domain names to IP addresses                 | `google.com → IP`             |
| 18 | DHCP              | Automatically assigns network settings                | Laptop receives IP            |
| 19 | MAC Address       | Identifies a network interface at Layer 2             | `00:1A:2B:3C:4D:5E`           |
| 20 | TCP               | Reliable, connection-oriented communication           | HTTPS, SSH                    |
| 21 | UDP               | Connectionless communication with low overhead        | DNS, streaming                |
| 22 | Port Number       | Identifies an application service                     | SSH 22, HTTP 80               |
| 23 | HTTP              | Transfers web content                                 | `http://example.com`          |
| 24 | HTTPS             | Encrypts web communication using TLS                  | `https://aws.amazon.com`      |
| 25 | SSH               | Secure remote server access                           | `ssh ec2-user@IP`             |
| 26 | ICMP              | Network diagnostics and error reporting               | `ping 8.8.8.8`                |
| 27 | Firewall          | Controls allowed and blocked traffic                  | Allow TCP 443                 |
| 28 | NAT               | Translates IP addresses                               | Private EC2 outbound internet |
| 29 | VPN               | Creates an encrypted network connection               | Office to AWS VPC             |
| 30 | Load Balancer     | Distributes traffic across servers                    | ALB → EC2 instances           |
| 31 | OSI Model         | Explains networking in seven layers                   | Layer 3 = Network             |
| 32 | TCP/IP Model      | Explains practical internet communication             | IP + TCP + HTTP               |
| 33 | Bandwidth         | Network data transfer capacity                        | 1 Gbps                        |
| 34 | Latency           | Delay in network communication                        | 20 ms                         |
| 35 | Availability Zone | Isolated infrastructure location within an AWS Region | `us-east-1a`                  |

## Important IP Address and CIDR Examples

| CIDR              | Total IP Addresses | Typical Usage      |
| ----------------- | ------------------ | ------------------ |
| `10.0.0.0/16`     | 65,536             | VPC CIDR           |
| `10.0.1.0/24`     | 256                | Public subnet      |
| `10.0.2.0/24`     | 256                | Private subnet     |
| `10.0.3.0/24`     | 256                | Database subnet    |
| `0.0.0.0/0`       | All IPv4 addresses | Default IPv4 route |
| `192.168.1.10/32` | 1                  | Specific host IP   |

AWS Note: AWS reserves five IPv4 addresses in each subnet. Therefore, a `/24` subnet provides 251 usable IPv4 addresses in AWS.
