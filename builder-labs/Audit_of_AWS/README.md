# Lab: Performing a Basic Audit of Your AWS Environment

## Source
**AWS Skill Builder (Builder Lab SPL-73)**

## Objective
To practice auditing the security posture of an AWS environment by verifying identity permissions, network isolation, and activity logging configurations.

## Skills Validated
* **IAM Analysis:** Validated user permissions and utilized the **IAM Policy Simulator** to confirm that actions not explicitly allowed are "Denied by Default" (Implicit Deny).
* **Network Hardening:** Audited **Security Groups** to verify a **Bastion Host** architecture, ensuring private instances (Web/SQL) only accept traffic from the hardened jump box.
* **Firewall Logic:** Distinguished between stateful **Security Groups** (Instance-level) and stateless **Network ACLs** (Subnet-level).
* **Forensics & Monitoring:** Leveraged **CloudTrail** to track API activity (the "Who" and "When") and **CloudWatch** to identify performance anomalies (the "Symptoms").

## Key Concept Learned
"CloudWatch tells you *what* is happening (e.g., the server stopped), but CloudTrail tells you *who* did it (e.g., User A deleted the instance). An effective audit requires both."