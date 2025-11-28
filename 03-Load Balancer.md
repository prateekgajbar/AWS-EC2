
#  Elastic Load Balancer (ELB)

An **Elastic Load Balancer** helps distribute incoming traffic across multiple servers so that no single server gets overloaded. It also checks which servers are healthy and sends traffic only to those.

###  What ELB Does

#### **1. Manages Traffic Smartly**

* It spreads incoming requests across multiple servers.
* Helps maintain performance and avoids server overload.

#### **2. Performs Health Checks**

* ELB keeps monitoring targets.
* If any server becomes unhealthy, ELB temporarily stops sending traffic to it.

#### **3. Supports Scaling**

* You can add or remove servers anytime without stopping the application.
* Makes your application more stable during high traffic.

---

##  Target Group

A **Target Group** is the place where ELB actually sends the traffic. It contains the list of servers (instances, IPs, or Lambdas) that will receive requests.

### Target Groups Handle:

* Health checks
* Protocols and ports
* Routing rules

 Load Balancer → Target Group → Healthy Targets

---

#  Types of AWS Load Balancers

Below are the main types of load balancers offered by AWS, including the **Classic Load Balancer**, which is now considered legacy but still appears in older architectures.
AWS provides multiple types of load balancers depending on the kind of traffic you want to handle.

## 1️ Classic Load Balancer (CLB)

* Operates at **Layer 4 (Transport)** and partially at **Layer 7 (Application)**.
* This is AWS’s oldest load balancer.
* Supports basic **HTTP/HTTPS** and **TCP** load balancing.
* Limited features compared to ALB & NLB.
* Does not support advanced routing like host-based or path-based routing.

**Use Cases:** Legacy applications, simple EC2-based architectures.

---

## 2️ Application Load Balancer (ALB) (ALB)

* Works at **Layer 7** of the OSI model.
* Used mainly for **HTTP/HTTPS** traffic.
* Can route traffic based on **URL path, domain name, headers, and query strings**.

**Common Uses:** Web apps, microservices, container apps.

---

## 2️ Network Load Balancer (NLB)

* Works at **Layer 4**.
* Supports **TCP, UDP, TLS**.
* Designed for **very high traffic** with extremely low latency.
* Provides static IPs per AZ.

**Common Uses:** Real-time applications like gaming, finance systems, or VoIP.

---

## 3️ Gateway Load Balancer (GLB)

* Works at **Layer 3 and 4**.
* Mainly used for deploying and scaling third‑party appliances such as firewalls or intrusion detection systems.
* Uses GENEVE protocol to forward traffic.

**Common Uses:** Security and inspection appliances.

---

#  AWS Global Accelerator

AWS Global Accelerator improves the performance of global applications by routing traffic through the AWS global network instead of the public internet.

###  Benefits of Global Accelerator

* Reduces latency using AWS’s private network
* Provides **2 static anycast IPs**
* Shifts traffic automatically to the nearest healthy region
* Supports both TCP and UDP

---

##  How Global Accelerator Works (Simple Explanation)

1. AWS gives two static IPs.
2. Users connect to these IPs.
3. Traffic enters the nearest AWS edge location.
4. AWS routes it through the global backbone network.
5. It reaches the closest healthy application endpoint (ALB, NLB, EC2, etc.).

### Example:

If the Mumbai region goes down, traffic automatically switches to Singapore without user interruption.

---

#  Comparison: ELB vs Global Accelerator

| Feature             | Elastic Load Balancer            | Global Accelerator         |
| ------------------- | -------------------------------- | -------------------------- |
| Purpose             | Distribute traffic within region | Improve global performance |
| Layer               | L4/L7                            | AWS Edge Network           |
| Latency Improvement | No                               | Yes                        |
| Static IP           | No                               | Yes                        |
| Failover            | AZ-level                         | Region-level               |

---

#  Architecture (Simple Text Format)

### ALB Flow

```
Client → ALB → Target Group → EC2 Instances
```

### Global Accelerator Flow

```
Client → AWS Edge Location → Global Accelerator → ALB / NLB → Target Group → EC2
```

---

#  Creating a Load Balancer Using AWS Console (GUI)

Below are the easy, step-by-step instructions to create an **Application Load Balancer (ALB)** using the AWS Management Console.

##  Step 1: Open EC2 Dashboard

* Log in to AWS Console
* Go to **Services → EC2**
* On the left menu, click **Load Balancing → Load Balancers**
* Click **Create Load Balancer**

##  Step 2: Select Load Balancer Type

Choose from:

* **Application Load Balancer (ALB)** – for HTTP/HTTPS
* **Network Load Balancer (NLB)** – for TCP/UDP/TLS
* **Gateway Load Balancer (GLB)** – for firewalls & inspection tools

For this example, select **Application Load Balancer**.

##  Step 3: Configure Basic Settings

Provide:

* **Name** (example: my-app-alb)
* **Scheme** (Internet-facing / Internal)
* **IP type** (IPv4 or Dual-stack)

Select at least **two Availability Zones** to ensure high availability.

##  Step 4: Configure Security Settings

* Choose **HTTP (80)** or **HTTPS (443)**
* If using HTTPS, upload or choose an **SSL Certificate** from ACM.

##  Step 5: Create or Select Security Group

* Choose an existing security group OR create a new one
* Allow inbound traffic for **HTTP/HTTPS** depending on your app

##  Step 6: Create a Target Group

* Target type: **Instance / IP / Lambda**
* Protocol and Port (e.g., HTTP:80)
* Health check path (e.g., `/`)

Click **Create Target Group**.

##  Step 7: Register Targets

* Choose EC2 instances
* Add them to the target group

Click **Include as Pending** → **Register** → **Next**

##  Step 8: Review & Create

Review all settings:

* Load Balancer Name
* Subnets
* Listeners
* Target Groups
* Security Group

Click **Create Load Balancer**.

---

