---

# Hosting an HTML Website on AWS with a Three-Tier VPC Architecture

## Project Overview

This project demonstrates setting up a highly available and scalable HTML web application on Amazon Web Services (AWS). It uses a multi-tier architecture to ensure fault tolerance, security, and scalability. The web application is served through two Amazon EC2 instances managed by an Auto Scaling group, ensuring the website remains available even under varying load conditions.

## Architecture Overview

### Components
- **EC2 Instances**: Hosts the web servers in private subnets to enhance security.
- **Application Load Balancer (ALB)**: Distributes incoming traffic across the two EC2 instances in different Availability Zones, ensuring high availability.
- **Auto Scaling Group**: Automatically adjusts the number of EC2 instances between 2 and 3 based on traffic, ensuring efficient resource utilization.
- **VPC Setup**: Configured with public and private subnets in two Availability Zones for fault tolerance and secure network isolation.
- **Security Groups**: Acts as a virtual firewall for EC2 instances to control inbound and outbound traffic.
- **IAM Roles**: Ensures secure service interaction with AWS resources.
- **Certificate Manager**: Manages SSL/TLS certificates, enabling HTTPS for secure communication.
- **Route 53**: Manages DNS for the application, ensuring the domain name resolves correctly to the ALB.
- **GitHub**: Stores web files and scripts, facilitating version control and collaboration.

### Diagram
![Alt-text](https://github.com/SFrank80/Techmax-Project/blob/a0e1e0e895a8d9933428c784d7e499dfbd9624a5/Host%20an%20HTML%20Website%20on%20Three%20Tier%20VPC.png)

## Deployment Process

1. **Launch Template Setup**: Before initiating the Auto Scaling group, a launch template was created to define the configuration of the EC2 instances. This template specifies the instance type, AMI ID, key pair, security groups, and associated block storage.

2. **User Data Script**: Automates the setup of EC2 instances. The script performs the following tasks:
   - Installs and configures Apache HTTP Server.
   - Downloads and deploys the web application from a GitHub repository.
   - Configures the server to start on system boot and immediately starts serving the deployed web application.

### User Data Script
```bash
#!/bin/bash

# Gain full administrative privileges
sudo su

# Update all installed packages
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Set the Apache document root as the working directory
cd /var/www/html

# Download and unpack the project source from GitHub
wget https://github.com/SFrank80/Techmax-Project/archive/refs/heads/main.zip
unzip main.zip
cp -r Techmax-Project-main/* .

# Clean up installation files
rm -rf Techmax-Project-main main.zip

# Configure Apache to start on boot and run it
systemctl enable httpd
systemctl start httpd
```

## Security Measures
- **HTTPS**: Enabled by default using AWS Certificate Manager, ensuring all data transmitted between the client and server is encrypted.
- **Security Groups**: Strictly configured to only allow traffic on necessary ports.
- **IAM**: Minimal permissions principle is applied to ensure services operate under the least privileges necessary to perform their tasks.

## Monitoring and Notifications
- **CloudWatch**: Monitors EC2 instance metrics and ALB performance.
- **SNS Notifications**: Configured to alert on significant Auto Scaling events or operational issues.

## Conclusion

This project effectively demonstrates how to architect and deploy a secure, scalable, and fault-tolerant web application on AWS. The use of AWS services like EC2, ALB, Auto Scaling, and Route 53 highlights the robustness of AWS for enterprise-level applications.

---
