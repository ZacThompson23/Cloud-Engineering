S3 + EC2 Report Bucket Lab (AWS Builder Labs)
Overview

This lab walks through configuring an Amazon S3 bucket for storing report data generated on an EC2 instance. The goal was to understand S3 bucket creation, object security, public access behavior, IAM permissions, bucket policies, EC2 → S3 interaction using the AWS CLI, and S3 Versioning.

This was completed inside an AWS Skill Builder “Builder Lab” using a temporary AWS account environment.

What I Did (Summary)
1. Created an S3 Bucket

Created a unique S3 bucket to store reporting data.

Uploaded an object to test initial behavior.

Attempted to access the object via its URL while Block Public Access (BPA) was enabled.

As expected, the object was private and returned AccessDenied.

Disabled BPA (not something you'd do in production) to test public access behavior.

2. Connected an EC2 Instance to S3 Using SSM

Connected to an EC2 instance via AWS Systems Manager Session Manager (SSM)—no SSH keys or open ports needed.

From the EC2 terminal, attempted to upload (PutObject) to the S3 bucket using AWS CLI.

Upload failed due to missing permissions on the EC2 instance role.

3. Configured Bucket Policy for EC2 Access

Identified the EC2 instance's IAM Role (EC2InstanceProfileRole).

Created a bucket policy granting that role:

s3:GetObject

s3:PutObject

Resource specified as:
arn:aws:s3:::<bucket-name>/*

After applying, EC2 successfully:

Uploaded a file to S3

Downloaded a file using the AWS CLI

4. Added Public Read Access (For Testing)

Added a second bucket policy statement granting:

Principal: "*" (everyone)

Action: s3:GetObject

This allowed any browser to access objects through their Object URLs.

Verified that previously private objects were now publicly viewable.

5. Enabled and Practiced S3 Versioning

Enabled Versioning on the S3 bucket to protect against accidental deletion.

Uploaded a second sample-file.txt with the same name to generate a new version.

Practiced versioning behavior:

Saw how S3 creates unique version IDs.

Deleted the object (created a delete marker, not an actual delete).

Removed the delete marker to restore the object.

Permanently deleted a specific version using the version ID.

Key Skills Practiced

S3 bucket creation & configuration

Block Public Access (BPA) behavior

IAM Roles and bucket policies

EC2 → S3 connectivity through AWS CLI

Understanding and using ARNs

Public access controls and security implications

S3 Versioning, delete markers, and version recovery

Hands-on AWS resource interaction via SSM

AWS Services Used

Amazon S3

Amazon EC2

AWS IAM

AWS Systems Manager (SSM) / Session Manager

AWS CLI

What I Learned

IAM Role permissions control what an EC2 instance can do inside AWS.

Block Public Access overrides everything — until you explicitly disable it.

Bucket policies must reference objects using /*.

Public access is granted with Principal: "*".

S3 Versioning protects against accidental deletion by using delete markers.

AWS SSM lets you access EC2 securely without SSH keys or open ports.

Everything in AWS is tied to ARNs — precise identifiers for resources.

Next Steps

I’ll continue working through more AWS Builder Labs to build hands-on experience, especially around:

IAM Roles & Policies

DynamoDB integration

VPC basics

EC2 networking

CloudFormation and automation