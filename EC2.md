# EC2 (Elastic Cloud Compute)
resizable compute capacity in the cloud (cloud based virtual machines). Allows you to quickly scale up and down.

Changed the startup scene by making it very cheap to get set up on machines. Fast and low cost because you don't have to buy and set up physical machines.

### EC2 Options
1. On Demand - fixed rate by the hour with no commitment
1. Reserved - provides a capacity reservation and offers a significant discount on the hourly charge for an instance. 1 or 3 year terms
1. Spot - enables you to bid whatever price you want for instance capacity, providing for even greater savings if your applications have flexible start and end times. (You are bidding for excess computing capacity)
1. Dedicated Hosts - Physical EC2 server dedicated for your use. Can help reduce costs when you have legacy server-bound software licenses.

##### On Demand
- You want low cost and flexibility with low up-front payment or long-term commitment
- Good for applications with short-term, spiky or unpredictable workloads that cannot be interrupted OR being developed/tested on EC2 for the first time

##### Reserved
- Applications with steady state or predictable usage
- Applications that require reserved capacity
- Able to make up front payments to reduce the total computing costs

##### Spot
- Applications that have flexible start and end times
- Applications that are only feasible at very low compute prices (Pharmacutical number crunching, etc)
- If an instance is terminated by Amazon because the spot price went above your bid price, you will not be charged for the partial hour of usage.
- If you terminate the instance yourself, you will be charged for any hour in which the instance ran


##### Dedicated Hosts
- Useful for regulatory requirements that may not support multi-tenant virtualization
- Great for licensing that does not support multi-tenancy or cloud deployments
- Can be purchased on-demand, or as a reservation for up to 70% off the on demand price


First character is "type"
Second character is "Generation"

|Family|Specialty|Use Case|
|------|---------|--------|
|D2|Dense Storage|Fileservers, Data Warehousing, Hadoop
|R4|Memory Optimized|Memory Intensive apps, DB|
|M4|General Purpose|Application Servers|
|C4|Compute Optimized|CPU Intensive Apps/DBs|
|G2|Graphics Intensive|Video Encoding/3D Application Streaming|
|I2|High Speed Storage|NoSQL DBs, Data Warehousing, etc|
|F1|Field Programmable Gate Array|Hardware acceleration for your code|
|T2|Lowest Cost, General Purpose|Web Servers, small DBs|
|P2|Graphics/General Purpose GPU|Machine Learning|
|X1|Memory Optimized|SAP HANA/Apache Spark, etc|

### EBS
- Allows you to create storage volumes and attach them to your EC2 instances
- Can create a file system on top of these volumes, run a database, etc (they are block devices)
- Are placed in a specific Availability Zone
- Are automatically replicated in same Availability Zone

##### EBS Types
1. General Purpose SSD (GP2)
  - General purpose, balances price and performance
  - Ratio of 3 Input/output operations per second (IOPS) per GB, up to 10k IOPS and ability to burst to 3k for extended periods of time for volumes under 1 Gib
1. Provisioned IOPS SSD(IO1)
  - Designed for I/O intensive applications such as large relational or NoSQL Databases
  - Use if you need more than 10k IOPS
  - Can provision up to 20k IOPS per volume
