# Elastic Compute Cloud (EC2)

EC2 is a web service that provides resizable compute capacity in the cloud.
It's just a  virtual machine in the cloud and it reduces the time required to obtain and boot new servers to minutes allowing you to quickly scale capacity both up and down as you computing requirements change.

### EC2 options
- **On-Demand**:
  - Allows you to pay fixed rate by hour (or by seconds) with no commitment
  - Perfect for users that want the low cost and flexibility of Amazon EC2 without any upfront payment or long term commitment
  - Application with short term, spiky or unpredictable workloads that cannot be interrupted.
  - Applications being developed or tested on Amazon EC2 for the first time

- **Reserved**:
 - Provides you with a capacity reservation and offers significant discount on the hourly charge for an instance. 1 year or 3 year
 - Applications with steady state or predictable usage
 - Applications that require reserved capacity
 - Users can make upfront payment to reduce their total computing cost even further
  - Standard RI's (upto 75% off on-demand)
  - Convertable RI's (upto 54% off on-demand) features the ability to change the attributes of RI
  - Scheduled RI's are available to launch within the time window you reserve

- **Spot**:
- Allows you to bid whatever price you want for instance capacity providing for even greater savings if your instances have flexible start and end time
 - Applications that have flexible start and end time
 - Applications that are feasible at very low compute prices
 - Users with an urgent need for large amount of additional computing capacity

- **Dedicated Hosts**: 
- Physical EC2 servers decicated for your use. Dedicated hosts can help you reduce cost by allowing you to use your existing server bound software licenses
- Useful for regulatory requirements that do not allow multi-tenancy
- Can be purchesed on-demand (hourly)

