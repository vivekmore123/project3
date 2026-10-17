


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

  Is project ke execution ke baad aapko kya **Expected Output** (ya Result) milega, uske points niche diye gaye hain. Yeh points aap apne GitHub repository ke `README.md` me bhi add kar sakte hain:

---

# 🎯 Expected Output & Verification Points

* **1. Successful Public Access to the Application:**
* Jab user ALB ke DNS name ya domain par request bhejega, toh application bina kisi error ke load hogi, kyunki Internet Gateway aur ALB saare incoming traffic ko correctly accept kar rahe hain.


* **2. Dynamic Path-Based Routing (URL Split Behavior):**
* **Student Portal:** Agar user URL me `/student/` access karega (`http://<alb-dns>/student/...`), toh response sirf **Student EC2 Instance** se aayega.
* **Teacher Portal:** Agar user `/teacher/` access karega, toh response sirf **Teacher EC2 Instance** se aayega.
* **College Portal:** Agar user `/college/` access karega, toh response sirf **College EC2 Instance** se aayega.


* **3. Secure Private Instances (No Direct Public IP):**
* Backend ke teeno EC2 instances (Student, Teacher, College) ke paas public IP address nahi hoga. Yeh internet se directly access nahi ho sakenge, jisse security puri tarah maintain rahegi.


* **4. Successful Outbound Internet via NAT Gateway:**
* Jab private instances par login karke aap koi software update ya `apt-get update` / `yum update` run karenge, toh woh packets **NAT Gateway** ke through successfully internet par jayenge aur updates download ho jayenge.


* **5. High Availability Across Zones:**
* Agar koi ek Availability Zone (jaise AZ-A) down bhi ho jata hai, tab bhi baaki zones (AZ-B aur AZ-C) ke instances apna kaam karte rahenge aur system me downtime nahi aayega.


* **Controlled Access:** Centralized traffic entry via the ALB minimizes the attack surface and protects backend EC2 instances.
