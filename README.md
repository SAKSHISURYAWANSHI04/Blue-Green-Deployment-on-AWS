# 🚀 Blue-Green Deployment on AWS



## 📌 Project Overview

This project shows how to deploy an application on AWS **without any downtime** using the **Blue-Green Deployment** method.


---

![Architecture](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/3c13c7a6855c8d19db913ff6d6d4e6d797c367e6/aws-blue-green-architecture.png)
🔵 Blue Environment:
- This is the current live application
- All user traffic is handled here
- It is stable and running version

🟢 Green Environment:
- This is the new version of the application
- Created for testing before going live
- Not receiving traffic initially

⚖️ Application Load Balancer (ALB):
- Acts as a traffic controller
- Receives all user requests
- Routes traffic to Blue or Green environment

📈 Auto Scaling Group (ASG):
- Maintains required number of EC2 instances
- Ensures high availability
- Used in both Blue and Green environments

🖥️ EC2 Instances:
- Virtual servers running the application (Nginx)
- Present inside both environments

🔄 Traffic Flow:
- Users → ALB → Blue (initially)
- After deployment → ALB → Green
------


## 🔵 Blue Environment Setup

### 🥇 Step 1: Create Launch Template

![Launch Template](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/3c13c7a6855c8d19db913ff6d6d4e6d797c367e6/Blue%20Launch%20Template%20created.png)

**Details:**

* Launch Template is used to define EC2 configuration
* It includes:

  * AMI (Ubuntu)
  * Instance type (t3.micro)
  * Security Group (HTTP + SSH)
  * User data (install Nginx)

---

### 🥈 Step 2: Create Target Group

![Blue Target Group](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/3c13c7a6855c8d19db913ff6d6d4e6d797c367e6/Blue%20TG%20created.png)

**Details:**

* Target Group connects EC2 instances to Load Balancer
* It checks health of instances
* Protocol: HTTP (Port 80)

---

### 🥉 Step 3: Create Auto Scaling Group (ASG)

![Blue ASG](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/main/Blue%20ASG%20created.png)

**Details:**

* ASG launches EC2 instances using Launch Template
* Maintains desired number of instances
* Attached to Target Group
* Ensures high availability

---

### 🔄 Flow

Users → ALB → Target Group → Blue ASG → EC2 Instances

---

### 🟢 Green Environment Setup

### 🥇 Step 1: Create Launch Template (Green)

![Green Launch Template](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/main/Blue%20Launch%20Template%20created.png)

**Details:**

* Similar to Blue, but used for new version
* Can update user data (new app version)
* Defines EC2 configuration

---

### 🥈 Step 2: Create Target Group (Green)

![Green Target Group](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/3c13c7a6855c8d19db913ff6d6d4e6d797c367e6/Blue%20TG%20created.png)

**Details:**

* New target group for Green environment
* Registers Green EC2 instances
* Health checks ensure instances are working

---

### 🥉 Step 3: Create Auto Scaling Group (Green)

![Green ASG](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/main/Blue%20ASG%20created.png)

**Details:**

* Launches EC2 instances for Green environment
* Uses Green Launch Template
* Attached to Green Target Group
* Not receiving traffic initially

---

### 🧪 Step 4: Test Green Environment

![Green Testing](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/main/Green%20verified%20directly%20on%20EC2%20IP.png)

**Details:**

* Test application using Green Target Group URL
* Verify new version is working correctly
* Ensure no errors before switching traffic

---

### 🔄 Flow

Users → ALB → (still Blue)
Green → Ready but not live

---

### 🌐 Blue Environment Live (Browser Output)

![Blue Live Output](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/main/Blue%20verified%20directly%20on%20EC2%20IP.png)

**Explanation:**

* This shows the Blue environment running in the browser
* Application is live and accessible using ALB DNS
* Users are currently connected to Blue environment

---

## 🧠 What This Means

* 🔵 Blue = Active production environment
* 🌐 Website is working normally
* ⚖️ ALB is routing traffic to Blue instances
* 🖥️ EC2 instances are serving the application

---

## 🔄 Flow

Users → ALB → Blue Target Group → Blue ASG → EC2 → Browser Output

---

## 🎯 Important Point

* This confirms your application is **live before deployment**
* Green environment will be created without disturbing this
---

### 🟢 Full Green Deployment (100% Traffic on Green)

![Full Green Traffic](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/main/Full%20Green%20—%20100%25%20traffic%20on%20Green.png)

**Explanation:**

* All user traffic is now routed to Green environment
* Green becomes the new live application
* Blue environment is no longer serving traffic

---

## 🧠 What This Means

* 🟢 Green = Active production environment
* 🌐 Users are accessing the new version
* ⚖️ ALB is routing 100% traffic to Green
* 🔵 Blue is kept as backup

---

## 🔄 Flow

Users → ALB → Green Target Group → Green ASG → EC2 → Browser

---

## 🎯 Important Point

* Deployment completed successfully
* Zero downtime achieved
* Easy rollback possible (switch back to Blue)

---

### 🟡 Canary Deployment (80% Blue / 20% Green)

![Canary Traffic Split](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/main/Canary%20—%2080/20%20split%20both%20responding.png)

**Explanation:**

* Traffic is split between Blue and Green environments
* 80% traffic goes to Blue (stable version)
* 20% traffic goes to Green (new version)

---

## 🧠 What This Means

* 🔵 Blue = Still main live environment
* 🟢 Green = Partially live (testing with real users)
* ⚖️ ALB distributes traffic based on weights
* 🧪 Used to test new version safely

---

## 🔄 Flow

80% Users → ALB → Blue Target Group
20% Users → ALB → Green Target Group

---

## 🎯 Important Point

* Gradual deployment reduces risk
* Issues can be detected early
* Easy rollback (send 100% back to Blue)

---

### 🔄 Rollback (Blue Live Again)

![Rollback to Blue](https://raw.githubusercontent.com/SAKSHISURYAWANSHI04/Blue-Green-Deployment-on-AWS/main/Rollback%20—%20Blue%20live%20again.png)

**Explanation:**

* Traffic is switched back from Green to Blue
* Blue becomes the live environment again
* Used when issues are found in Green

---

## 🧠 What This Means

* 🔵 Blue = Active production environment again
* 🟢 Green = Stopped or under fix
* ⚖️ ALB routes 100% traffic back to Blue
* 🚨 Quick recovery from failure

---

## 🔄 Flow

Users → ALB → Blue Target Group → Blue ASG → EC2 → Browser

---

## 🎯 Important Point

* Rollback is instant (few seconds)
* No downtime during rollback
* Ensures system stability

---
✅ Project Completed Successfully.
