# Lab: Amazon EC2 Linux Instance Lifecycle

## Source
**AWS Skill Builder (Builder Lab SPL-200)**

## Objective
To gain hands-on experience with the complete lifecycle of an Amazon EC2 instance, focusing on bootstrapping, security, and vertical scaling.

## Skills Validated
* **Instance Provisioning:** Launched an Amazon Linux 2023 instance using the AWS Console.
* **Bootstrapping:** Used `User Data` scripts to automatically install an Apache HTTP server upon launch.
* **Security Configuration:** Modified **Security Groups** to allow HTTP (Port 80) traffic while maintaining least-privilege access.
* **Vertical Scaling:** Performed an instance resize (stopped instance -> changed type from `t3.micro` to `t3.small` -> restarted) to simulate handling increased load.
* **Storage Management:** Modified an EBS volume size and verified the separation of Compute (EC2) and Storage (EBS).

## Key Concept Learned
"I learned that Security Groups are stateful firewalls. If I allow traffic IN on Port 80, the return traffic is automatically allowed OUT, regardless of the source port."