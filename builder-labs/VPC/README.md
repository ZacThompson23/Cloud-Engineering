# Lab: VPC Network Architecture Fundamentals

## Source
**AWS Skill Builder (Builder Lab SPL-84)**

## Objective
To practice constructing a logically isolated virtual network (VPC) and configuring traffic routing between public and private subnets.

## Skills Validated
* **VPC Creation:** Deployed a VPC with a custom CIDR block (`10.0.0.0/16`).
* **Subnetting:** Configured a **Public Subnet** (for web resources) and a **Private Subnet** (for isolated back-end resources).
* **Routing Tables:**
    * Associated the Public Subnet with an **Internet Gateway (IGW)**.
    * Associated the Private Subnet with a **NAT Gateway** to allow secure outbound internet access without inbound exposure.
* **Traffic Analysis:** Inspected the difference between Security Groups (Instance-level) and Network ACLs (Subnet-level).

## Key Concept Learned
"The difference between a Public and Private subnet isn't just the name—it's the Route Table. Without a route to an Internet Gateway, a subnet is effectively private."