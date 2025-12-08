# Lab: Introduction to IAM (Identity & Access Management)

## Source
**AWS Skill Builder (Builder Lab SPL-66)**

## Objective
To implement a "Least Privilege" security model by managing IAM Users, Groups, and Policies. The goal was to simulate a real-world scenario where different employees require distinct levels of access to AWS resources based on their job functions.

## Skills Validated
* **Identity Management:** Explored the structure of IAM Users and Groups.
* **Policy Analysis:** Inspected **JSON-based IAM Policies** to understand the specific API actions allowed (e.g., `ec2:StartInstances`, `s3:ListBucket`) versus those that are denied.
* **Access Control Implementation:**
    * **S3 Support:** Assigned a user to a group with **Read-Only** access to S3.
    * **EC2 Support:** Assigned a user to a group with **Read-Only** access to EC2 (could view instances but failed when attempting to "Stop" one).
    * **EC2 Admin:** Assigned a user to a group with **Full Administrative** control over EC2 (successfully stopped instances).
* **Testing & Verification:** Used the unique **IAM Sign-in URL** to log in as different users and verify that permissions were correctly enforced by the AWS Cloud.

## Key Concept Learned
"I learned that permissions are best managed by attaching policies to **Groups** rather than individual Users. This allows for scalable administration—when a new employee joins a team, I simply add them to the Group, and they instantly inherit the correct permissions."