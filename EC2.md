

  Amazon EC2 

---

#  Elastic Compute Cloud (EC2)

Amazon EC2 (Elastic Compute Cloud) is a web service that allows you to run virtual servers in the cloud. It provides scalable, secure, and cost‑efficient compute capacity, allowing you to deploy applications without buying physical hardware.

### Why the name “EC2”?

* **Elastic** – You can increase or decrease the number or size of servers anytime.
* **Compute** – It provides processing power for applications.
* **Cloud** – All resources run on AWS cloud infrastructure.

### What is an EC2 Instance?

An **EC2 instance** is simply a virtual machine (VM) that runs on AWS. You can select the OS, hardware configuration, and storage type based on your needs.

---

#  Amazon Machine Image (AMI)

An **Amazon Machine Image (AMI)** is a pre-configured template used to create EC2 instances.

### What does an AMI include?

* Operating system (Ubuntu, Amazon Linux, Windows, etc.)
* Default software/applications
* Architecture (x86_64 / ARM)
* Storage mappings
* Launch permissions

### Key Points:

* You **must** choose an AMI when launching an instance.
* You can launch multiple identical instances from one AMI.
* You can also use different AMIs for different configurations.

---

#  Types of EC2 Instances

AWS categorizes EC2 instances based on CPU, memory, storage, and networking needs.

## 1️ General Purpose Instances

Balanced compute, memory, and storage.

**Best for:** Web apps, microservices, small databases.

**Examples:** t2, t3, t4g, m5, m6

---

## 2️ Compute Optimized Instances

Designed for high‑performance CPU operations.

**Best for:** Gaming servers, scientific modeling, batch processing.

**Examples:** c4, c5, c6

---

## 3️ Memory Optimized Instances

High RAM relative to CPU.

**Best for:** Large databases, caching servers, in‑memory analytics.

**Examples:** r4, r5, x1

---

## 4️ Storage Optimized Instances

Fast I/O performance + large storage capacity.

**Best for:** Big data, file storage, log processing.

**Examples:** i3, d3, h1

---

## 5️ Accelerated Computing Instances

Use specialized hardware like GPUs or FPGAs.

**Best for:** Machine learning, video rendering, 3D processing.

**Examples:** g4, p4, f1

---

#  Key Pair (SSH Keys)

A **key pair** is used to securely connect to EC2 instances without a password.

### Key pair consists of:

* **Public key** – stored on the instance
* **Private key (.pem)** – kept by the user

### Public Key

Acts like a “lock” installed on the server.

### Private Key

You use this to unlock and log in securely.

>  Never share private key.

---

#  Security Group

A **security group** acts as a virtual firewall for your instance.

### Two types of rules:

* **Inbound rules:** control incoming traffic
* **Outbound rules:** control outgoing traffic

### Example:

* Allow SSH (port 22) from your IP
* Block all unknown traffic

Security groups help ensure only trusted sources can connect.

---

#  EC2 Instance States

| State             | Meaning                                             |
| ----------------- | --------------------------------------------------- |
| **Pending**       | Instance is being created and initialized           |
| **Running**       | Instance is active and ready to use                 |
| **Stopping**      | Shutting down process has started                   |
| **Stopped**       | Instance is off but can restart; no compute charges |
| **Starting**      | Restarting from “Stopped” state                     |
| **Shutting‑down** | Instance is being terminated                        |
| **Terminated**    | Instance deleted permanently                        |
| **Hibernating**   | Instance RAM saved to EBS for fast resume           |

---

#  EC2 Pricing Models

AWS provides various cost options depending on flexibility and usage.

## 1️ On‑Demand

* Pay per hour/second
* No commitment
* Good for testing or unpredictable workloads

## 2️ Reserved Instances

* 1 or 3‑year commitment
* Up to **75% cheaper** than On‑Demand
* Payment options:

  * All upfront
  * Partial upfront
  * No upfront

## 3️ Savings Plans

* Commit to a fixed $/hour usage
* More flexible than Reserved Instances
* Works across regions & instance families

## 4️ Spot Instances

* Use unused AWS capacity at **up to 90% discount**
* Can be interrupted anytime
* Best for flexible, fault‑tolerant workloads

## 5️ Dedicated Hosts

* A full physical EC2 server dedicated to your account
* Useful for license‑specific workloads (Windows, SQL)

---

#  Remote Access: SSH vs RDP

##  SSH (Linux)

* Uses **port 22**
* Command‑line interface
* Technically advanced but powerful
* Example:

```
ssh -i mykey.pem ec2-user@<public-ip>
```

##  RDP (Windows)

* Uses **port 3389**
* Full graphical interface (GUI)
* Good for beginners

---
###  Common Ports

| Purpose | Port |
| ------- | ---- |
| SSH     | 22   |
| HTTP    | 80   |
| HTTPS   | 443  |
| RDP     | 3389 |


##  Amazon Machine Image (AMI)

An AMI defines the template used to launch EC2 instances. It acts like a snapshot of your machine that can be reused.

###  AMI Contains:

* Root OS image
* Software packages
* Boot instructions
* Block device mapping
* Permissions for launching

###  Types of AMIs

* AWS Provided AMIs
* Marketplace AMIs
* Custom AMIs (created from your running server)
* Community AMIs

##  Status Checks in EC2

AWS runs automatic health checks to ensure your instance is working properly.

### **1. System Status Check**

Checks AWS infrastructure (network outage, power failure, physical host health).

### **2. Instance Status Check**

Checks your instance-level issues:

* Boot errors
* OS corruption
* Network misconfiguration

### **3. EBS Volume Status Check**

Checks I/O performance and health of attached volumes.

---