### EC2 instance types
![EC2_Types](https://s3.amazonaws.com/hfcontents/kbimages/EC2_Types.png "EC2_Types")
FIGHT DR MC PX

### System Status Check
Checks wheather insatance is reachable
These checks monitor if AWS systems required to use this instance and ensure they are functioning properly
In case of failure reboot/terminate

### Instance Status Check
Makes sure you can get traffic to the operating system
These checks monitor your software and network configuration for this instance
In case of failure reboot the instance so that it gets launched on different host

### Security Group
 - Virtual firewall for your instances
 - One security group can be applied to multiple instances
 - All Inbound traffic is blocked by default
 - All Outbound traffic is allowed
 - Changes to security group take effect immediately
 - You can have any number of EC2 instances within security group
 - You can have multiple EC2 instances attached to security group
 - Security group are **STATEFUL**
   If you create an inbound rule allowing traffic in, that traffic is automatically allowed back out again
 - You cannot block specific IP address using security groups instead use Network Access Control List (STATELESS)
 - You can specify allow rules but not deny rules

# Elastic Block Storage (EBS)

### EBS Volume types
 - **General Purpose SSD (GP2)**
    - General purpose, balances both price and performance
    - Ratio of 3 IOPS/GB with up to 10k IPOS with the ability to burst upto 3k IOPS for extended period of time for volumes at 3334 GiB and above
 - **Provisioned IOPS SSD (IO1)**
    - Designed for IO intensive operations such as large relational or NoSQL DB's
    - Use if needed more than 10k IOPS
    - Provision upto 20k IOPS per volume
 - **Throughput optimized HDD (ST1)**
    - Before SSD we had magnetic storage volume
    - Big data
    - Log processing
    - Data warehousing
    - Cannot be a boot volume
 - **Cold HDD (SC1)**
    - Cannot be a root volume
    - Low cost storage for infrequently accessed workloads
    - File server
 - **Magnetic (Standard)**
    - Lowest cost per gigabyte of all EBS volume types that is bootable
    - These are ideal for workloads where data is accessed infrequently
    - Suitable for applications where the lowest storage cost is important

### Volumes and Snapshots
- Volumes exists on EBS
    - Virtual Hard Disk
- Snapshots exist on S3
- Snapshots are point in time copies of Volumes
- Snapshots are incremental - this means that only the blocks that have changed since the last snapshots are moved to S3
- **Security**
    - EBS root volumes of default AMI's cannot be encrypted
 Snapshots of encrypted volumes are going to be encrypted automatically
    - Volumes restored from encrypted snapshots are encrypted automatically
    - You can share snapshots only if they are encrypted

### Snaphsot of Root Device Volumes
- To take the snapshot of EBS volumes that serve as root device, you should stop the instance before taking the snapshot
- You can create AMI's from EBS backed instances and snapshots
- You can change EBS volume size and type on the fly (best practise is to stop the instance)
- EBS volume will always be in the same AZ as the instance
- To move EBS volume from one AZ to another, take a snaphot or image of it and then copy it to the new AZ/region

# Redundant Array of Independent Disk (RAID)
RAID arrays are used when you are not getting the desired IO

### Types of RAID array

- **RAID 0**:
  - Striped: Multiple disks striped (combined) togather to form a single volume
  - No Redundancy: If any of the volume fails whole RAID array would fail
  - Good Performance
  - Typically used in gaming
- **RAID 1**:
  - Mirrored: You got one disk and you mirror exact copy of it to another disk
  - Redundancy: If one disk fails you can still use the RAID array using another disk
  - **RAID 5:**
    - You get 3 disks or more and you are writing parity to another disk
	- Amazon discourages you from making RAID 5 array on EBS volumes
	- Parity is a checksum. If one of the disks fails, you can read the RAID array and using checksum you can find what the missing data is
- **RAID 10:**
  - Combination of RAID 0 and RAID 1.Striped and Mirrored. Good Redundancy and Good Performance

### Taking Sanpshot of RAID array
  - **Problem**
  Snapshot excludes data held in application and OS cache. This does not matter if using single volume RAID array. If using multiple multiple volumes in RAID array this can be problem due to independencies of RAID array
 - **Solution:**
    Stop application from writing to the disk, Flush all caches to the disk.
	This can be done using
	 - Freeze the file system
	 - Unmount RAID array
	 - Stop EC2 instance

### Volumes & Snapshots
 - You can create an image out of EC2 instance
 - You can take a snaphot of Volumes
 - You can create and image from Volume
 - You can transfer snapshot/volume from one region to another, make them public or transfer it to another AWS account
- AWS provided AMI's are not encrypted
- You can share snapshots only if they are encrypted
- Snapshots of encrypted volumes are encrypted automatically
- Volumes restored from encrypted snapshots are encrypted automatically

# Amazon Machine Image (AMI)

You can select AMI based on:
 - Region
 - Architecture (32/64 bit)
 - Operating System
 - Launch Permissions
 - Storage for Root Device
  - Instance backed (Ephemeral storage [not durable])
    - Cannot be stopped
	- If the host dies, you lose you lose your instance
	- Root device is created from template stored in S3
	- Provisioning is slow
	- With instance termination, by default, root volume will be deleted. There is no wy to tell AWS to keep root volume device
  - EBS backed
    - Can be stopped
	- Volume can be attached to another instance
	- Root device is created from an EBS snapshot
	- Provisioning is faster
	- With instance termination, by default, root volume will be deleted However you can tell AWS to keep root volume device

# Elastic Load Balancers (ELB)
 - Virtual applicance (machine)
 - Instances monitored by ELB are reported as InService or OutofService
 - Health checks check the instance health by talking to it
 - Have their own DNS. Their IP address is not published since it might change
 - Types

  - **Application Load Balancer (ALB)**
    - Best suited for load balancing HTTP/HTTPS traffic
	- Operates at layer 7 and are application aware (X-forwarded, Sticky sessions)
	  - X-Forwarded-For header: Forwards public IP of the client to EC2 instance passing through load balancers. Verious API's are available which can tell Country, Organisation where the Public IP request is coming from
![x_forwaded](https://s3.amazonaws.com/hfcontents/kbimages/X-Forwaded.png "x_forwaded")

  - ** Network Load Balancer (NLB)**
    - Best suited for load balancing TCP traffic where extreme performane is required
    - Operates at layer 4
	- Capable of handling millions of requests per second while maintaining ultra low latencies

  - **Classic Load Balancers (CLB)**
    - Legacy ELB
	- You can load balance both HTTP/HTTPS and TCP traffic
	- If application stops responding, ELB responds with 504 error (gateway timeout).
	  It means application is not responding within the ideal timeout period. This could be either at Web server layer or Database layer

# Cloud Watch
 - **Dashboards**: Creates awesome dashboards to see what is happening with your AWS environment
 - **Alarms**: allows you to set Alarms that notify you when perticular threasholds and hit
 - **Logs**: CLoudWatch logs helps you to aggregate monitor and store logs
 - Standard Monitoring is 5 minutes, Detailed Monitoring is 1 minute
 - **Events**:
   CloudWatch Events helps you to respond to state changes in your AWS resources. When your resources change state they automatically send events into an event stream. You can create rules that match selected events in the stream and route them to targets to take action. You can also use rules to take action on a pre-determined schedule. For example, you can configure rules to:
    - Automatically invoke an AWS Lambda function to update DNS entries when an event notifies you that Amazon EC2 instance enters the Running state
	- Direct specific API records from CloudTrail to a Kinesis stream for detailed analysis of potential security or availability risks
	- Take a snapshot of an Amazon EBS volume on a schedule

CloudWatch is for Monitoring and logging whereas AuditTrails is for Auditing (Role/S3/User creation etc...)

# Misc
  - One subnet One AZ
  - IAM Role had Policy (defining all the actions you can perform)
  - Role > Policy > Actions
  - If you planning to use roles CLI from aws instance you should use IAM role. If using from your local machine use credentials

# Explore
- SG - All Outbound traffic is allowed vs If you create an inbound rule allowing traffic in, that traffic is automatically allowed back out again
- EBS IOPS
- Why not always use RAID 10
- Using Cassandra with RAID array
- DMZ
- Cloud Watch custom alarms
