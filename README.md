


# 🚀 Project Overview: AWS Infrastructure (ALB Path-Based Routing & NAT Gateway)

## 📌 1. Introduction

This project implements a secure, highly available, and scalable web application architecture deployed within the **AWS Cloud (Mumbai Region: `ap-south-1`)**. The primary goal is to intelligently manage incoming web traffic using path-based routing while safeguarding backend servers (EC2 instances) from direct exposure to the public internet.

---

## 🏗️ 2. Architectural Components & Design


<img width="1408" height="768" alt="image_8317480b" src="https://github.com/user-attachments/assets/8a519c92-2c13-4dbb-aed5-6b0ba02e53ab" />


* **Internet Gateway (IGW):**
* Acts as the bridge between your VPC and the public internet.


* Handles all incoming user requests (**Inbound Traffic**) and application responses (**Outbound Internet Traffic**).




* **Application Load Balancer (ALB):**
* Deployed in a **Public Subnet** to serve as the single entry point for all incoming user traffic.


* Utilizes **Path-Based Routing** rules to inspect incoming URL paths and forward requests to corresponding Target Groups.




* **Path-Based Routing Rules:**
* **Rule 1 (`/student/*`):** Routes traffic destined for the student portal to the **TG-Student** target group and its respective backend instance.


* **Rule 2 (`/teacher/*`):** Routes traffic destined for the teacher portal to the **TG-Teacher** target group.


* **Rule 3 (`/college/*`):** Routes traffic destined for the college portal to the **TG-College** target group.




* **Private Subnets & Backend EC2 Instances:**
* To maximize security, all backend compute workloads are isolated within **Private Subnets** across multiple Availability Zones (**AZ A, B, and C**), preventing direct public access:


* **Student Instance:** Hosted in Availability Zone A.


* **Teacher Instance:** Hosted in Availability Zone B.


* **College Instance:** Hosted in Availability Zone C.






* **NAT Gateway:**
* Since private instances do not have inbound public IP addresses, they require a **NAT Gateway** to safely initiate outbound connections to the internet.


* Enables private instances to download software patches, updates, or query external APIs without exposing them to inbound threats.





---

## 🔄 3. Complete Traffic Flow Lifecycle

1. **User Request:** A user initiates an HTTP/HTTPS request from the internet.
2. **Entry Point:** The request passes through the **Internet Gateway (IGW)** and hits the **Application Load Balancer (ALB)**.


3. **Routing Decision:** The ALB evaluates the URL path (`/student/`, `/teacher/`, or `/college/`) against configured rules.


4. **Backend Processing:** The request is forwarded via the appropriate Target Group to the matching **Private EC2 Instance** in its respective Availability Zone.


5. **Outbound Communication:** If a private instance needs to communicate with the outside world, the traffic flows securely through the **NAT Gateway** and out via the **Internet Gateway**.



---

## 🛡️ 4. Security & Best Practices

* **Network Isolation:** Core application logic and data layers remain fully hidden behind private subnets.


* **High Availability & Fault Tolerance:** Spreading resources across multiple Availability Zones (**AZ A, AZ-B, AZ-C**) ensures continuous uptime even if one zone fails.

 Here are the **Expected Output & Verification Points** in English, ready to be added to your GitHub repository documentation (`README.md`):

---

# 🎯 Expected Output & Verification Points

* **1. Successful Public Access to the Application:**
* When a user sends a request to the ALB's DNS name or domain, the application loads smoothly without errors, as the Internet Gateway and ALB correctly accept and handle all incoming traffic.




* **2. Dynamic Path-Based Routing (URL Split Behavior):**
* **Student Portal:** Accessing `/student/` in the URL (`http://<alb-dns>/student/...`) routes the request exclusively to the **Student EC2 Instance** via the `TG-Student` target group.


* **Teacher Portal:** Accessing `/teacher/` routes the request exclusively to the **Teacher EC2 Instance** via the `TG-Teacher` target group.


* **College Portal:** Accessing `/college/` routes the request exclusively to the **College EC2 Instance** via the `TG-College` target group.




* **3. Secure Private Instances (No Direct Public IP):**
* All three backend EC2 instances (Student, Teacher, and College) operate within **Private Subnets** without public IP addresses, preventing any direct access from the public internet and maintaining strict infrastructure security.




* **4. Successful Outbound Internet via NAT Gateway:**
* When private instances require software updates, security patches, or external API calls, outgoing packets are successfully routed through the **NAT Gateway** and out via the **Internet Gateway**.




* **5. High Availability Across Availability Zones:**
* By distributing workloads across multiple Availability Zones (**Availability Zone A, B, and C**), the architecture ensures high availability and fault tolerance, meaning failure in one zone will not bring down the entire application.
