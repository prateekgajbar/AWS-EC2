
#  What is Auto Scaling?

Auto Scaling automatically adds or removes EC2 instances based on traffic or demand. It helps applications adjust capacity without manual work.

In simple words:
 → When traffic increases → launch more servers
 → When traffic decreases → remove extra servers

---

#  Benefits of Auto Scaling

### **1. Improved Performance**

* Keeps your app running smoothly even during traffic spikes.

### **2. Cost Savings**

* You don’t pay for unused servers during low traffic periods.

### **3. High Availability**

* Helps your app stay available by replacing unhealthy instances.

---

#  Types of Scaling

## 1️ Horizontal Scaling

* Also called "scale-out".
* Adds more machines/instances.
* Good for distributing load.

Example: Instead of 1 big server, use 3 smaller servers.

---

## 2️ Vertical Scaling

* Also called "scale-up".
* Increase power of the same machine (more CPU, RAM, etc.).

Example: Upgrading from t2.micro → t3.medium.

---

## 3️ Manual Scaling

* You manually increase or decrease the number of EC2 instances.
* Done through AWS Console or CLI.

---

## 4️ Dynamic Scaling

* Fully automatic scaling based on CloudWatch alarms.
* Best for unpredictable or changing workloads.
* Example: Scale out when CPU > 70%.

---

# A) Create Auto Scaling Group (ASG) Using AWS Console (GUI)

##  Step 1: Create a Launch Template

This template defines how your EC2 instances will be launched.

1. Go to **EC2 Console**
2. Click **Launch Templates** on left side
3. Click **Create launch template**
4. Fill details:

   * Name: `my-asg-template`
   * AMI: Select your OS
   * Instance type: t2.micro (example)
   * Key pair: Select existing
   * Security Group: Allow SSH/HTTP as per your need
5. Click **Create template**

---

##  Step 2: Create an Auto Scaling Group

1. Go to **Auto Scaling Groups** (left menu)
2. Click **Create Auto Scaling group**
3. Select your **launch template**
4. Enter a name: `my-auto-scaling-group`
5. Choose **VPC** and **Subnets** (at least 2 for high availability)

---

##  Step 3: Configure Group Size

Set the scaling limits:

* **Desired capacity**: 1
* **Minimum capacity**: 1
* **Maximum capacity**: 3

This means:

* ASG must have at least 1 instance
* Can scale up to 3 instances when needed

---

##  Step 4: Add Scaling Policies

Choose **Target Tracking Policy** (easy)

* Example: Maintain CPU at **50%**
* If CPU goes above 50% → scale out
* If CPU goes below 50% → scale in

---

##  Step 5: Add Notifications (Optional)

You can connect SNS to get alerts when scaling happens.

---

##  Step 6: Review & Create

Check all settings → Click **Create Auto Scaling group**.

---

# B) Implementation Auto Scaling with a Load Balancer (GUI)

When you use Auto Scaling, you almost always want to put a Load Balancer in front of the ASG to distribute traffic and perform health checks. Below are the GUI steps I follow when I implement Auto Scaling with an Application Load Balancer (ALB).

##  Step A: Create a Target Group (for ALB)

1. Go to **EC2 Console → Target Groups**.
2. Click **Create target group**.
3. Choose target type: **Instance** (or IP/Lambda).
4. Protocol: **HTTP**, Port: **80** (adjust to your app).
5. Set health check path (e.g., `/health` or `/`).
6. Create the target group (do not register targets here if you plan to attach via ASG).

##  Step B: Create (or Use Existing) Application Load Balancer

1. Go to **EC2 Console → Load Balancers → Create Load Balancer → Application Load Balancer**.
2. Fill name, scheme (Internet-facing/internal), IP type.
3. Select at least two subnets (different AZs).
4. Create listeners (HTTP 80 / HTTPS 443) and for the default action choose the **Target Group** you created.
5. Finish creating ALB.

##  Step C: Attach Load Balancer to the Auto Scaling Group

You can attach the ALB during ASG creation or attach an ALB to an existing ASG.

**During ASG creation:**

* In the Auto Scaling Group wizard, when you reach the **Load balancing** section, choose **Attach to an existing load balancer** and then select **Application Load Balancer** and the target group you created.

**Attach to existing ASG:**

* Go to **Auto Scaling Groups → Select your ASG → Actions → Edit**
* In the **Load balancing** section, add the target group.

##  Step D: Configure Health Checks and Grace Period

* Set **Health check type** to **ELB** in the ASG so the group uses the load balancer health checks.
* Set **Health check grace period** (for example, 300 seconds) to allow instances time to start before being marked unhealthy.
* Set **Deregistration delay** on ALB target group (default 300s) so in-flight requests are handled before instance termination.

##  Step E: Scaling Policies Using ALB Metrics (Optional)

* In the ASG, use **Target tracking scaling policy** based on **ALB Request count per target** or **Average CPU**.
* Example: target request count per target = 1000 → scale out when requests per instance are higher.

##  Step F: Test the Setup

1. Generate traffic (curl, load testing, or open app in browser).
2. Check EC2 instances under the ASG; see instances added/removed as traffic changes.
3. Check ALB Target Group to ensure targets show **healthy**.
4. Verify that traffic is evenly distributed across instances.
---
