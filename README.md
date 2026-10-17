# project3
Is AWS Infrastructure Project ka poora professional overview, structured aur step-by-step points me niche diya gaya hai, jisse aap apni repository ya documentation me direct use kar sakte hain:

---

# 🚀 Project Overview: AWS Architecture (ALB Path-Based Routing & NAT Gateway)

## 📌 1. Introduction

Yeh project **AWS Cloud (Mumbai Region: `ap-south-1`)** ke andar ek highly available, secure aur scalable web application architecture ko implement karta hai. Iska mukhya uddeshya incoming web traffic ko intelligent tareeqe se manage karna aur backend servers (EC2 instances) ko internet ke direct khatron se bachana hai.

---

## 🏗️ 2. Architectural Components & Design
<img width="1408" height="768" alt="image_8317480b" src="https://github.com/user-attachments/assets/3c46a86a-cd54-4551-8ee5-632fb8214c69" />




* **Internet Gateway (IGW):**
* Yeh VPC aur public internet ke beech ka gateway hai.


* User ki taraf se aane wala **Inbound Traffic** yahin se enter hota hai aur application ka **Outbound Traffic** yahin se bahar jata hai.




* **Application Load Balancer (ALB):**
* Yeh **Public Subnet** me deploy hota hai aur saare incoming requests ko pehle receive karta hai.


* Yeh **Path-Based Routing** ka use karke URLs ke aadhar par traffic ko alag-alag Target Groups aur instances par forward karta hai.




* **Path-Based Routing Rules:**
* **Rule 1 (`/student/*`):** Agar URL me `/student/` path milta hai, toh traffic **TG-Student** target group ke zariye **Student Instance** par jata hai.


* **Rule 2 (`/teacher/*`):** Agar URL me `/teacher/` path milta hai, toh traffic **TG-Teacher** target group ke zariye **Teacher Instance** par jata hai.


* **Rule 3 (`/college/*`):** Agar URL me `/college/` path milta hai, toh traffic **TG-College** target group ke zariye **College Instance** par jata hai.




* **Private Subnets & Backend EC2 Instances:**
* Security ko dhyan me rakhte hue saare backend servers ko **Private Subnets** me alag-alag Availability Zones me rakha gaya hai taaki in par direct public access na ho:


* **Student Instance:** Availability Zone A me sthit hai.


* **Teacher Instance:** Availability Zone B me sthit hai.


* **College Instance:** Availability Zone C me sthit hai.






* **NAT Gateway:**
* Kyunki private instances ke paas direct internet access nahi hota, isliye unhein software updates, patches, ya external APIs access karne ke liye **NAT Gateway** ki zarurat hoti hai.


* Yeh private instances ko secure outbound internet connection provide karta hai.





---

## 🔄 3. Complete Traffic Flow Lifecycle

1. **User Request:** User internet ke through application ko access karne ke liye request bhejta hai.


2. **Entry Point:** Request **Internet Gateway (IGW)** ko cross karte hue **Application Load Balancer (ALB)** par pahunchti hai.


3. **Routing Decision:** ALB URL path (`/student/`, `/teacher/`, ya `/college/`) ko analyze karta hai.


4. **Backend Processing:** Path ke match hone par, ALB uss specific Target Group ke through corresponding **Private EC2 Instance** ko request bhejta hai.


5. **Outbound Communication:** Agar private instance ko internet par koi data bhejna ho, toh request **NAT Gateway** aur **Internet Gateway** ke raste bahar jati hai.



---

## 🛡️ 4. Security & Best Practices

* **Isolation:** Database aur backend servers ko private subnets me rakhakar unhein secure banaya gaya hai.


* **Load Distribution:** Multiple Availability Zones (AZ-A, AZ-B, AZ-C) ka use karne se high availability aur fault tolerance milti hai.


* **Controlled Access:** ALB single public entry point ke roop me kaam karta hai, jisse direct server attacks ka khatra kam ho jata hai.
