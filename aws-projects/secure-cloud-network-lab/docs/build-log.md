# Engineering Build Log

## September 13–14, 2026 — Project Initialization

### Objective

Establish the local project structure and prepare for a controlled AWS deployment.

### Work Completed

- Created the secure cloud network project directory.
- Created directories for documentation, diagrams, screenshots, Terraform, and Python.
- Created the initial README and engineering build log files.
- Verified the existing local Git repository and its GitHub remote.
- Confirmed that the repository uses the `main` branch and SSH authentication.
- Corrected a Linux path-capitalization mistake that created two separate project directories.
- Moved the lab into the correct Git-tracked repository.

### Technical Lesson

Linux paths are case-sensitive. The following paths refer to two different directories:

- `/home/zac/projects/cloud-engineering`
- `/home/zac/Projects/Cloud-Engineering`

The lowercase directory was verified as the Git repository connected to GitHub. The new lab was moved into that repository before being staged or committed.

### Design Decision

The AWS environment will first be constructed manually to understand each component and its relationships. It will later be reproduced with Terraform and extended with Python and Boto3 automation.

### Cost Status

No AWS resources have been created for this project. Current project cost: $0.

### Next Step

Commit the initial documentation to GitHub, configure AWS budget alerts, and design the VPC address plan before deploying resources.