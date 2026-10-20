AWS ALB Path-Based Routing with Private EC2 Instances and NAT Gateway

📌 Project Overview

This project demonstrates how to deploy three separate web applications on AWS using an Application Load Balancer (ALB), path-based routing, private EC2 instances, and a NAT Gateway.

The infrastructure is deployed in the AWS Mumbai Region ("ap-south-1") using one VPC, 2 Public Subnets, 3 Private Subnets, and 3 Availability Zones.

The ALB routes incoming HTTP requests to the appropriate application based on the URL path. Each application runs on a separate private EC2 instance with Nginx. A NAT Gateway provides outbound internet connectivity to private instances without exposing them directly to the internet.

🏗️ Architecture Diagram
<img width="1000" height="558" alt="shared_image" src="https://github.com/user-attachments/assets/313b70c0-09e8-4802-b72c-bbbc02be3fdd" />



"AWS ALB Path-Based Routing Architecture" (aws-architecture.png)

Architecture Components

- Amazon VPC: Provides an isolated network for the infrastructure.
- 2 Public Subnets: Host the internet-facing ALB across two Availability Zones; one also hosts the NAT Gateway.
- 3 Private Subnets: Host the Student, Teacher, and College EC2 instances separately.
- 3 Availability Zones: Distribute the private application instances across "ap-south-1a", "ap-south-1b", and "ap-south-1c".
- Internet Gateway (IGW): Enables internet connectivity for public-facing resources.
- Application Load Balancer (ALB): Receives HTTP requests on port 80 and forwards them according to URL paths.
- 3 Target Groups: Route requests to the corresponding EC2 instances.
- NAT Gateway: Provides outbound internet access to private instances.
- Security Groups and Route Tables: Control network access and traffic routing.

🔀 Path-Based Routing

URL Path| Target Group| EC2 Instance| Application
"/student/*"| TG-Student| student-server| Student Portal
"/teacher/*"| TG-Teacher| teacher-server| Teacher Portal
"/college/*"| TG-College| college-server| College Portal

🌐 Application Pages

- Student: Welcome to Student Portal
- Teacher: Welcome to Teacher Portal
- College: Welcome to College Portal

All three applications are accessed using the same ALB DNS name with different URL paths.

🔐 Security Configuration

- Private EC2 instances do not have public IP addresses.
- The ALB security group allows HTTP traffic on port 80.
- EC2 security groups allow HTTP traffic only from the ALB security group.
- Public route tables direct internet traffic through the Internet Gateway.
- Private route tables use the NAT Gateway for outbound internet access.

🛠️ AWS Services Used

Amazon EC2 | Amazon VPC | Application Load Balancer | Target Groups | NAT Gateway | Internet Gateway | Route Tables | Security Groups | Nginx | Linux

🎯 Learning Outcomes

- Configuring public and private subnets.
- Implementing ALB path-based routing.
- Creating and configuring target groups.
- Deploying Nginx websites on private EC2 instances.
- Configuring NAT Gateway and route tables.
- Understanding AWS network security.

✅ Project Result

Successfully configured a single Application Load Balancer to route requests to three separate web applications hosted on private EC2 instances. The design separates public access from private application servers and provides outbound connectivity through a NAT Gateway.

👨‍💻 Project Type

Hands-on AWS Cloud Networking and Load Balancing Project.
