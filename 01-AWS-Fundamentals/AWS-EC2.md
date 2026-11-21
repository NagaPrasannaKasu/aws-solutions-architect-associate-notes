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

Your rough notes mention seven instance-type categories, but they describe these four:

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

> **Note:** The source notes refer to seven categories but only list four. The other categories have not been added here so the notes stay aligned with your original material.

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

> These are troubleshooting clues, not guaranteed diagnoses. A connection problem can have more than one cause.

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
- When using the Console or CLI connection method, EC2 Instance Connect pushes a temporary SSH public key to the instance. The key is available for 60 seconds for connection setup.
- The IAM identity making the connection needs the required EC2 Instance Connect permissions.

## 8. IAM Roles and EC2 - Important Security Point

- Avoid storing long-term AWS access keys directly on an EC2 instance.
- When an application running on EC2 needs to access another AWS service, attach an **IAM role** to the EC2 instance.
- The role provides temporary credentials to the application, instead of requiring you to store access keys in a file or in the code.
- Follow the **principle of least privilege**: give the role only the permissions the application needs.

### Important clarification about IAMReadOnlyAccess

- `IAMReadOnlyAccess` is **not a general requirement** for using EC2 Instance Connect or for attaching an IAM role to an EC2 instance.
- EC2 Instance Connect requires the connecting IAM identity to have the relevant connection permissions.
- An IAM role attached to an EC2 instance should contain permissions required by the application running on that instance. Use `IAMReadOnlyAccess` only when the application genuinely needs read-only access to IAM information; do not attach it automatically.

## 9. Quick Revision - AWS SAA-C03

- **EC2:** Virtual machines in AWS; an IaaS service.
- **EBS and EFS:** Network-attached storage options.
- **Instance Store:** Storage on the physical host hardware.
- **ELB:** Distributes incoming traffic across targets.
- **ASG:** Adjusts the number of EC2 instances.
- **User Data:** Runs startup/bootstrap commands when an instance launches.
- **Security Group:** Stateful virtual firewall with allow rules only.
- **Port 22:** SSH and SFTP.
- **Port 21:** FTP.
- **Port 80:** HTTP.
- **Port 443:** HTTPS.
- **Port 3389:** RDP.
- **IAM Role for EC2:** Gives applications on an instance temporary AWS credentials.
- **EC2 Instance Connect:** Uses IAM permissions for the connection and a temporary SSH public key; it does not require an IAMReadOnlyAccess role on the instance.

## 10. Official AWS References

- [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
- [Connect to your EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect.html)
- [Connect to a Linux instance using EC2 Instance Connect](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-connect-methods.html)
- [Attach an IAM role to an EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/attach-iam-role.html)
- [Amazon EC2 security groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html)

---

*Personal study notes for AWS Certified Solutions Architect - Associate (SAA-C03).*
