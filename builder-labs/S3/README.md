# Lab: Amazon S3 Storage & Security Fundamentals

## Source
**AWS Skill Builder (Builder Lab SPL-TF-100)**

## Objective
To configure a secure, scalable object storage solution for a theoretical application, focusing on access control, data durability, and programmatic access via the AWS CLI.

## Skills Validated
* **Bucket Configuration:** Created an S3 bucket with unique naming conventions and defined Region placement.
* **Access Management (Public vs. Private):**
    * Navigated the interaction between **Block Public Access (BPA)** settings and **Access Control Lists (ACLs)**.
    * Successfully transitioned an object from private to public visibility for web hosting scenarios.
* **Bucket Policies (JSON):**
    * Used the AWS Policy Generator to author a JSON Bucket Policy.
    * Implemented a "Least Privilege" architecture: Granted `PutObject` permissions *only* to a specific EC2 IAM Role, while granting `GetObject` (Read) access to the public.
* **Data Durability (Versioning):** Enabled **Bucket Versioning** to protect against accidental deletion. Demonstrated how to restore files using "Delete Markers" and how to permanently delete specific object versions.
* **Programmatic Access:** Connected to an EC2 instance via Session Manager and used the **AWS CLI** (`aws s3 ls`, `aws s3 cp`) to upload and retrieve data, verifying permissions from a command-line interface.

## Key Concept Learned
"I learned that when Versioning is enabled, 'Deleting' a file doesn't actually remove it—it adds a 'Delete Marker.' To truly restore the file, you don't re-upload it; you simply delete the Delete Marker."