1. Throughput Optimized HDD (ST1)
  - Big Databases
  - Data Warehouses
  - Log processing
  - Any other data that is in sequence
  - Cannot be a boot volume (you can't boot from one)
1. Cold HDD (SC1)
  - Lowest Cost Storage for infrequently accessed workloads
  - File Servers
  - Cannot be a boot volume
1. Magnetic (Standard)
 - Lowest cost per gig that is still bootable
 - Ideal for workloads where data is accessed infrequently and you want it cheap as hell


 **You cannot mount 1 EBS volume to multiple EC2 instances. If you need a shared file system you need to use EFS**

Exam Tips:
- Termination protection is turned off by default
- On an EBS backed instance, the default action is for the volume to be deleted when the instance is deleted
- EBS Root volume is the volume that your EC2 instance boots from. You cannot encrypt a default AMI root volume. You can use third party tools to encrypt the root volume (bit locker). Additional volumes can always be encrypted.


#### Security Groups
- Virtual firewall that controls access to your EC2 instances
- One instance can have multiple security Groups (many to many relationship between security groups and ec2 instances)
   - Have rules that allows traffic to and from instances
     - Any security rule change is applied IMMEADIATELY
     - If you add an inbound rule, it will automatically open it as outbound as well (security groups are stateful)
     - All inbound traffic is blocked by default
     - All outbound traffic is allowed by default
     - When you have an instance with multiple security groups, the results are additive

** You cannot block specific IP addresses using security groups, you have to use Network Access Control Lists **

#### Volumes and Snapshots
- Volumes exist on EBS (Elastic Block Storage)
  - Volumes are virtual hard disks
  - Must be in same Availability Zone as EC2 instance in order to attach
- Snapshots are point in time copies of Volumes
  - Stored on S3
  - Are incremental, only the blocks that have changed since your last snapshot are moved to S3
  - First snapshot may take some time to create but later ones will be faster

#### RAID, Volumes and Snapshots
Redundant Array of Independent disks
- RAID 0 : striped, no redundancy, good performance
- RAID 1 : mirrored, redundancy
- RAID 5 : good for reads, bad for writes, AWS does not recommend ever putting RAID 5's on EBS
- RAID 10 : striped and mirrored, good redundancy, good performance

You would typically use RAID 0 or RAID 10 to get better disk IO on a database not supported by AWS

Problem: When you try to take a snapshot of a RAID array, the snapshot excludes data held in the cache by applications and the OS. This tends not to matter on a single volume, but with RAID, this causes issues due to interdependencies of the array.

Solution: Take application specific snapshot. Stop all applications from writing to disk and flush all caches.
- Freeze the file system
- Unmount the RAID array
- Shut down the associated EC2 instance (easiest)

#### Amazon Machine Image (AMI)
Provides the information needed to launch a virtual server in the cloud
1. Template for root volume for the instance (i.e. the OS, an app server, and applications)
1. Launch permissions that control which AWS accounts can use the AMI to launch instances
1. A block device mapping that specifies the volumes to attach to the instance with it is launched

** Review AMI hardening and cleanup requirements for exam **

Exam Tips:
- AMIs are regional. You can only launch and AMI from the region in which it is stored. You can copy AMIs to other regions using the console or command line
- AMIs are stored in S3

#### AMI Types
- EBS
  - Root device for instance is Amazon EBS volume created from an Amazon EBS snapshot
  - Used by most AMIs
  - Can be stopped, you won't lose data if it is stopped
  - Root volume is deleted by default when instance is deleted, but you can tell AWS not to do this
- Instance Store
  - Root device for instance is an instance store volume created from a template stored in S3
  - Cannot stop/start the instance, only reboot and terminate (no data loss on reboot)
  - If underlying host fails, you will lose your data
  - Sometimes called Ephemeral Storage
  - Root volume is always deleted when deleting the instance

#### Elastic Load Balancers
- Instance monitored by ELB are reported as InService or OutOfService
- Health Checks check the instance health by talking to it (you define what address to hit)
- Have their own DNS name, you are never given an IP address

** Read ELB FAQ for Classic Load Balancers **

#### CloudWatch
Found under management tools (do not confuse with CloudTrail or cloudFormation)
- Can build dashboards that display different metrics from your EC2 instances, S3 buckets, etc
- Standard monitoring (enabled by default) = poll every 5 minutes
- Detailed monitoring (manually enabled when you create the EC2 instance)= poll every 1 minute
- Alarms: alert based on metrics (Like CPU Utilization)
  - Could send email when over CPU threshold
  - Could stop or terminate instance when under CPU utilization threshold
- Events: respond to state changes in an AWS resource
- Logs: helps aggregate, monitor, and store logs

#### Identity Access Management (IAM) Roles
- Roles are more secure than storing your access key and secret access key on individual EC2 instances
- Roles are easier to manage (you can change permissions afterward and don't have to worry about changing access keys)
- Roles can only be assigned when that EC2 instance is being provisioned (they can be modified and deleted however)
- Roles are universal, you can use them in any region

#### Bash Scripting
- you can specify a bash script to be executed on your EC2 instances when it is allocated
