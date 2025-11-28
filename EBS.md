## Elastic Block Storage   

##  What is Amazon EBS
Amazon Elastic Block Store (EBS) provides persistent block storage volumes for EC2 instances. Unlike Instance Store, EBS retains data even if the EC2 instance stops or restarts. Think of EBS as a highly durable cloud SSD/HDD that you can attach, detach, resize, back up, and encrypt.

##  Key Characteristics of EBS
- **Persistent**: Data remains even after instance stops.
- **Independent Scaling**: You can increase storage without changing compute.
- **Highly Durable**: Replicated automatically within same AZ.
- **Low Latency**: Optimized for fast I/O.
- **Secure**: Supports full encryption.
- **Flexible**: Resize, change type, or tune performance anytime.

---
#  EBS Volume Types 
EBS volumes are divided into SSD-based and HDD-based categories.

##  1. SSD Volumes (Optimized for IOPS)
Designed for fast, random I/O operations.

### General Purpose SSD (gp2)
- Default for most workloads.
- Baseline performance: **3 IOPS per GB**.
- Max: **16,000 IOPS**.
- Ideal for boot volumes, web servers, dev/test, small databases.

### General Purpose SSD (gp3)
- Next-gen SSD volume type.
- Performance independent of size.
- You can manually set:
  - Up to **16,000 IOPS**
  - Up to **1,000 MB/s throughput**
- Best for cost-effective scaling.

### Provisioned IOPS SSD (io1 / io2)
- Designed for critical, I/O-intensive workloads.
- io2 offers **99.999% durability**.
- Supports **up to 64,000 IOPS**.
- Used for high-performance DB systems like:
  - MySQL
  - Oracle
  - SQL Server
  - SAP HANA

### io2 Block Express
- Future-grade EBS performance.
- Features:
  - **4,000 MB/s throughput**
  - **256,000 IOPS**
  - Ultra-low latency (<1 ms)
- Suitable for enterprise-grade databases.

---
##  2. HDD Volumes (Optimized for Throughput)
Used for workloads requiring sequential access.

###  Throughput Optimized HDD (st1)
- Designed for big data, streaming logs, analytics.
- Offers up to **500 MB/s throughput**.
- Best for:
  - Data warehouses
  - MapReduce jobs
  - Log processing

###  Cold HDD (sc1)
- Lowest cost storage option.
- Used for infrequently accessed workloads.
- Ideal for archival data.

---
##  3. Magnetic Standard 
- No longer recommended.
- Very slow performance.
- Used only for backward compatibility.

---
#   EBS Points 
- EBS is AZ-scoped → you cannot attach a volume to EC2 in another AZ.
- EBS volumes can be dynamically reconfigured (change type, expand size).
- EBS uses **network-attached storage**, not physically local to EC2.
- EBS is suitable for OS-level file systems (ext4, xfs).
- EBS has extremely high durability because data is replicated across multiple servers automatically.

---

##  EBS Snapshots 
EBS Snapshots are incremental, point-in-time backups of your EBS volumes. They are internally stored in Amazon S3.

##  How Snapshots Work
- First snapshot = full backup.
- Future snapshots = only changed blocks are saved.
- Snapshots can be used to create volumes in the same or different regions.

##  Snapshot Use Cases
- Disaster recovery
- Migrating across Availability Zones
- AMI creation
- Versioning for data
- Cloning production volumes for testing

##  Snapshot Lifecycle 
AWS Data Lifecycle Manager (DLM) allows you to automate:
- Snapshot creation
- Retention policy
- Scheduled backups

---
#  EBS Encryption 
AWS encrypts:
- Data at rest (stored on disk)
- Data in transit (between EC2 ↔ EBS)
- Snapshots
- Volumes restored from snapshots

##  Encryption Using KMS
- Uses AWS KMS keys (AWS-managed or customer-managed).
- Zero overhead on modern EC2 instances.
- Mandatory in regulated industries.

##  Benefits of EBS Encryption
- No performance loss
- End-to-end protection
- Simplifies compliance
- Easy rotation of keys

---
#  Partitioning & File System 
When We attach a new EBS volume, it is raw and unformatted.

##  Step 1: View volumes
```
lsblk

```

##  Step 2: Partition the disk
```
sudo fdisk /dev/xvdf ,/dev/nvme1n1
          - n-create new partitions
          - d-delete partitions
          - p-print partition table
          - w-write changed and exit
          - q-quit

```

##  Step 3: Format with filesystem
```
sudo mkfs -t ext4 /dev/xvdf1 , /dev/nvme1n1

```

##  Step 4: Mount the volume
```
sudo mkdir /data
sudo mount /dev/xvdf1 /data , /dev/nvme1n1 /data

```

##  Step 5: Verify
```
df -h
```

##  Step 6: Permanent Mount
```
To ensure the partition mounts automatically after a reboot, update the `/etc/fstab` file.

           1. Get the UUID of the Partition:
            
              sudo blkid /dev/xvdf1
              - Copy the UUID from the output.
           
           2. Edit `/etc/fstab`:
              sudo vi /etc/fstab
           - Add an entry for the new partition. Use the UUID from the previous step. For example:
             
                UUID=your-uuid-here /mnt/mydata ext4 defaults,nofail 0 2
               
           - Replace `your-uuid-here` with the actual UUID and adjust the file system type if 
                 necessary.
               /dev/xvdf1 /data ext4 defaults,nofail 0 2
              /dev/nvme1n1 /data ext4 defaults,nofail 0 2
```
##  Step 7: Unmounting  EBS Volume (`umount`)
```
sudo umount  /dev/xvdf1 /data  
sudo umount /dev/nvme1n1 /data
```
---








