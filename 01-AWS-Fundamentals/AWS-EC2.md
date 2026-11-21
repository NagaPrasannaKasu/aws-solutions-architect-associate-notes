# Amazon EC2 - Simplified Study Notes

## 1. What is Amazon EC2?

- **Amazon EC2 (Elastic Compute Cloud)** is an Infrastructure as a Service (IaaS) service.
- It allows you to rent virtual machines in AWS. These virtual machines are called **EC2 instances**.
- In an EC2-based architecture, you can use other AWS services to build and manage your application:
  - **EC2:** Runs virtual machines.
  - **EBS (Elastic Block Store):** Provides storage volumes for EC2 instances.
  - **EFS (Elastic File System):** Provides network file storage.
  - **ELB (Elastic Load Balancing):** Distributes incoming traffic across multiple targets, such as EC2 instances.
  - **ASG (Auto Scaling Group):** Automatically adjusts the number of EC2 instances based on scaling conditions.

## 2. EC2 Storage Options

EC2 can use different types of storage:

- **Network-attached storage:**
  - **EBS:** Block storage volumes that can be attached to EC2 instances.
  - **EFS:** Network file storage that can be accessed by multiple clients or instances.
- **EC2 Instance Store:**
  - Storage located on hardware attached to the physical host running the EC2 instance.

## 3. EC2 User Data

- **EC2 User Data** is a script or set of commands used to configure an instance when it first launches.
- It is commonly used for bootstrap tasks, such as installing software, updating packages, or starting an application.
- On Linux instances, User Data commands typically run with root privileges.
- User Data is useful for automating the initial setup instead of configuring every instance manually.

## 4. EC2 Instance Types

Instance types provide different balances of CPU, memory, storage, and networking resources. Choose a type based on the workload.

- **General Purpose:**
  - Suitable for a wide range of workloads, such as web servers and code repositories.
  - Provides a balance of compute, memory, and networking resources.
- **Compute Optimized:**
  - Suitable for compute-intensive workloads.
  - Designed for tasks that need high-performance processors.
- **Memory Optimized:**
  - Suitable for workloads that process large datasets in memory.
  - Provides high memory capacity and performance.
- **Storage Optimized:**
  - Suitable for workloads that need fast access to large datasets stored locally.

## 5. Security Groups - EC2 Firewall

- A **Security Group** acts as a virtual firewall for an EC2 instance.
- It controls which inbound and outbound network traffic is allowed.
- Security Groups contain **allow rules only**; they do not have explicit deny rules.
- A Security Group can be associated with multiple EC2 instances.
- Security Groups are configured outside the operating system of the EC2 instance and are applied to the instance's network interface.
- Security Groups are **stateful**: response traffic for an allowed connection is automatically allowed.
- For a newly created Security Group, the default rules are:
  - **Inbound:** All traffic is blocked until you add an allow rule.
  - **Outbound:** All outbound traffic is allowed by default.

### Connection troubleshooting hints

- **Connection timed out:** Check the Security Group inbound rules first. Also check the source IP, route/network configuration, and whether the instance is ready.
- **Connection refused:** The instance may be reachable, but the application or service may not be running or listening on the expected port. Check the application and operating-system firewall too.

## 6. Common Network Ports

| Port | Protocol / Service | Common purpose |
|---|---|---|
| 21 | FTP | File Transfer Protocol; used for file transfers. |
| 22 | SSH | Secure command-line access to Linux instances. |
| 22 | SFTP | Secure file transfer over SSH. |
| 80 | HTTP | Access to a website without HTTPS encryption. |
| 443 | HTTPS | Access to a website using encrypted HTTPS. |
| 3389 | RDP | Remote Desktop access to Windows instances. |

**Remember:** SFTP uses SSH and normally operates over port 22. FTP commonly uses port 21.

## 7. Connecting to EC2 Instances

### SSH

- **SSH (Secure Shell)** allows you to connect to and control a remote Linux machine using a command line.
- macOS and Linux include an SSH client in the terminal.
- Windows 10 and later generally include the OpenSSH client.
- PuTTY is another SSH client commonly used on Windows.

### EC2 Instance Connect

- **EC2 Instance Connect** provides a way to connect to supported Linux EC2 instances using the AWS Console, AWS CLI, or an SSH client.
- Avoid storing long-term AWS access keys directly on an EC2 instance connect.
- Never enter Access key ID into EC2 Instance connect, because who ever logs in with your account can retrieve the access key Ids. Instead use IAM Roles (for this at first our EC2 Instance should have policy 'IAMReadOnlyAccess')
- Follow the **principle of least privilege**: give the role only the permissions the application needs.

---

*Personal study notes for AWS Certified Solutions Architect - Associate (SAA-C03).*
